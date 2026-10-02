# Schaltschrank
Der Schaltschrank ist zweigeteilt. Es gibt eine rechte und eine linke Seite. Des Weiteren befinden sich hinter dem Schaltschrank die Steuerungen der Roboter mit der Bezeichnung *CS8C*.

## Rechte Schaltschrankseite
Den Aufbau der rechten Schaltschrankseite können der folgenden Abbildung und Tabelle entnommen werden.

<div align="center">
	<img decoding="async" src="../../images/Schaltschrank/Schaltschrank-rechts.png" alt="Schaltschrank rechts" width="550" height="390"> <br>
	 Schaltschrank rechts
</div>

| Nr. | Beschreibung             | Hersteller   | Bezeichnung  |
| --- | ------------------------ | ------------ | ------------ |
| 2   | Ethernet-Port-Multiplier | Beckhoff     | CU2508       |
| 3   | Overload Current Control |              |              |
| 4   | Akkupack                 | Beckhoff     | C9900-U330   |
| 5   | Ventilator               | Rital        | 3241100      |
| 6   | Stromwandler             | MBS Sulzbach | 30227        |
| 7   | USB-Verlängerung         | Beckhoff     | CU88001-0000 |
| 8   | Industrie-PC             | Beckhoff     | C6930-0050   |
| 9   | Power Supply 1           | Puls         | QT40.241     |
| 10  | Klemmen                  | Beckhoff     | -            |

### Ethernet-Port-Multiplier
Ein Ethernet-Port-Multiplier ist ein Gerät, das mehrere getrennte Ethernet-Netze über einen gemeinsamen Uplink anbindet und die Frames anhand von Kennungen wieder den richtigen Ports zuordnet. Damit stellt er die physische und echtzeitfähige Aufteilung in mehrere Ethernet-Ports bereit. Er wird für die XTS benötigt, weil die XTS-Software den CU2508 als eigene Ethernet-Instanz im Master erwartet, bevor die Module hinzugefügt werden. Zudem besteht das XTS aus mehreren Segmenten, die sauber getrennt und synchronisiert laufen müssen. Das wird durch den Port-Multiplier realisiert.

### Klemmen
Die hier befindlichen Klemmen werden in der Software als *Main IO* bezeichnet. Folgende Klemmen mit folgenden Bezeichnungen (nach Schaltplan) sind verbaut:

<div align="center">
	<img decoding="async" src="../../images/Schaltschrank/KlemmreihenfolgeMainIO.png" alt="Klemmenreihenfolge MainIO" width="200" height="300"> <br>
	 Klemmenreihenfolge Main IO
</div>

## Linke Schaltschrankseite
Den Aufbau der linken Schaltschrankseite können der folgenden Abbildung und Tabelle entnommen werden.

<div align="center">
	<img decoding="async" src="../../images/Schaltschrank/Schaltschrank-links.png" alt="Schaltschrank links" width="550" height="350"> <br>
	 Schaltschrank links
</div>

| Nr. | Beschreibung                | Hersteller   | Bezeichnung   |
| --- | --------------------------- | ------------ | ------------- |
| 1.1 | Sicherungen links      		| Siemens      |               |
| 1.2 | Sicherungen rechts     		| Siemens      |               |
| 2   | Power Supply XTS            |              |               |
| 3.1 | Power Supply 2              | Puls         | QT20.241      |
| 3.2 | Power Supply 3              | Puls         | QT40.481      |
| 3.3 | Power Supply 4              | Puls         | QT40.481      |
| 4   | Leistungsschutz XTS 1 und 2 | Siemens      | 3RT2526-1BB40 |
| 5   | Türkopplung                 | Wöhner       |               |
| 6   | Sicherung Power Supply 4    | Siemens      |               |
| 7   | Sicherung Power Supply 3    | Siemens      |               |
| 8   | Stromwandler                | MBS Sulzbach | 50-0039       |

<div align="center">
	<img decoding="async" src="../../images/Schaltschrank/Sicherungen1.png" alt="Sicherungen1" width="450" height="300"> <br>
	 Sicherungen links: <br>
	 1 - EL3773, 2 - Power Supply 1, 3 - ?, 4 - Power Supply 2
</div>

<div align="center">
	<img decoding="async" src="../../images/Schaltschrank/Sicherungen2.png" alt="Sicherungen2" width="500" height="300"> <br>
	 Sicherungen rechts: <br>
	 1 - Power Supply 3, 2 - Power Supply 4, 3 - Roboter 1, 4 - Roboter 2, <br>
	 5 - Roboter 3, 6 - EL3403, 7 - Ventilator, 8 - TP-Link
</div>

## Robotersteuerungen

> Informationen zur Steuerung vom Hersteller erhalten Sie im **User Manual** online oder im ILIAS-Kurs.

Stäubli bietet zwei Systeme zur Steuerung ihrer Roboter an: [uniVAL-drive](https://www.staubli.com/global/de/robotics/produkte/digitale-loesungen/uniVAL-drive.html) und [uniVAL-plc](https://www.staubli.com/global/de/robotics/produkte/digitale-loesungen/uniVAL-plc.html). Die Steuerung der Roboter des efa-Demonstrators verwenden uniVAL-drive. Es ist eine Schnittstelle, welche die Kommunikation via EtherCAT mit dem IPC und somit eine Programmierung mit der Software TwinCAT möglich macht.
