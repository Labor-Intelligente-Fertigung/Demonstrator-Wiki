# XTS

| Bezeichnung				| XTS (eXtended Transport System)			  |
| ------------------------- | ------------------------------------------- |
| Hersteller				| [Beckhoff Automation](https://www.beckhoff.com/de-de/) |
| Anzahl                    | 2                                           |
| Spannungsversorgung       | 24 V_DC                                     |
| Kraft                     | 80 N (2 m/s)                                |
| Maximalgeschwindigkeit    | 4 m/s                                       |
| Betriebstemperatur        | +5°C bis +40°C                              |
| Relative Luftfeuchtigkeit | 15% bis 95%, ohne Betauung und Kondensation |

> Weitere Infos finden Sie auch im **User Manual** des Herstellers online oder im ILIAS-Kurs. 

## Grundlagen
Beim XTS bewegen sich magnetisch angetriebene Mover entlang einer Fahrstrecke. Dabei besteht das System aus **Motormodulen**, **Führungsschienen** und **Movern**.

<div class="xts-numbered-figure" markdown>
<div align="center">
	<img decoding="async" src="../../images/XTS/XTS-Aufbau.png" alt="XTS Aufbau" width="500" height="300"> <br>
	 Aufbau des XTS [adaptiert aus <a href="https://download.beckhoff.com/download/document/motion/xts_ba_de.pdf">Beckhoff Automation</a>]
</div>
<div class="xts-numbered-figure__numbers" markdown>

1. Kurvenschiene
2. Gerade Führungsschiene mit Schleuse
3. Mover
4. Schleuse
5. Kurvensegment
6. Gerades Modul
7. Gerade Führungsschliene ohne Schleuse
8. Maschinenbett
9. Typenschild

</div> </div>

<div class="xts-numbered-figure" markdown>
<div align="center">
	<img decoding="async" src="../../images/XTS/Mover-Aufbau.png" alt="Mover Aufbau" width="310" height="220"> <br>
	 Aufbau des Movers [adaptiert aus <a href="https://download.beckhoff.com/download/document/motion/xts_ba_de.pdf">Beckhoff Automation</a>]
</div>
<div class="xts-numbered-figure__numbers" markdown>

1. Führungsrollen
2. Magnetplatten
3. Geberfahne
4. ESD-Bürste

</div> </div>

## Funktionsweise
Das XTS kann Mover bewegen und dessen Positionen genau erfassen. Die Mover werden durchnummeriert von 1 bis n. Jeder Mover wird als eigene NC Achse betrachtet, welche über einen SoftDrive gesteuert wird. Die Bewegung passiert mit Hilfe von Magneten. An den Movern sind auf der Ober- und Unterseite Magnetplatten befestigt. Diese bestehen aus mehreren Magneten, welche abwechselnd in Nord und Süd angeordnet sind. In den Modulen sind Elektromagnete verbaut. Damit sich ein Mover bewegt, wird die Polarization der Magnete verändert.

Damit die Mover auf dem XTS lokalisiert werden können, sind **Geberfahnen** nötig. Diese sind an den Movern befestigt. Sie werden von Sensoren in den Modulen des XTS erkannt, woraufhin die absoluten Positionen der Mover berechnet werden. 

Es ist möglich einen bestimmten Mover als ersten in der Reihe zu definieren. Bei dem **Mover 1** ist die Reihenfolge der Magnete vertauscht. Um die Ansteuerung dieses Movers zu ermöglichen, muss im Projekt eingestellt werden, dass ein solcher Mover vorhanden ist. Zudem wird eine **Mover 1 Detection** als Initialisierung benötigt. Hierbei werden alle Mover kurz bewegt. Über die Geberfahne kann dann erkannt werden, welcher Mover sich verkehrtherum bewegt.

Über eine **Collision Avoidance Group** können die Abstände der Mover überwacht werden, wodurch Kollisionen verhindert werden können.

## Aufbau
Auf dem Demonstrator befinden sich zwei in sich geschlossene XTS. Es sind pro XTS Module mit den folgenden Produktbezeichnungen verbaut:

- 10 x *AT2000-0250*
- 2 x *AT2001-0250*
- 2 x *AT2050-0500*

Die Anordnung kann der folgenden Abbildung entnommen werden. Die andere Hälfte ist identisch, um 180° gedreht. Die Mover haben die Bezeichnung *AT9011-0050*.

<div align="center">
	<img decoding="async" src="../../images/XTS/AnordnungModule.png" alt="Modulanordnung" width="900" height="280"> <br>
	 Anordnung der Module des verbauten XTS
</div>

## Stationen

```
POS_ROBOT1			: LREAL := 301.3627;	// position for station 1
POS_LED				: LREAL := 1165;		// position of blue led
POS_ROBOT2_XTS1		: LREAL := 2038.1325;	// position for station 2 at XTS 1
POS_SENSOR			: LREAL := 1750;		// position of sensor
POS_INFRONT_XTS2	: LREAL := 700;			// position wait infront word at XTS 2
POS_ROBOT2_XTS2		: LREAL := 1462.0169;	// position for station 2 at XTS 2
POS_BEHIND_XTS2		: LREAL := 2100;		// position wait behind word at XTS 2
POS_LINECAM_START	: LREAL := 2268;		// start position for line camera
POS_ROBOT3			: LREAL := 3114.4173;	// position for station 3	
```


## Mover-Montage
Für verschiedene Anwendungen kann es nötig sein, dass Mover von einem XTS entfernt oder hinzugefügt werden müssen. Das kann an den Schleusen des XTS passieren. Hier befinden sich Löcher in der Führungsschiene.

Sie benötigen folgendes Werkzeug:

- Aufgleishilfe
- Innensechskantbit SW 2,5
- Drehmomentenschlüssel
- Messschieber

>[!WARNING] Warnung!
>Es besteht Quetschgefahr durch starke magnetische Anziehung. Das Magnetplattenset der Mover und die Module des XTS ziehen sich stark magnetisch an. Bei unachtsamen Verhalten können Verletzungen an Händen und Fingern oder Beschädigungen am System entstehen. 
>
>- Halten Sie den Mover beim Aufgleisen immer mit beiden Händen fest.

=== "Vorbereitung"

	Führen Sie zur Vorbereitung folgende Schritte durch:

	1. Stellen Sie sicher, dass der Demonstrator ausgeschaltet ist.
	2. Entfernen Sie die Schrauben an der Schleuse.
	3. Entfernen Sie die Schleuse.
	4. Setzen Sie die Aufgleishilfe auf.
	5. Schrauben Sie die Aufgleishilfe mit den eben gelösten Schrauben fest.

=== "Mover montieren"

	Um einen Mover zu montieren, müssen Sie folgende Schritte ausführen:

	1. Positionieren Sie den Mover mittig über Aufgleishilfe.
	2. Drehen Sie den Mover so, dass die Öffnung parallel zur Führungsschiene und die Geberfahne nach oben zeigt.
	3. Drehen Sie den Mover leicht über eine Seite in die Führungsschiene. Die magnetische Anziehungskraft wird die Mover in Richtung Modul ziehen. Achten Sie darauf, dass die Führungsrollen dabei nicht auf die Kanten der Aufgleishilfe gedrückt werden.
	4. Schieben Sie den Mover entlang der Führungsschiene aus der Aufgleishilfe.
	5. Prüfen Sie mit dem Messschieber, ob der Luftspalt zwischen den Magnetplatten des Movers und den Modulen auf beiden Seiten symmetrisch ist und ungefähr *0,85 mm* beträgt.
	6. Prüfen Sie mit dem Messschieber, ob der Abstand der Geberfahne zum Modul ungefähr *0,90 mm* beträgt.

=== "Mover entfernen"

	Um einen Mover zu entfernen, müssen Sie folgende Schritte ausführen:

	1. Schieben Sie den Mover mittig auf die Aufgleishilfe. Die magnetische Anziehungskraft wird den Mover an das Modul ziehen.
	2. Drücken Sie den Mover mit beiden Händen auf einen leichten Abstand zum Modul.
	3. Ziehen Sie den Mover mit beiden Händen über eine Seite der Aufgleishilfe von der Füh-rungsschiene. Achten Sie darauf, dass die Führungsrollen dabei nicht an die Kanten der Aufgleishilfe gedrückt werden.

=== "Nachbereitung"
	Führen Sie folgende abschließende Schritte durch:

	1. Entfernen Sie die Schrauben aus der Aufgleishilfe.
	2. Entfernen Sie die Aufgleishilfe.
	3. Setzen Sie die Schleuse ein.
	4. Schrauben Sie die Schleuse mit den Schrauben mit einem Drehmoment von *3 Nm* fest.
