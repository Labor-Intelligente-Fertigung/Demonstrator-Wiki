# FB_CaGroup

## Einsatzbereich
Der Funktionsbaustein *FB_CaGroup* kapselt die grundlegende Verwaltung einer Collision-Avoidance-Achsgruppe. Dafür verwendet er die TwinCAT-Motion-Control-Funktionsblöcke `MC_GroupReset`, `MC_GroupEnable`, `MC_GroupDisable`, `MC_GroupStop`, `MC_UngroupAllAxes` und `MC_GroupReadStatus`.

Er wird von einem übergeordneten Baustein, beispielsweise der XTS-Ablaufsteuerung, verwendet und stellt Methoden zum Aktivieren, Deaktivieren, Stoppen, Rücksetzen, Überwachen und Auflösen der Gruppe bereit. Das Hinzufügen einzelner Mover erfolgt nicht in diesem Baustein, sondern über `FB_Mover.M_AddToGroup`.

Die eigentliche Collision-Avoidance-Konfiguration sowie bewegungs- und sicherheitsrelevante Grenzwerte werden außerhalb des Bausteins in der TwinCAT-Konfiguration bzw. in den verwendeten Motion-Parametern festgelegt.

## Schnittstelle

### Deklaration
```
FUNCTION_BLOCK FB_CaGroup
VAR_INPUT
    refCaGroupRef : REFERENCE TO AXES_GROUP_REF; // Reference to the collision avoidance group
END_VAR
VAR_OUTPUT
END_VAR
VAR
    fbMcGroupReset             : MC_GroupReset;
    stMCGroupResetReturn       : ST_McOutputs;
    fbMcGroupEnable            : MC_GroupEnable;
    stMcGroupEnableReturn      : ST_McOutputs;
    fbMcGroupDisable           : MC_GroupDisable;
    stMcGroupDisableReturn     : ST_McOutputs;
    fbMcGroupStop              : MC_GroupStop;
    stMcGroupStopReturn        : ST_McOutputs;
    fbMcUngroupAllAxes         : MC_UngroupAllAxes;
    stMcUngroupAllAxesReturn   : ST_McOutputs;
    fbMCReadGroupState         : MC_GroupReadStatus;
    stMCReadGroupState         : ST_McOutputs;
END_VAR
```

Die internen Motion-Control-Instanzen und ihre Statusstrukturen sind nach außen gekapselt. Die Bedienung erfolgt über Methoden und Eigenschaften.

### Eingänge

```
refCaGroupRef : REFERENCE TO AXES_GROUP_REF;
```

Referenz auf die zugehörige Collision-Avoidance-Achsgruppe. Im zyklischen Bausteincode wird die Referenz mit `__ISVALIDREF(refCaGroupRef)` geprüft. Bei ungültiger Referenz werden die internen Motion-Control-Funktionsblöcke in diesem Zyklus nicht aufgerufen.

Es existieren keine direkten Ausgänge.

## Eigenschaften

### P_CountAxisInGroup

```
PROPERTY P_CountAxisInGroup : UDINT
```

Liefert die aktuelle Anzahl der Achsen in der Gruppe aus `refCaGroupRef.NcToPlc.Common.GroupAxesCount`. Die Eigenschaft wird unter anderem von `M_Ungroup` verwendet, um zu prüfen, ob die Gruppe vollständig geleert wurde.

### P_GroupRef

```
PROPERTY P_GroupRef : REFERENCE TO AXES_GROUP_REF
```

Stellt die interne Gruppenreferenz als Referenz bereit. Damit kann ein übergeordneter Baustein dieselbe Gruppe beispielsweise an `FB_Mover.M_AddToGroup` übergeben.

### P_GroupState

```
PROPERTY P_GroupState : MC_GROUP_STATE
```

Liefert den aktuellen Gruppenzustand aus `refCaGroupRef.NcToPlc.Common.GroupStatus.State`.

## Methoden

Alle Methoden liefern einen Wert vom Typ `E_MethodState`. Die zugehörigen Motion-Control-Funktionsblöcke werden zyklisch im Hauptteil von `FB_CaGroup` aufgerufen.

### M_Enable

```
METHOD M_Enable : E_MethodState
VAR_INPUT
END_VAR
```

**Funktion:**

Aktiviert die Achsgruppe über `MC_GroupEnable`. Solange kein Befehl aktiv ist, wird `Execute` gesetzt. Während der Bearbeitung bzw. nach Abschluss wird `Execute` wieder zurückgenommen.

**Rückgabewerte:**

- `E_StateBusy`: Aktivierung läuft.
- `E_StateDone`: Gruppe wurde aktiviert (`Done = TRUE`).
- `E_StateError`: Fehler beim Aktivieren der Gruppe.
- `E_StateUndifine`: Der interne Status ist weder *Busy*, *Done* noch *Error*.

### M_Disable

```
METHOD M_Disable : E_MethodState
VAR_INPUT
END_VAR
```

**Funktion:**

Deaktiviert die Achsgruppe über `MC_GroupDisable`. Die Methode setzt den Befehl selbstständig und nimmt `Execute` während der Bearbeitung bzw. nach Abschluss wieder zurück.

**Rückgabewerte:**

- `E_StateBusy`: Deaktivierung läuft.
- `E_StateDone`: Gruppe wurde deaktiviert.
- `E_StateError`: Fehler beim Deaktivieren der Gruppe.

### M_Stop

```
METHOD M_Stop : E_MethodState
VAR_INPUT
END_VAR
```

**Funktion:**

Stoppt die Achsgruppe über `MC_GroupStop`. Im Baustein werden keine zusätzlichen Dynamikparameter vorgegeben; das Verhalten ergibt sich aus der Motion-Konfiguration und dem verwendeten Funktionsbaustein.

**Rückgabewerte:**

- `E_StateBusy`: Stopp läuft.
- `E_StateDone`: Gruppenstopp wurde abgeschlossen.
- `E_StateError`: Fehler beim Stoppen der Gruppe.

### M_Reset

```
METHOD M_Reset : E_MethodState
VAR_INPUT
END_VAR
```

**Funktion:**

Setzt einen Gruppenfehler über `MC_GroupReset` zurück. `Execute` wird gesetzt, sobald kein Reset aktiv ist, und während der Bearbeitung bzw. nach Abschluss wieder zurückgenommen.

**Rückgabewerte:**

- `E_StateBusy`: Reset läuft.
- `E_StateDone`: Reset wurde abgeschlossen.
- `E_StateError`: Fehler beim Reset der Gruppe.

### M_ReadState

```
METHOD M_ReadState : E_MethodState
VAR_INPUT
END_VAR
```

**Funktion:**

Aktiviert `MC_GroupReadStatus` und wartet auf einen gültigen Gruppenstatus. Sobald der Status gültig ist, wird `Enable` wieder zurückgenommen. Der eigentliche Gruppenzustand kann anschließend über `P_GroupState` gelesen werden.

**Rückgabewerte:**

- `E_StateBusy`: Statusabfrage läuft.
- `E_StateDone`: Ein gültiger Gruppenstatus liegt vor (`Valid = TRUE`).
- `E_StateError`: Fehler bei der Statusabfrage.

### M_Ungroup

```
METHOD M_Ungroup : E_MethodState
VAR_INPUT
END_VAR
```

**Funktion:**

Entfernt über `MC_UngroupAllAxes` alle Achsen aus der Gruppe. Die Methode gilt erst dann als abgeschlossen, wenn der Motion-Befehl `Done` meldet und `P_CountAxisInGroup = 0` ist.

**Rückgabewerte:**

- `E_StateBusy`: Das Auflösen der Gruppe läuft oder die Gruppe enthält noch Achsen.
- `E_StateDone`: Alle Achsen wurden erfolgreich aus der Gruppe entfernt.
- `E_StateError`: Fehler beim Auflösen der Gruppe.

### M_Clear

```
METHOD M_Clear : E_MethodState
VAR_INPUT
END_VAR
```

**Funktion:**

Setzt die `Execute`- bzw. `Enable`-Signale aller internen Motion-Control-Funktionsblöcke zurück. Die Methode dient zum Aufräumen und Initialisieren von Befehlsfolgen und liefert nach dem Rücksetzen `E_StateDone`.

## Verwendung

Ein übergeordneter Zustandsautomat kann den Baustein beispielsweise in folgender Reihenfolge verwenden:

1. **Ausgangszustand herstellen:** Falls `P_CountAxisInGroup <> 0`, `M_Ungroup()` bis `E_StateDone` aufrufen.
2. **Mover hinzufügen:** Jedem beteiligten `FB_Mover` über `M_AddToGroup(TRUE, fbCaGroup.P_GroupRef)` die Gruppenreferenz übergeben.
3. **Gruppe aktivieren:** `M_Enable()` zyklisch aufrufen, bis `E_StateDone` erreicht ist.
4. **Status überwachen:** `M_ReadState()` verwenden und den Gruppenzustand über `P_GroupState` auswerten.
5. **Anhalten oder deaktivieren:** Bei Bedarf `M_Stop()` und anschließend `M_Disable()` aufrufen.
6. **Fehlerbehandlung:** Gruppenfehler mit `M_Reset()` zurücksetzen und Befehle bei Bedarf über `M_Clear()` initialisieren.

Da die Motion-Control-Funktionsblöcke im Hauptteil zyklisch abgearbeitet werden, muss auch die Instanz von `FB_CaGroup` in jedem SPS-Zyklus aufgerufen werden.