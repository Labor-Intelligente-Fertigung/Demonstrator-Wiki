# FB_Mover

## Einsatzbereich
Der Funktionsbaustein *FB_Mover* kapselt die Ansteuerung eines einzelnen XTS-Movers als TwinCAT-Motion-Achse. Er verwendet die Funktionsblöcke `MC_Power`, `MC_Reset`, `MC_AddAxisToGroup`, `MC_Halt`, `MC_HaltCA` und `MC_MoveAbsoluteCA`.

Neben dem Ein- und Ausschalten sowie dem Rücksetzen des Movers unterstützt der Baustein absolute und modulo-basierte Positionierungen, kontrolliertes Halten, Collision-Avoidance-Halten und das Hinzufügen des Movers zu einer Collision-Avoidance-Gruppe. Zusätzlich verwaltet er einfache Produktinformationen für den Mover.

Die Bahn- und Collision-Avoidance-Konfiguration sowie sicherheitsrelevante Begrenzungen werden außerhalb dieses Bausteins in der TwinCAT-/XTS-Konfiguration festgelegt.

## Schnittstelle

### Deklaration
```
FUNCTION_BLOCK FB_Mover
VAR_INPUT
    nMoverID     : UDINT;                  // Mover ID
    refMover     : REFERENCE TO AXIS_REF;  // Reference to the mover axis
    fTrackLength : LREAL;                  // Track length used for modulo moves
END_VAR
VAR_OUTPUT
END_VAR
VAR
    refGroupIntern                 : REFERENCE TO AXES_GROUP_REF;
    fbPower                        : MC_Power;
    stPowerReturn                  : ST_McOutputs;
    fbReset                        : MC_Reset;
    stResetReturn                  : ST_McOutputs;
    fbAddAxisToGroup               : MC_AddAxisToGroup;
    stAddAxisToGroupReturn         : ST_McOutputs;
    fbRemoveAxisFromGroup          : MC_RemoveAxisFromGroup;
    stRemoveAxisFromGroupReturn    : ST_McOutputs;
    fbHalt                         : MC_Halt;
    stHaltReturn                   : ST_McOutputs;
    fbHaltCA                       : MC_HaltCA;
    stHaltCaReturn                 : ST_McOutputs;
    fbMoveAbsoluteCa               : MC_MoveAbsoluteCA;
    stMoveAbsoluteCaReturn         : ST_McOutputs;
    bProductOnMover                : BOOL;
    sLetterOnMover                 : STRING(1);
END_VAR
```

Die internen Motion-Control-Instanzen und Statusstrukturen sind nach außen gekapselt. Es existieren keine direkten Ausgänge; Bedienung und Diagnose erfolgen über Methoden und Eigenschaften.

### Eingänge

#### nMoverID

```
nMoverID : UDINT;
```

Eindeutige Kennung des Movers. Beim Hinzufügen zu einer Collision-Avoidance-Gruppe wird sie mit `UDINT_TO_IDENTINGROUP` in die Gruppenkennung `IdentInGroup` umgewandelt.

#### refMover

```
refMover : REFERENCE TO AXIS_REF;
```

Referenz auf die NC-Achse des Movers. Der zyklische Bausteincode prüft die Referenz mit `__ISVALIDREF(refMover)`. Bei gültiger Referenz wird außerdem `refMover()` zyklisch aufgerufen, um den Achsstatus zu aktualisieren.

#### fTrackLength

```
fTrackLength : LREAL;
```

Gesamtlänge der XTS-Strecke. Der Wert wird in `M_MoveModulo` verwendet, um eine modulo-basierte Zielposition in eine absolute Zielposition umzurechnen.

## Bewegungsparameter

```
TYPE ST_MoveParameter :
STRUCT
    fPosition         : LREAL;                           // Target position on the track
    fDistance         : LREAL;                           // Relative distance
    fVelocity         : LREAL;                           // Mover velocity
    fAccel            : LREAL := P_XTS.MC_DEFAULT;       // Acceleration
    fDecel            : LREAL := P_XTS.MC_DEFAULT;       // Deceleration
    fJerk             : LREAL := P_XTS.MC_DEFAULT;       // Jerk
    fGap               : LREAL;                          // Gap between movers in mm
    eDirection         : Tc2_Mc2.MC_DIRECTION := MC_Positive_Direction; // Driving direction
    bContinuousUpdate  : BOOL := TRUE;                   // Continuously update command values
END_STRUCT
END_TYPE
```

Für `M_MoveAbsolute` und `M_MoveModulo` werden insbesondere Position, Geschwindigkeit, Beschleunigung, Verzögerung, Ruck, Collision-Avoidance-Abstand und `bContinuousUpdate` verwendet. `M_MoveModulo` wertet zusätzlich `eDirection` aus. `fDistance` wird von diesen beiden Methoden nicht verwendet.

## Eigenschaften

### P_AxisStatus

```
PROPERTY P_AxisStatus : REFERENCE TO ST_AxisStatus
```

Stellt einen Referenzzugriff auf `refMover.Status` bereit. Darüber können unter anderem Achsfehler, Stillstand und die Aktivierung des externen Sollwertgenerators ausgewertet werden.

### P_CurrAbsolutePos

```
PROPERTY P_CurrAbsolutePos : LREAL
```

Liefert die aktuelle absolute Istposition aus `refMover.NcToPlc.ActPos`.

### P_CurrModuloPos

```
PROPERTY P_CurrModuloPos : LREAL
```

Liefert die aktuelle Modulo-Istposition aus `refMover.NcToPlc.ModuloActPos`.

### P_CurrTurns

```
PROPERTY P_CurrTurns : DINT
```

Liefert die aktuelle Anzahl vollständiger Umläufe aus `refMover.NcToPlc.ModuloActTurns`.

### P_GetVelo

```
PROPERTY P_GetVelo : LREAL
```

Liefert die aktuelle Sollgeschwindigkeit aus `refMover.NcToPlc.SetVelo`.

### P_Halt

```
PROPERTY P_Halt : REFERENCE TO Tc2_MC2.ST_McOutputs
```

Stellt die Statusstruktur des internen `MC_Halt`-Bausteins bereit. Typische Signale sind `Done`, `Busy`, `Active`, `CommandAborted`, `Error` und `ErrorID`.

### P_HaltCa

```
PROPERTY P_HaltCa : REFERENCE TO Tc2_MC2.ST_McOutputs
```

Stellt die Statusstruktur des internen `MC_HaltCA`-Bausteins bereit.

### P_MoveModulo

```
PROPERTY P_MoveModulo : REFERENCE TO Tc2_MC2.ST_McOutputs
```

Stellt die Statusstruktur von `MC_MoveAbsoluteCA` bereit. Da sowohl `M_MoveAbsolute` als auch `M_MoveModulo` dieselbe interne Instanz verwenden, enthält die Eigenschaft den Status des zuletzt über diese Instanz ausgeführten Bewegungsbefehls.

### P_ProductOnMover

```
PROPERTY P_ProductOnMover : BOOL
```

Les- und schreibbare Kennzeichnung, ob sich ein Produkt auf dem Mover befindet.

### P_LetterOnMover

```
PROPERTY P_LetterOnMover : STRING(1)
```

Les- und schreibbare Produktinformation für einen einzelnen Buchstaben auf dem Mover.

## Methoden

Alle Methoden dienen als Schnittstelle für einen übergeordneten Zustandsautomaten. Die internen Motion-Control-Funktionsblöcke werden zusätzlich im Hauptteil von `FB_Mover` zyklisch aufgerufen.

### M_PowerOn

```
METHOD M_PowerOn : E_MethodState
VAR_INPUT
    fOverride : LREAL;
END_VAR
```

**Funktion:**

Aktiviert den Mover über `MC_Power`. `Enable`, `Enable_Positive` und `Enable_Negative` werden gesetzt. Der Override wird mit `LIMIT(0, fOverride, 100)` auf 0 bis 100 Prozent begrenzt.

**Rückgabewerte:**

- `E_StateBusy`: Freigabe wird aufgebaut.
- `E_StateDone`: Mover ist freigegeben (`fbPower.Status = TRUE`).

### M_PowerOff

```
METHOD M_PowerOff : E_MethodState
VAR_INPUT
END_VAR
```

**Funktion:**

Nimmt die drei Enable-Signale von `MC_Power` zurück.

**Rückgabewerte:**

- `E_StateBusy`: Ausschaltvorgang läuft.
- `E_StateDone`: Mover ist nicht mehr freigegeben (`NOT fbPower.Status`).

### M_Reset

```
METHOD M_Reset : E_MethodState
VAR_INPUT
    bQuickStop : BOOL;
END_VAR
```

**Funktion:**

Setzt den Mover über `MC_Reset` zurück. Vor dem Reset werden die internen Statusstrukturen für Power, Reset, Gruppenoperationen, Halt und Bewegung mit `MEMSET` gelöscht.

Der Reset wird ausgelöst, wenn `bQuickStop = TRUE` ist oder `refMover.Status.Error` einen Achsfehler meldet. Liegt keine dieser Bedingungen vor, liefert die Methode `E_StateError`.

**Rückgabewerte:**

- `E_StateBusy`: Reset läuft.
- `E_StateDone`: Reset wurde erfolgreich abgeschlossen.
- `E_StateError`: Resetfehler oder ungültige Aufrufbedingung.

### M_AddToGroup

```
METHOD M_AddToGroup : E_MethodState
VAR_INPUT
    bExecute : BOOL;
    refGroup : REFERENCE TO AXES_GROUP_REF;
END_VAR
```

**Funktion:**

Fügt den Mover über `MC_AddAxisToGroup` einer Collision-Avoidance-Gruppe hinzu. Ein interner Zustandsautomat startet den Vorgang auf der steigenden Flanke von `bExecute`, übernimmt `refGroup` in `refGroupIntern` und verwendet `nMoverID` als `IdentInGroup`.

Nach `Done` oder `Error` muss `bExecute` auf `FALSE` gesetzt werden. Dadurch wird die Methode für einen neuen Auftrag zurückgesetzt und liefert zunächst wieder `E_StateUndifine`.

**Rückgabewerte:**

- `E_StateBusy`: Mover wird hinzugefügt.
- `E_StateDone`: Mover wurde erfolgreich hinzugefügt.
- `E_StateError`: Ungültige Mover-Referenz oder Fehler von `MC_AddAxisToGroup`.
- `E_StateUndifine`: Auftrag wurde nach Rücknahme von `bExecute` zurückgesetzt.

### M_MoveAbsolute

```
METHOD M_MoveAbsolute : E_MethodState
VAR_INPUT
    stMoveParameter : ST_MoveParameter;
END_VAR
```

**Funktion:**

Führt mit `MC_MoveAbsoluteCA` eine absolute Collision-Avoidance-Positionierung aus. Vor dem neuen Auftrag wird die Statusstruktur gelöscht und ein alter Befehl mit `Execute := FALSE` zurückgesetzt. Anschließend werden Position, Dynamikwerte, Mindestabstand und `bContinuousUpdate` übernommen und der neue Befehl gestartet.

**Rückgabewerte:**

- `E_StateBusy`: Bewegung läuft.
- `E_StateDone`: Zielposition wurde erreicht.
- `E_StateError`: Ein alter Befehl ist noch aktiv oder der Motion-Befehl meldet einen Fehler.

### M_MoveModulo

```
METHOD M_MoveModulo : E_MethodState
VAR_INPUT
    stMoveParameter : ST_MoveParameter;
END_VAR
```

**Funktion:**

Rechnet eine Zielposition auf der zyklischen XTS-Strecke in eine absolute Zielposition um und führt anschließend `MC_MoveAbsoluteCA` aus.

**Ablauf (vereinfacht):**

- Aktuelle Modulo-Sollposition, Umlaufzahl und absolute Sollposition lesen.
- Zielposition mit `ModAbs` und `ModTurns` auf die Streckenlänge beziehen.
- Absolute Zielposition für positive und negative Fahrtrichtung berechnen.
- Entsprechend `eDirection` die positive, negative oder kürzeste Fahrstrecke auswählen.
- Bewegung mit den Werten aus `stMoveParameter` über `MC_MoveAbsoluteCA` starten.

Für den Positionsvergleich wird zusätzlich `P_XTS.POSITION_WINDOW` berücksichtigt. Eine gültige, von null verschiedene `fTrackLength` ist Voraussetzung für die Modulo-Berechnung.

**Rückgabewerte:**

- `E_StateBusy`: Bewegung läuft.
- `E_StateDone`: Zielposition wurde erreicht.
- `E_StateError`: Ein alter Befehl ist noch aktiv oder der Motion-Befehl meldet einen Fehler.

### M_Halt

```
METHOD M_Halt : E_MethodState
VAR_INPUT
    fDecel : LREAL := 1E300;
    fJerk  : LREAL := 1E300;
END_VAR
```

**Funktion:**

Führt einen kontrollierten Halt des Movers über `MC_Halt` aus. Verzögerung und Ruck werden vor dem Start übernommen. Beim Start werden die Statusstrukturen von Power und Halt zurückgesetzt.

**Rückgabewerte:**

- `E_StateBusy`: Halt läuft.
- `E_StateDone`: Halt wurde abgeschlossen.
- `E_StateError`: Fehler beim Haltbefehl.

### M_HaltCa

```
METHOD M_HaltCa : E_MethodState
VAR_INPUT
    fDecel : LREAL := 1E300;
    fJerk  : LREAL := 1E300;
    fGap   : LREAL := 100;
END_VAR
```

**Funktion:**

Führt über `MC_HaltCA` einen Collision-Avoidance-Halt mit vorgegebenem Mindestabstand aus. Der Befehl wird nur gestartet, wenn `refMover.Status.ExtSetPointGenEnabled = TRUE` ist.

**Rückgabewerte:**

- `E_StateBusy`: Collision-Avoidance-Halt läuft.
- `E_StateDone`: Halt wurde abgeschlossen.
- `E_StateError`: Motion-Fehler oder externer Sollwertgenerator ist nicht aktiviert.

### M_Clear

```
METHOD M_Clear : E_MethodState
VAR_INPUT
END_VAR
```

**Funktion:**

Setzt die `Execute`-Signale für Reset, Gruppenoperationen, Halt und Bewegung zurück. Die Methode dient zum vollständigen Aufräumen interner Befehle und liefert anschließend `E_StateDone`.

### M_ClearHalt

```
METHOD M_ClearHalt : E_MethodState
VAR_INPUT
END_VAR
```

**Funktion:**

Setzt ausschließlich die `Execute`-Signale von `MC_Halt` und `MC_HaltCA` zurück und liefert anschließend `E_StateDone`.

### M_ClearMove

```
METHOD M_ClearMove : E_MethodState
VAR_INPUT
END_VAR
```

**Funktion:**

Setzt das `Execute`-Signal von `MC_MoveAbsoluteCA` zurück und liefert anschließend `E_StateDone`.

## Verwendung

Ein übergeordneter Zustandsautomat kann den Baustein beispielsweise wie folgt verwenden:

1. **Mover freigeben:** `M_PowerOn(fOverride := 100.0)` zyklisch aufrufen, bis `E_StateDone` erreicht ist.
2. **Collision-Avoidance-Gruppe zuweisen:** `M_AddToGroup(TRUE, fbCaGroup.P_GroupRef)` bis `E_StateDone` aufrufen und danach mit `M_AddToGroup(FALSE, fbCaGroup.P_GroupRef)` zurücksetzen.
3. **Positionieren:** `ST_MoveParameter` füllen und je nach Aufgabenstellung `M_MoveAbsolute(...)` oder `M_MoveModulo(...)` aufrufen. Den Status über den Methodenrückgabewert und `P_MoveModulo` auswerten.
4. **Halten:** Für einen normalen Halt `M_Halt(...)`, für einen Halt unter Berücksichtigung des Mover-Abstands `M_HaltCa(...)` verwenden.
5. **Produktdaten verwalten:** Belegung und Buchstabe über `P_ProductOnMover` und `P_LetterOnMover` setzen bzw. lesen.
6. **Fehlerbehandlung:** Achsstatus über `P_AxisStatus` prüfen, bei Bedarf `M_Reset(...)` ausführen und interne Befehle über die Clear-Methoden zurücksetzen.

Die Instanz von `FB_Mover` muss in jedem SPS-Zyklus aufgerufen werden, damit Achsstatus und interne Motion-Control-Funktionsblöcke kontinuierlich aktualisiert werden. `M_MoveAbsolute` und `M_MoveModulo` verwenden dieselbe `MC_MoveAbsoluteCA`-Instanz und dürfen daher nicht gleichzeitig gestartet werden.