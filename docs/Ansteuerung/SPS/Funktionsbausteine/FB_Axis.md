# FB_Axis

## Einsatzbereich
Der Funktionsbaustein *FB_Axis* kapselt die Ansteuerung einer kontinuierlichen Achse auf Basis der TwinCAT-Motion-Control-Funktionsblöcke `MC_Power`, `MC_Halt`, `MC_Stop`, `MC_Reset` und `MC_MoveAbsolute` aus der Bibliothek *TE1000 Motion Control 2 (Tc2_MC2)*.

Er wird von einem übergeordneten Baustein (z.B. Zustandsautomat) verwendet und stellt dafür Methoden zum Ein- und Ausschalten, Stoppen, Halten, Bewegen und Rücksetzen der Achse bereit.

Sicherheitsrelevante Begrenzungen (z.B. maximale Position, Geschwindigkeit, Beschleunigung) werden ausschließlich über die Achskonfiguration im TwinCAT-Projekt realisiert und sind nicht Bestandteil dieses Bausteins.

## Schnittstelle

### Deklaration
```
FUNCTION_BLOCK FB_Axis
VAR_INPUT
    Axis : REFERENCE TO AXIS_REF;    // Referenz auf die Achse
END_VAR
VAR_OUTPUT
END_VAR
VAR
    fbPower              : MC_Power;          // Achs-Softwarefreigabe
    stPowerReturn        : ST_McOutputs;      // Status von fbPower

    fbHalt               : MC_Halt;           // Achsstopp (kontrolliert)
    stHaltReturn         : ST_McOutputs;      // Status von fbHalt

    fbStop               : MC_Stop;           // Stopp mit Abschalten
    stStopReturn         : ST_McOutputs;      // Status von fbStop

    fbReset              : MC_Reset;          // Reset der Achse
    stResetReturn        : ST_McOutputs;      // Status von fbReset

    fbMoveAbsolute       : MC_MoveAbsolute;   // Absolutpositionierung
    stMoveAbsoluteReturn : ST_McOutputs;      // Status von fbMoveAbsolute
END_VAR
```

Die interne Instanzierung der MC-Funktionsblöcke sowie deren Statusstrukturen ist nach außen gekapselt.

### Eingänge

```
Axis : REFERENCE TO AXIS_REF;
```

Referenz auf die zugehörige Achse im TwinCAT-Motion-System. Der Baustein führt zu Beginn eine Referenzprüfung durch (`__ISVALIDREF(Axis)`), bei ungültiger Referenz wird die Abarbeitung abgebrochen.

Es existieren keine weiteren direkten Ein- oder Ausgänge. Die Bedienung erfolgt ausschließlich über Methoden und Eigenschaften.

## Bewegungsparameter

```
TYPE ST_MoveParameter :
STRUCT
    fPosition : LREAL;                   // Absolute Zielposition
    fVelocity : LREAL;                   // Verfahrgeschwindigkeit
    fAccel    : LREAL := 1E300;          // Beschleunigung
    fDecel    : LREAL := 1E300;          // Verzögerung
    fJerk     : LREAL := 1E300;          // Ruck
END_STRUCT
END_TYPE
```
Der Typ ST_MoveParameter fasst alle Parameter für eine absolute Positionierung zusammen: Zielposition, Geschwindigkeit, Beschleunigung, Verzögerung und Ruck. Für Beschleunigung, Verzögerung und Ruck sind Default-Werte vorbelegt. Diese können aber pro Fahrbefehl überschrieben werden.

## Eigenschaften
### P_Power
Diese Eigenschaft stellt einen Referenzzugriff auf die Statusstruktur des internen *MC_Power*-Funktionsbausteins bereit.

Über P_Power können z.B. *Done-, Busy-, Active-* und *Error-Status* für die Achsfreigabe aus einem übergeordneten Baustein ausgelesen werden, ohne die internen MC-Instanzen direkt zu kennen.

### P_MoveAbsolute
Diese Eigenschaft stellt den Status der letzten Absolutfahrt über *MC_MoveAbsolute* bereit.

Typische Nutzung ist die Auswertung von *Done-, Busy-* oder *Error-Flags* einer Positionierbewegung im höhergeordneten Baustein.

## Methoden
Alle Methoden sind als Schnittstelle für einen übergeordneten Baustein gedacht und werden von diesem in der Ablaufsteuerung (z.B. Zustandsautomat) aufgerufen.

### M_PowerOn
```
METHOD M_PowerOn : E_MethodState
VAR_INPUT
    fOverride : LREAL;
END_VAR
```

**Funktion:**

Aktiviert die Achse über *MC_Power* und setzt *Enable, Enable_Positive* und *Enable_Negative* auf TRUE. Der Override wird mit `LIMIT(0, fOverride, 100)` begrenzt und dem *MC_Power*-Baustein zugewiesen.

**Rückgabewerte (E_MethodState):**

- `E_StateBusy`: Achsfreigabe wird aufgebaut.
- `E_StateDone`: Achse ist erfolgreich eingeschaltet (`fbPower.Status = TRUE`).
- `E_StateError`: Fehler im *MC_Power*-Baustein (`fbPower.Error = TRUE`).​

### M_PowerOff
```
METHOD M_PowerOff : E_MethodState
VAR_INPUT
END_VAR
```

**Funktion:**

Deaktiviert die Achse, indem *Enable*-Signale von *MC_Power* zurückgenommen werden.

**Rückgabewerte:**

- `E_StateBusy`: Ausschaltvorgang läuft.
- `E_StateDone`: Achse ist deaktiviert (`NOT fbPower.Status`).
- `E_StateError`: Fehler im *MC_Power*-Baustein.

### M_MoveAbsolute
```
METHOD M_MoveAbsolute : E_MethodState
VAR_INPUT
    stMoveParameter : ST_MoveParameter;
END_VAR
```

**Funktion:**

Führt eine absolute Positionierfahrt mit den Parametern aus `stMoveParameter` aus. Vor dem neuen Befehl werden Rückgabestrukturen für Power und MoveAbsolute zurückgesetzt, ein eventuell aktiver vorheriger Move-Befehl wird überschrieben.

**Ablauf (vereinfacht):**

- Prüfen, ob bereits ein Move-Befehl aktiv ist (`P_MoveAbsolute.Done`).
- Warten, bis Move-Befehl abgeschlossen ist oder alten Befehl mit `Execute := FALSE` zurückgesetzt, anschließend neuen Befehl mit `Execute := TRUE` und Parametern aus `stMoveParameter`.
- `BufferMode` wird auf *MC_Aborting* gesetzt, wodurch laufende Befehle abgebrochen und durch den neuen Befehl ersetzt werden.

**Rückgabewerte:**

- `E_StateBusy`: Bewegung läuft.
- `E_StateDone`: Zielposition erreicht (`fbMoveAbsolute.Done`).
- `E_StateError`: Fehler im Bewegungsbefehl (z.B. Motion-Fehler, falscher Zustand).

### M_Halt
```
METHOD M_Halt : E_MethodState
VAR_INPUT
    fDecel : LREAL := 1E300;
    fJerk  : LREAL := 1E300;
END_VAR
```

**Funktion:**

Führt einen kontrollierten Halt über *MC_Halt* aus, mit optionaler Vorgabe von Verzögerung und Ruck. Ist noch kein Haltbefehl aktiv, werden Statusstrukturen zurückgesetzt und *Execute* gesetzt.

**Rückgabewerte:**

- `E_StateBusy`: Halt wird ausgeführt.
- `E_StateDone`: Halt abgeschlossen (`fbHalt.Done`).
- `E_StateError`: Fehler beim Haltbefehl (`fbHalt.Error`).

### M_Stop
```
METHOD M_Stop : E_MethodState
VAR_INPUT
    fDecel : LREAL := 1E300;
    fJerk  : LREAL := 1E300;
END_VAR
```

**Funktion:**

Führt einen Stopp über *MC_Stop* aus. Typischerweise mit zusätzlichem Deaktivieren der Achse. Es werden *Decel* und *Jerk* gesetzt, Rückgabestrukturen zurückgesetzt und anschließend *Execute* gesetzt, sofern noch kein Stop-Befehl aktiv ist.

**Rückgabewerte:**

- `E_StateBusy`: Stopp läuft.
- `E_StateDone`: Stopp abgeschlossen (`fbStop.Done`).
- `E_StateError`: Fehler beim Stop-Befehl (`fbStop.Error`).

### M_Reset
```
METHOD M_Reset : E_MethodState
VAR_INPUT
    bQuickStop : BOOL;
END_VAR
```

**Funktion:**

Kapselt den Reset der Achse über *MC_Reset*. Vor Ausführung werden alle Statusstrukturen (*Power, Reset, Halt, Stop* und *MoveAbsolute*) mit `MEMSET` zurückgesetzt. Die Entscheidung, wann ein Reset bei Fehlern tatsächlich ausgelöst wird, liegt im übergeordneten Baustein, der *M_Reset* aufruft.

**Logik:**

- Wenn `bQuickStop = TRUE` oder ein Achsfehler anliegt (`Axis.Status.Error`), wird `fbReset.Execute := TRUE` gesetzt.
- Liegt kein Fehler an und `bQuickStop = FALSE`, wird `E_StateError` zurückgegeben.​

**Rückgabewerte:**

- `E_StateBusy`: Reset wird ausgeführt.
- `E_StateDone`: Reset abgeschlossen oder kein Fehler mehr an der Achse.
- `E_StateError`: Fehler beim Reset oder ungültige Aufrufbedingung.

### M_Clear
```
METHOD M_Clear : E_MethodState
VAR_INPUT
END_VAR
```

**Funktion:**

Setzt interne Execute-Bits der MC-Funktionsblöcke (*Reset, Halt, Stop* und *MoveAbsolute*) zurück und markiert den Vorgang als abgeschlossen. Diese Methode ist typischerweise für das Aufräumen nach Befehlsfolgen im übergeordneten Zustandsautomaten vorgesehen.

## Verwendung
Ein übergeordneter Zustandsautomat könnte den Baustein wie folgt verwenden:

1. **Achsfreigabe:** Aufruf von `FB_Axis.M_PowerOn(fOverride := 100.0)` bis `E_StateDone`.
2. **Positionieren:** Füllen von `stMoveParameter` mit Zielposition und Dynamikwerten. Aufruf von `FB_Axis.M_MoveAbsolute(stMoveParameter := ...)` und Auswertung von `E_MethodState` sowie `P_MoveAbsolute`.
3. **Halt/Stop bei Bedarf:** Bei kontrolliertem Halt `M_Halt(fDecel, fJerk)`. Bei Stopp mit Abschalten ``M_Stop(fDecel, fJerk)`` und anschließend `M_PowerOff`.
4. Fehlerbehandlung: Fehlererkennung über Eigenschaften `P_Power` und `P_MoveAbsolute` im höheren Baustein. Auslösung von `M_Reset(bQuickStop := TRUE/FALSE)` entsprechend der gewünschten Strategie.

Neue Befehle können laufende Befehle bewusst überschreiben, da BufferMode z.B. auf *MC_Aborting* gesetzt ist und Execute-Bits entsprechend neu gesetzt werden. Der übergeordnete Baustein ist dafür verantwortlich, diese Logik sinnvoll zu steuern.
