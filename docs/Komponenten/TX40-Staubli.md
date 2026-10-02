# Roboter TX40

| Bezeichnung				| TX40-S2-R5														 |
| ------------------------- | ------------------------------------------------------------------ |
| Typ						| 6-Achs-Knickarm-Roboter											 |
| Hersteller				| [Stäubli Faverges SCA](https://www.staubli.com/de/de/home.html)	 |
| Anzahl                    | 3                                                                  |
| Gewicht                   | 27 kg                                                              |
| Nutzlast                  | 1,7 kg (Nenngeschwindigkeit), 2,3 kg (re-duzierte Geschwindigkeit) |
| Bewegungsradius           | 450 mm (von Achse 1-5)                                             |
| Maximale Geschwindigkeit  | 8,2 m/s                                                            |
| Maximale Energie          | 65 J                                                               |
| Wiederholgenauigkeit      | ±0,02 mm                                                           |
| Geräuschpegel             | 66 dBa                                                             |
| Betriebstemperatur        | +5°C bis +40°C                                                     |
| Relative Luftfeuchtigkeit | 30% bis 95%, kondensationsfrei                                     |
| Softwareversion			| s7.9.1 - uniVAL drive												 |
| Steuerung					| [CS8C](./Schaltschrank.md)

> Weitere Infos finden Sie im **User Manual** des Herstellers online oder im ILIAS-Kurs.

## Kinematik

> Grundlagen der Robotik werden [hier](../Grundlagen/Robotik.md) erklärt.

Die Bewegungen der Roboterarmgelenke werden durch Servomotoren erzeugt, die mit Lagesensoren verbunden sind. An den Achsen 1, 2, 3 und 5 sind diese Servomotoren hier mit einer Haltebremse ausgestattet.

=== "Aufbau"

	<div align="center">
		<img decoding="async" src="../../images/robot/Roboter-Segmente.png" alt="Robotersegmente" width="400" height="400"> <br>
		Segmente und -achsen des TX40 [adaptiert aus <a href="https://www.staubli.com/global/de/robotics/produkte/roboterarme/tx2-40.html">Stäubli</a>]
	</div>

=== "Koordinatensysteme"

	<div align="center">
		<img decoding="async" src="../../images/robot/Roboter-Koordinatensysteme.png" alt="Roboter Koordinatensysteme" width="600" height="450"> <br>
		Koordinatensysteme des TX40 [adaptiert aus <a href="https://www.staubli.com/global/de/robotics/produkte/roboterarme/tx2-40.html">Stäubli</a>]
	</div>

=== "Maße"

	<div align="center">
		<img decoding="async" src="../../images/robot/RoboterKinematik.png" alt="Roboter Kinematik" width="400" height="520"> <br>
		Kinematik des TX40 [adaptiert aus <a href="https://www.staubli.com/global/de/robotics/produkte/roboterarme/tx2-40.html">Stäubli</a>]
	</div>

## Aufgaben

**Roboter 1** <br> Nimmt Produkt aus Ausgabe und legt es auf einen Mover des XTS 1.

**Roboter 2** <br> Nimmt Produkt vom Mover des XTS 1. Wenn es benötigt wird, wird es unter Kamera 2 gelegt und anschließend auf einen Mover des XTS 2. Wird es nicht benötigt, wird es direkt auf einen Mover des XTS 2 gelegt.

**Roboter 3** <br> Nimmt Produkt vom Mover des XTS 2 und legt es in die Ausgabe.

## Posen
### Home Posen

=== "Roboter 1"

	<div align="center">
		<img decoding="async" src="../../images/robot/Roboter1-Home.png" alt="Roboter 1 Home" width="400"> <br>
		Home Pose Roboter 1
	</div>

	```
	ROBOT1_HOME_ACS : ARRAY[1..6] OF LREAL := [68.2515, 27.2293, 94.6346, -0.1171, 57.5845, -93.2959];
	ROBOT1_HOME_MCS : ARRAY[1..6] OF LREAL := [76.7778, 286.6219, 16.2903, -179.4550, -0.1296, -18.3914];
	```
=== "Roboter 2"

	<div align="center">
		<img decoding="async" src="../../images/robot/Roboter2-Home.png" alt="Roboter 2 Home" width="450"> <br>
		Home Pose Roboter 2
	</div>

	```
	ROBOT2_HOME_ACS : ARRAY[1..6] OF LREAL := [-15.8673, 59.1811, 57.2852, 0.0523, 63.8398, 163.7821];
	ROBOT2_HOME_MCS : ARRAY[1..6] OF LREAL := [388.86, -74.09, -50.0, -179.871, 0.281, 0.326];
	```

=== "Roboter 3"

	<div align="center">
		<img decoding="async" src="../../images/robot/Roboter3-Home.png" alt="Roboter 3 Home" width="400"> <br>
		Home Pose Roboter 3
	</div>

	```
	ROBOT3_HOME_ACS : ARRAY[1..6] OF LREAL := [61.7513, -36.1016, -90.5319, 1.0230, -55.0941, 86.4549];
	ROBOT3_HOME_MCS : ARRAY[1..6] OF LREAL := [-177.273, -258.0, -17.426, 178.322, 0.926, 154.685];
	```

### Weitere Posen im MCS

=== "Roboter 1"

	```
	ROBOT1_XTS1_APPROACH		: ARRAY[1..6] OF LREAL := [288.907, 255.113, 16.291, -179.455, -0.131, -18.391];
	ROBOT1_XTS1_PRETARGET		: ARRAY[1..6] OF LREAL := [288.707, 255.319, -101.260, -179.455, -0.131, -18.391];
	ROBOT1_XTS1_TARGET			: ARRAY[1..6] OF LREAL := [288.907, 255.773, -122.360, -179.0, -0.5, -18.391];
	ROBOT1_XTS1_DEPART			: ARRAY[1..6] OF LREAL := [289.907, 255.113, 16.291, -179.0, -0.5, -18.391];
	ROBOT1_STATION_APPROACH		: ARRAY[1..6] OF LREAL := [-68.699, 337.873, 16.292, -165.361, -8.237, -18.532];
	ROBOT1_STATION_PRETARGET	: ARRAY[1..6] OF LREAL := [-151.55, 417.31, 4.8, -165.361, -8.237, -18.532];
	ROBOT1_STATION_TARGET		: ARRAY[1..6] OF LREAL := [-147.016, 424.627, -18.5, -165.361, -8.237, -18.532];
	ROBOT1_STATION_POSTTARGET	: ARRAY[1..6] OF LREAL := [-147.016, 424.527, 0.0, -165.361, -8.237, -18.532];
	```

=== "Roboter 2"

	```
	ROBOT2_XTS1_APPROACH		: ARRAY[1..6] OF LREAL := [388.86, -74.09, -50.0, -179.871, 0.281, 0.326];
	ROBOT2_XTS1_PRETARGET		: ARRAY[1..6] OF LREAL := [388.86, -74.09, -101.268, -179.871, 0.281, 0.326];
	ROBOT2_XTS1_TARGET			: ARRAY[1..6] OF LREAL := [388.86, -74.09, -121.268, -179.871, 0.281, 0.326];
	ROBOT2_XTS1_DEPART			: ARRAY[1..6] OF LREAL := [388.86, -74.09, -50.0, -179.871, 0.281, 0.326];
	ROBOT2_XTS2_APPROACH		: ARRAY[1..6] OF LREAL := [383.68, 74.1, -50.000, -179.873, 0.281, 0.329];
	ROBOT2_XTS2_PRETARGET		: ARRAY[1..6] OF LREAL := [383.68, 74.1, -101.925, -179.873, 0.281, 0.329];
	ROBOT2_XTS2_TARGET			: ARRAY[1..6] OF LREAL := [383.68, 74.1, -121.925, -179.873, 0.281, 0.329];
	ROBOT2_XTS2_DEPART			: ARRAY[1..6] OF LREAL := [388.68, -74.1, -50.0, -179.873, 0.281, 0.329];
	ROBOT2_STATION_APPROACH1	: ARRAY[1..6] OF LREAL := [388.68, -74.1, 55.75, -179.873, 0.281, 0.329];
	ROBOT2_STATION_APPROACH2	: ARRAY[1..6] OF LREAL := [-8.21, 382.48, 55.75, -179.873, 0.281, 0.329];
	ROBOT2_STATION_PRETARGET	: ARRAY[1..6] OF LREAL := [-8.21, 382.48, 35.75, -179.873, 0.281, 0.329];
	ROBOT2_STATION_TARGET		: ARRAY[1..6] OF LREAL := [-8.21, 382.48, 15.75, -179.873, 0.281, 0.329];
	ROBOT2_STATION_WAIT			: ARRAY[1..6] OF LREAL := [-8.21, 221.834, 55.75, -179.873, 0.281, 0.329];
	ROBOT2_STATION_DEPART		: ARRAY[1..6] OF LREAL := [383.68, 74.1, 55.75, -179.873, 0.281, 0.329];
	```

=== "Roboter 3"

	```
	ROBOT3_XTS2_APPROACH		: ARRAY[1..6] OF LREAL := [-247.829, -258.0, -170.426, 178.322, 0.926, 154.685];
	ROBOT3_XTS2_PRETARGET		: ARRAY[1..6] OF LREAL := [-247.829, -258.0, -200.426, 178.322, 0.926, 154.685];
	ROBOT3_XTS2_TARGET			: ARRAY[1..6] OF LREAL := [-247.829, -258.0, -218.426, 178.322, 0.926, 154.685];
	ROBOT3_XTS2_DEPART			: ARRAY[1..6] OF LREAL := [-247.829, -258.0, -80.426, 178.322, 0.926, 154.685];
	ROBOT3_STATION_APPROACH		: ARRAY[1..6] OF LREAL := [48.871, -283.923, 205.842, 167.970, 6.050, 151.907];
	ROBOT3_STATION_PRETARGET	: ARRAY[1..6] OF LREAL := [143.901, -283.923, 205.842, 167.970, 6.050, 151.907];
	ROBOT3_STATION_TARGET		: ARRAY[1..6] OF LREAL := [151.115, -273.791, 182.118, 167.970, 6.050, 151.907];
	ROBOT3_STATION_DEPART		: ARRAY[1..6] OF LREAL := [48.871, -283.923, 205.842, 167.970, 6.050, 151.907];
	```