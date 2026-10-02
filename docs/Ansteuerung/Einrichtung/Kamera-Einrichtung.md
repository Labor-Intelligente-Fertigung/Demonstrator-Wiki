# Kamera Einrichtung
Hier wird erklärt, wie Sie die Kameras in ein bestehendes TwinCAT-Projekt einbinden können.

>Grundlagen bezüglich Bildverarbeitung finden Sie [hier](../../Grundlagen/Vision.md).
>Weitere Hintergrundinformationen gibt es zur [Kamera 1](../../Komponenten/MantaG125-AlliedVision.md), zur [Kamera 2](../../Komponenten/GenieNano-Teledyne.md) und zur [Kamera 3](../../Komponenten/LineaGigE-Teledyne.md).

## Vorbereitung
1. Starten Sie den Demonstrator.
2. Öffnen Sie Ihr TwinCAT-Projekt und stellen Sie das Zielsystem passen ein (Anleitung dazu finden Sie [hier](../index.md)).
3. Wenn Sie noch keine Geräte eingefügt haben, scannen Sie jetzt nach Geräten.
4. Wenn in Ihrem Projektmappen-Explorer noch kein Eintrag "VISION" vorhanden ist, klicken Sie per Rechtsklick im Explorer auf das Projekt und klicken Sie "VISION" unter `Versteckte Konfigurationen zeigen` an.

<div align="center">
	<img decoding="async" src="../../../images/camera/VersteckteKonfigurationen.png" alt="Versteckte Konfigurationen" width="450"> <br>
	 Versteckte Konfigurationen zeigen
</div>

## Vision Application erstellen
1. Klicken Sie per Rechtsklick im Projektmappen-Explorer auf "VISION" und fügen Sie ein neues Element hinzu.
2. Klicken Sie mit einem Rechtsklick auf die neue Application und fügen Sie auch hier ein neues Element hinzu.

	<div align="center">
		<img decoding="async" src="../../../images/camera/ApplikationHinzufuegen.png" alt="Applikation hinzufuegen" width="300"> <br>
		Applikation hinzufügen
	</div>

3. Wählen Sie die GigEVision Kamera aus und benennen Sie diese im aufkommenden Fenster und bestätigen Sie mit OK.
4. Jetzt muss die Kamera verknüpft werden. Entweder befindet sie sich bereits im Baum oder sie kann durch das Klicken auf `Neu...` hinzugefügt werden.

	<div align="center">
		<img decoding="async" src="../../../images/camera/NeueKamera.png" alt="Neue Kamera einbinden" width="450"> <br>
		Neue Kamera einbinden
	</div>

5. Falls Sie die Kamera neu hinzufügen wollen, klicken Sie im neuen Fenster nochmal auf `Neu...`
6. Wählen Sie hier die Kamera aus und bestätigen Sie mit OK. Jetzt sollte die Kamera im Hauptbaum erscheinen und dessen *IpStack* kann von Ihnen ausgewählt werden.

	<div align="center">
		<img decoding="async" src="../../../images/camera/ipStack.png" alt="IpStack einbinden" width="450"> <br>
		IpStack einbinden
	</div>

7. In einem neuen Fenster öffnet sich der Kamera Initialisierungs-Assistent. Klicken Sie hier auf `Discover Devices`. Anschließend sollte die Kamera in der Liste erscheinen.
8. Wählen Sie die Kamera aus und klicken Sie dann auf `Setze IP Adresse für Gerät...`. Bestätigen Sie mit OK.
9. Die Kamera kann jetzt ausgewählt und mit OK bestätigt werden.

	<div align="center">
		<img decoding="async" src="../../../images/camera/SelectCamera.png" alt="Kamera auswählen" width="450"> <br>
		Kamera auswählen
	</div>

## Kamera Reiter
Klicken Sie in "VISION" per Doppelklick auf die eben erstellte Kamera. Folgende Reiter stehen Ihnen jetzt zur Verfügung:

### General
Hier können Sie die Verbindung überprüfen und generelle Informationen zur Kamera, wie den Hersteller, Modellname und IP-Adresse, auslesen.

<div align="center">
	<img decoding="async" src="../../../images/camera/General.png" alt="Reiter General" width="350"> <br>
	Reiter General
</div>

### Configuration Assistant
Befindet sich TwinCAT im Konfig-Modus, dann können hier Parameter der Kamera, wie der Bildausschnitt oder die IP-Adresse, geändert werden. Zusätzlich kann hier durch das Klicken auf `Start Acquisition` ein Probebild aufgenommen werden.

<div align="center">
	<img decoding="async" src="../../../images/camera/KonfigurationAssistant.png" alt="Configuration Assistant" width="500"> <br>
	Reiter Configuration Assistant
</div>

### Recors/Playback

Hier können Kamera-Streams aus dem Online-Modus aufgezeichnet und später in die SPS eingespeist werden. Das kann beispielsweise verwendet werden, wenn die Kamera nicht immer online zur Verfügung steht. Das reale Bildverhalten der Kamera wird also mit original Bildern simuliert.

### Camera Calibration
In diesem Reiter kann eine geometrische Kalibrierung von Flächenkameras mit Hilfe eines Kalibrierungsmusters vorgenommen werden.
