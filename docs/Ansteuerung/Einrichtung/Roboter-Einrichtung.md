# Roboter Einrichtung
Hier wird erklärt, wie Sie ein neues TwinCAT-Projekt mit den Robotern anlegen und die Roboterbewegung programmieren können.

>Grundlagen bezüglich Robotik und den verschiedenen Koordinatensystemen finden Sie [hier](../../Grundlagen/Robotik.md). Informationen zu den Robotern erhalten Sie [hier](../../Komponenten/TX40-Staubli.md).

## Vorbereitung

1. Laden Sie sich aus dem ILIAS-Kurs das zip-File "Roboter-Konfig" herunter und entpacken Sie dieses.
2. Öffnen Sie TwinCAT und erstellen Sie ein neues TwinCAT Projekt.
3. Schalten Sie den Demonstrator ein und verbinden Sie die Hardware (siehe [Start](./index.md)).
4. Verbinden Sie sich mit dem Demonstrator als Zielsystem (siehe [Start](./index.md)).

## Achsen einrichten

1. Scannen Sie nach Geräten und fügen Sie die Geräte an den Adaptern *Ethernet* und *Ethernet 6* ein.
2. Fügen Sie die NC-Achsen ein, indem Sie das aufkommende Fenster bestätigen. Diese Achsen sind im ACS angegeben.
3. Sie können nun die eingefügten Achsen unter `Motion` umbenennen und in Ordner sortieren. Die Reihenfolge der Achsen ist: Roboter 1 ACS 1-6, Roboter 2 ASC 1-6, Roboter 3 ASC 1-6.
4. Importieren Sie die Parameter, indem Sie die erste Achse mit einem Rechtsklick auswählen und `XML Parameter importieren...` klicken. 
5. Wählen Sie nun die entsprechende xml-Datei aus dem entpackten Ordner "Roboter-Konfig" aus (Beispiel: `tx40-acs-1.xml` für die erste Achse).
6. Wiederholen Sie Schritt 6 und 7 für alle Achsen aller Roboter.
7. Erstellen Sie für jeden Roboter 6 weitere Achsen, die MCS-Achsen, indem Sie mit einem Rechtsklick auf `Achsen` klicken und anschließend `Neues Element hinzufügen...`.
8. Importieren Sie auch für diese Achsen die Parameter mit Hilfe der xml-Dateien.
9. Stellen Sie bei allen MCS-Achsen unter `Einstellungen` den Achstyp auf `Standard (Verknüpfung über Encoder und Antrieb)` und setzen Sie den Haken bei `Simulation`.

Anschließend sollte es im Projekmappen-Explorer ungefähr so aussehen:

<div align="center">
	<img decoding="async" src="../../../images/robot/TwinCAT-RoboterAchsen.png" alt="RoboterachsenTwinCAT" width="350" height="450"> <br>
	 Ansicht nach Achseinrichtung
</div>

Um die Transformationen vom ACS in MCS zu berechnen, muss die Kinematik der Roboter eingefügt werden. Klicken Sie dazu mit einem Rechtsklick unter `Motion` auf `Achsen` und wählen Sie `Vorhandenes Element hinzufügen...` aus. Wählen Sie nun aus dem Ordner "Roboter-Konfig" nacheinander die drei `TX40_*_KINEMATIC.xti` aus.

## Geräte verknüpfen

1. Löschen Sie beim Gerät unter `E/A` am Adapter *Ethernet 6* die drei uniVAL-Einträge. Sie enthalten fehlerhafte Informationen.
2. Fügen Sie anschließend die richtigen Roboter ein, indem Sie mit einem Rechtsklick auf das Gerät und `Vorhandenes Element hinzufügen...` klicken.
3. Wählen Sie nacheinander so alle drei `ROBOT_* (uniVAL).xti` aus dem Ordner "Roboter-Konfig" aus.
4. Nun müssen Sie die ACS-Achsen erneut mit dem I/O verbinden. Klicken Sie dazu nacheinander auf die Ordner der Roboter-Achsen und markieren Sie die leeren Felder `Link to I/O` der 6 ACS-Achsen. Machen sie auf die obere einen Rechtklick und wählen Sie `Achsen E/A Verknüpfung ändern` aus. Klicken Sie nacheinander die vorgeschlagenen Achsen an.

	<div align="center">
		<img decoding="async" src="../../../images/robot/TwinCAT-RoboterAchsenVerknuepfung.png" alt="RoboterachsenTwinCAT" width="450" height="240"> <br>
		Ansicht nach Achsverknüpfung
	</div>

5. Verknüpfen Sie die uniVAL-Output-Variablen `SetTime` aller 6 Achsen mit der Variablen `NcToPlc.OpModeDWord` der ACS-6 Achse des jeweiligen Roboters. Hierzu müssen Sie in dem Verknüpfungsfenster den Haken bei `Nur unbenutzte` entfernen und bei `Alle Typen` setzen.
6. Da die Variable nicht passend ist, müssen Sie nach dem Auswählen folgende Einstellungen vornehmen: "Überlappend": 8 und "Zugeordnete Var/Offset": 24 

	<div align="center">
		<img decoding="async" src="../../../images/robot/TwinCAT-RoboterSetTime.png" alt="RoboterachsenTwinCAT" width="500" height="400"> <br>
		Einstellungen bei Variablenverknüpfung
	</div>

## Manuelle-Achsbewegung
Nachdem nun die Achsen im Projekt mit der Hardware verbunden worden sind, können diese manuell verfahren werden. Führen Sie dazu folgendes aus:

1. Wechseln Sie in den Online-Modus und loggen Sie sich ein.
2. Schalten Sie alle ACS-Achsen eines Roboters frei. Klicken Sie dazu im Projektmappen-Explorer unter `Motion` die erste ACS-Achse des Roboters per Doppelklick an.
3. Wechseln Sie in den Reiter `Online` und klicken Sie im Bereich *Freigaben* auf `Setzen`.
4. Klicken Sie hier auf den Button `Alle`.
5. Führen Sie Schritt 3 und 4 auch für alle weiteren ACS-Achsen des Roboters aus.

Wechseln Sie nun wieder zu der Achse, die Sie verfahren möchten. Hier können Sie nun durch das Klicken auf F1-4 auf der Tastatur oder den entsprechenden Buttons die Achse verfahren.
