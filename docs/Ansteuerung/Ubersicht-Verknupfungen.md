# Übersicht Verknüpfungen
Hier sind alle Geräte des Demonstrator beschrieben, die durch das Scannen der Geräte des Demonstrators in TwinCAT eingelesen werden können. Für das Scannen benötigen Sie eine Verbindung zum Demonstrator. Der Verbindungsaufbau ist unter [Programmierung](./index.md) bereits erklärt.

## XTS1 und XTS2
Diese sind über Port Multiplier (CU2508) angeschlossen. Da beide XTS über zwei Einspeisungsmodule verfügen haben sie auch jeweils zwei Master. Zusätzlich gibt es für jedes XTS einen Ethernet Adapter. Die XTS sind somit aufgeteilt in: 

- Ethernet Adapter (`XTS1` und `XTS2`)
- Port 1 (`XTS1-1` und `XTS2-1`)
- Port 2 (`XTS1-2` und `XTS2-2`)

> [!TIP] Tipp
> Zum Erkennen der richtigen Geräte, kann durch einen Doppelklick auf das Gerät im Projektmappen-Explorer anschließend im Reiter `Adapter` die *Adapter Referenz* ausgelesen werden.
> <div align="center">
>	<img decoding="async" src="../../images/XTS/Verbindung-AdapterReferenz.png" alt="Adapter Referenz" width="550"> <br>
>	 Adapter Referenz auslesen
></div>

## Real Robot

>Das Einscannen der Roboter ist auf der Seite [Roboter Einrichtung](./Einrichtung/Roboter-Einrichtung.md) separat erklärt. Hier gibt es einige Dinge zu beachten.

Die drei Roboter sind über einen gemeinsamen EtherCAT Master angeschlossen. Es können zusätzlich zu den 6-Roboterachsen folgende Variablen verwendet werden:

| Channel    | Variablenname | Datentyp | Erklärung         |
| :--------- | :------------ | :------- | :---------------- |
| UEN1_DOOR1 | bInEstop      | Bool     | Roboter in Notaus |
| valve1     | bGrasp        | Bool     | Greifer schließen |

## Front-IO
Die hier angeschlossenen Klemmen haben keine Funktion und können im TwinCAT Projekt deakiviert werden. Folgende Klemmen sind hier verbaut:

- EK1100
- 3 x EL2809
- 3 x EL3403
- EL3773
- EL9011

## Main-IO
Die Namen der Klemmen sind dem Schaltplan entnommen. Folgend sind alle Klemmen des Main-IOs und dessen Anschlüsse aufgelistet.

> [!TIP] Tipp
> Um die Channel im *Verknüpfungsfenster* einzuschalten, schalten Sie in der rechten Leiste des Fensters `Variablengruppen zeigen` ein.
> <div align="center">
>	<img decoding="async" src="../../images/Verknupfung-Variablengruppen.png" alt="Variablengruppen" width="500"> <br>
>	 Variablengruppen einschalten
></div>

### 400DI1 - EL1008

| Channel |   I/O   | Datentyp | Erklärung                                    |
| :-----: | :-----: | :------: | :------------------------------------------- |
|    1    | Eingang |   Bool   | Tür Taster 3 (Reserve)                       |
|    2    | Eingang |   Bool   | Tür Taster 2 (Anmeldung / Quittierung)       |
|    3    | Eingang |   Bool   | Tür Taster 1 (Quittierung Notaus Kette)      |
|    4    | Eingang |   Bool   | Türverriegelung (Tür geschlossen)            |
|    7    | Eingang |   Bool   | Lichtschranke (Buchstabe auf Mover an XTS 2) |

### 410DI1 - EL1008

| Channel |   I/O   | Datentyp | Erklärung                                     |
| :-----: | :-----: | :------: | :-------------------------------------------- |
|    1    | Eingang |   Bool   | Hilfsschalter 200F2 (Spannungsversorgung XTS) |
|    2    | Eingang |   Bool   | Hilfsschalter 200F1 (Spannungsversorgung XTS) |
|    3    | Eingang |   Bool   | Hilfsschalter 200F3 (Spannungsversorgung XTS) |
|    4    | Eingang |   Bool   | Hilfsschalter 200F5 (Spannungsversorgung XTS) |
|    5    | Eingang |   Bool   | Hilfsschalter 70F1                            |
|    6    | Eingang |   Bool   | Hilfsschalter 200F4 (Spannungsversorgung XTS) |
|    7    | Eingang |   Bool   | Hilfsschalter 200F6 (Spannungsversorgung XTS) |

### 420DI1 - EL1008 

| Channel |   I/O   | Datentyp | Erklärung                                    |
| :-----: | :-----: | :------: | :------------------------------------------- |
|    1    | Eingang |   Bool   | Zylinder Status: Ausgefahren (extended)      |
|    2    | Eingang |   Bool   | Zylinder Status: Eingefahren (retracted)     |
|    3    | Eingang |   Bool   | Schlitten Status: Eingefahren (retracted)    |
|    4    | Eingang |   Bool   | Schlitten Status: Ausgefahren (extended)     |
|    5    | Eingang |   Bool   | Lichtschranke (Buchstabe auf Schlitten)      |
|    6    | Eingang |   Bool   | Lichtschranke (Buchstabe auf Mover an XTS 1) |

### 430DI1 - EL1008 
Hier liegen keine relevanten Verknüpfungen vor.

### 440DO1 - EL2008 

| Channel |   I/O   | Datentyp | Erklärung                            |
| :-----: | :-----: | :------: | :----------------------------------- |
|    1    | Ausgang |   Bool   | Zylinder Befehl: Einfahren (Retract) |
|    2    | Ausgang |   Bool   | Schlitten Befehl: Ausfahren (Extend) |
|    3    | Ausgang |   Bool   | Lampe auf XTS1                       |
|    4    | Ausgang |   Bool   | LED Tür Taster 3                     |
|    5    | Ausgang |   Bool   | LED Tür Taster 2                     |
|    6    | Ausgang |   Bool   | LED Tür Taster 1                     |

### 450D01 - EL2024 

| Channel |   I/O   | Datentyp | Erklärung      |
| :-----: | :-----: | :------: | :------------- |
|    1    | Ausgang |   Bool   | Licht Kamera 1 |
|    2    | Ausgang |   Bool   | Licht Kamera 2 |
|    3    | Ausgang |   Bool   | Licht Kamera 3 |

### 460DO1 - EL2522 
Hier liegen keine relevanten Verknüpfungen vor.

### 110ST1 - EL9110 
Hier findet eine Potenzialeinspeisung statt.

### 500SI1 - EL1918
Durch eine Safety-PLC sind in dieser Klemme verschiedenste Variablen definiert, die für die Sicherheitsfunktionen benötigt werden.

|  Channel   |   I/O   |   Datentyp    | Variablenname                       | Erklärung                                          |
| :--------: | :-----: | :-----------: | :---------------------------------- | :------------------------------------------------- |
| Message_35 | Eingang | Safety.FSOE_6 | FSOE                                | Safety-over-EtherCAT (Türverriegelung)             |
| Message_36 | Eingang | Safety.FSOE_6 | FSOE                                | Safety-over-EtherCAT (XTS Power Supply, Roboter 1) |
| Message_37 | Eingang | Safety.FSOE_6 | FSOE                                | Safety-over-EtherCAT (Roboter 2 & 3)               |
|   Var 13   | Eingang |     Bool      | IbEStop1_SeOk                       | Notaus Schalter Status                             |
|   Var 14   | Eingang |     Bool      | IbDo1_Estop_SeOk                    | Tür Status (Notaus)                                |
|   Var 15   | Eingang |     Bool      | IbDo1_SeOk                          | Tür Status                                         |
|   Var 17   | Eingang |     Bool      | IbXtsPowerSupply_ScOk               | XTS Status (Spannungsversorgung)                   |
|   Var 19   | Eingang |     Bool      | IbRobotSystems_ScOk                 | Roboter Status (Spannungsversorgung)               |
|   Var 40   | Eingang |     Bool      | IbDo1_Locked                        | Tür Status (Verriegelt)                            |
| Message_35 | Ausgang |     FSOE      | Safety.FSOE_6                       | Safety-over-EtherCAT (Türverriegelung)             |
| Message_36 | Ausgang |     FSOE      | Safety.FSOE_6                       | Safety-over-EtherCAT (XTS Power Supply, Roboter 1) |
| Message_37 | Ausgang |     FSOE      | Safety.FSOE_6                       | Safety-over-EtherCAT (Roboter 2 & 3)               |
|   Var 1    | Ausgang |     Bool      | ObTwinSafeGrpErrAck_Io              | Error Bestätigung (IO)                             |
|   Var 21   | Ausgang |     Bool      | ObDo1_UnlckReq                      | Tür Entriegeln                                     |
|   Var 22   | Ausgang |     Bool      | ObTwinSafeGrpErrAck_ RobotSystems   | Error Bestätigung (Roboter)                        |
|   Var 23   | Ausgang |     Bool      | ObTwinSafeGrpErrAck_ XtsPowerSupply | Error Bestätigung (XTS Power Supply)               |
|   Var 24   | Ausgang |     Bool      | ObTwinSafeGrpErrAck_ Do1Release     | Error Bestätigung (Türfreigabe)                    |
|   Var 25   | Ausgang |     Bool      | ObTwinSafeGrpRun_ Do1Release        | Türfreigabe                                        |
|   Var 26   | Ausgang |     Bool      | ObTwinSafeGrpRun_Io                 | IO                                                 |
|   Var 27   | Ausgang |     Bool      | ObTwinSafeGrpRun_ RobotSystem       | Roboter                                            |
|   Var 28   | Ausgang |     Bool      | ObTwinSafeGrpRun_ XtsPowerSupply    | XTS Power Supply                                   |
|   Var 30   | Ausgang |     Bool      | ObDo1_Reset                         | Tür Reset                                          |
|   Var 31   | Ausgang |     Bool      | ObDo1_Estop_Reset                   | Tür Notaus Reset                                   |
|   Var 32   | Ausgang |     Bool      | ObEstop1_Reset                      | Notaus Reset                                       |
|   Var 39   | Ausgang |     Bool      | ObDo1_LckReq                        | Tür Verriegeln                                     |

### 500SO1 - EL2904

|    Channel    |   I/O   |   Datentyp    | Variablenname | Erklärung                              |
| :-----------: | :-----: | :-----------: | :------------ | :------------------------------------- |
| FSOE Module 1 | Eingang | FSOE_DAA792F3 | Message_35    | Safety-over-EtherCAT (Türverriegelung) |

### 510SO1 - EL2904

|    Channel    |   I/O   |   Datentyp    | Variablenname | Erklärung                                          |
| :-----------: | :-----: | :-----------: | :------------ | :------------------------------------------------- |
| FSOE Module 1 | Eingang | FSOE_DAA792F3 | Message_36    | Safety-over-EtherCAT (XTS Power Supply, Roboter 1) |

### 520SO1 - EL2904

|    Channel    |   I/O   |   Datentyp    | Variablenname | Erklärung                            |
| :-----------: | :-----: | :-----------: | :------------ | :----------------------------------- |
| FSOE Module 1 | Eingang | FSOE_DAA792F3 | Message_37    | Safety-over-EtherCAT (Roboter 2 & 3) |

### 700AI1 - EL3773 

|  Channel   |   I/O   |      Datentyp       | Erklärung      |
| :--------: | :-----: | :-----------------: | -------------- |
| U1 Sampels | Eingang | U1_Samples_055F796E | Spannung L1    |
| I1 Sampels | Eingang | U1_Samples_055F796E | Stromstärke L1 |
| U2 Sampels | Eingang | U1_Samples_055F796E | Spannung L2    |
| I2 Sampels | Eingang | U1_Samples_055F796E | Stromstärke L2 |
| U3 Sampels | Eingang | U1_Samples_055F796E | Spannung L3    |
| I3 Sampels | Eingang | U1_Samples_055F796E | Stromstärke L3 |

### 710AI1 - EL3403

| Channel                                 |   I/O   | Datentyp | Variablenname               |
| :-------------------------------------- | :-----: | :------: | :-------------------------- |
| PM Inputs Channel 1: TxPDO Toggle       | Eingang |   Bool   | L1_TxPDO_Toggle             |
| PM Inputs Channel 1: Current            | Eingang |   DINT   | L1_Current                  |
| PM Inputs Channel 1: Voltage            | Eingang |   DINT   | L1_Voltage                  |
| PM Inputs Channel 1: Active Power       | Eingang |   DINT   | L1_ActivePower              |
| PM Inputs Channel 1: Index              | Eingang |  DSINT   | L1_IndexIn                  |
| PM Inputs Channel 1: Variant Value      | Eingang |   DINT   | L1_VariantValue             |
| PM Inputs Channel 2: TxPDO Toggle       | Eingang |   Bool   | L2_TxPDO_Toggle             |
| PM Inputs Channel 2: Current            | Eingang |   DINT   | L2_Current                  |
| PM Inputs Channel 2: Voltage            | Eingang |   DINT   | L2_Voltage                  |
| PM Inputs Channel 2: Active Power       | Eingang |   DINT   | L2_ActivePower              |
| PM Inputs Channel 2: Index              | Eingang |  DSINT   | L2_IndexIn                  |
| PM Inputs Channel 2: Variant Value      | Eingang |   DINT   | L2_VariantValue             |
| PM Inputs Channel 3: TxPDO Toggle       | Eingang |   Bool   | L3_TxPDO_Toggle             |
| PM Inputs Channel 3: Current            | Eingang |   DINT   | L3_Current                  |
| PM Inputs Channel 3: Voltage            | Eingang |   DINT   | L3_Voltage                  |
| PM Inputs Channel 3: Active Power       | Eingang |   DINT   | L3_ActivePower              |
| PM Inputs Channel 3: Index              | Eingang |  DSINT   | L3_IndexIn                  |
| PM Inputs Channel 3: Variant Value      | Eingang |   DINT   | L3_VariantValue             |
| PM Status Data: Missing Zero Crossing A | Eingang |   Bool   | Status_MissingZeroCrossingA |
| PM Status Data: Missing Zero Crossing B | Eingang |   Bool   | Status_MissingZeroCrossingB |
| PM Status Data: Missing Zero Crossing C | Eingang |   Bool   | Status_MissingZeroCrossingC |
| PM Status Data: PhaseSequenceError      | Eingang |   Bool   | Status_PhaseSequenceError   |
| PM Outputs: Cannel 1 Index              | Eingang |  USINT   | L1_IndexOut                 |
| PM Outputs: Cannel 2 Index              | Eingang |  USINT   | L2_IndexOut                 |
| PM Outputs: Cannel 3 Index              | Eingang |  USINT   | L3_IndexOut                 |
