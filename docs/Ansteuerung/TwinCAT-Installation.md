# TwinCAT-Installation
Als Software für die SPS-Programmierung wird die Software TwinCAT verwendet. Sie müssen die Software mit einigen zusätzlichen Bibliotheken auf Ihrem Entwicklungs-Laptop oder PC herunterladen, wenn Sie an der Programmierung des Demonstrators arbeiten wollen.

> TwinCAT Grundlagen können Sie [hier](../Grundlagen/TwinCAT.md) nachlesen. 

## Benötigte Bibliotheken

<div class="efa-highlight" markdown>

- TwinCAT Standard Engineering 4026.17
- TC1000 ADS 1.0.0
- TF5400 Advanced Motion Pack 3.3.77
- TF5000 NC PTP 1.0.3
- TF5100 NCI 1.0.3
- TF5850 XTS 4.3.4
- TF7100 Vision 5.8.4
- TF5113 Kinematic Transformation L4
- (TC1700 Usermode Runtime) *wenn Lokal gearbeitet werden soll*
</div>

> [!INFO] Information
> Die Bibliothek `TF5113 Kinematic Transformation L4` unterliegt einer besonderen Lizenz und kann daher nicht über den Package Manager oder die Webseite heruntergeladen werden.
> Die Bibliothek muss in den Pfad:
> ```
> C:\ProgramData\Beckhoff\TcPkg\lib
> ```
> Sie kann dann über den Package Manager als einzelnes Paket installiert werden.

## Anleitung
> [!NOTE] Hinweis
> Die Schritte können aufgrund von neueren Software-Versionen abweichen.

=== "Neuinstallation"

    Haben Sie TwinCAT noch nicht auf Ihrem Gerät installiert, so können Sie folgende Schritte zur Installation ausführen:

	1. Laden Sie sich die Installationsdatei des *Package Manager* der Firma Beckhoff [hier](https://www.beckhoff.com/de-de/support/downloadfinder/suchergebnis/?c-1=26782567) herunter. Dazu müssen Sie sich einen kostenlosen Account auf der Seite erstellen.
	2. Installieren Sie den *Package Manager*.
	3. Starten Sie den *Package Manager*.
	4. Klicken Sie auf `Next`. 
	5. Als Feed können Sie den eingestellten Feed von Beckhoff eingestellt lassen. Sie müssen sich mit Ihrem Beckhoff Account anmelden. 
	6. Klicken Sie auf `Next`.
	7. Wählen Sie aus, welche Art der Installation Sie haben wollen (empfohlen: TwinCAT XAE Shell 64).
	8. Klicken Sie auf `Finish`.
	9. Wählen Sie die Bibliotheken in den entsprechenden Versionen zum Download aus, indem Sie eine Bibliothek anklicken und in den Fenster auf der rechten Seite die richtige Version suchen.
	10. Nachdem Sie alle Bibliotheken ausgewählt haben, klicken Sie in der rechten Leiste auf das Symbol `Selected Products` und starten Sie hier die Installation.
	11. Starten Sie Ihr Gerät neu, wenn die Installation beendet ist.

=== "Migration von 4024 auf 4026"

    Haben Sie TwinCAT bereits in der Version 4024 installiert, so müssen Sie mit den folgenden Schritten TwinCAT auf die neue Version updaten:

	1. Laden Sie sich die Installationsdatei des *Package Manager* der Firma Beckhoff [hier](https://www.beckhoff.com/de-de/support/downloadfinder/suchergebnis/?c-1=26782567) herunter. Nutzen Sie dazu Ihren Beckhoff Account.
	2. Installieren Sie den *Package Manager*.
	3. Starten Sie den *Package Manager*.
	4. Klicken Sie auf `Next`. 
	5. Als Feed können Sie den eingestellten Feed von Beckhoff eingestellt lassen. Sie müssen sich mit Ihrem Beckhoff Account anmelden. 
	6. Klicken Sie auf `Next`.
	7. Wählen Sie aus, welche Art der Installation Sie haben wollen (empfohlen: TwinCAT XAE Shell 64).
	8. Klicken Sie auf `Next`.
	9. Migrieren Sie Ihre aktuelle TwinCAT Installation, indem Sie auf Yes klicken und den Anweisungen folgen.
	10.	Klicken Sie auf `Finish`.
	10. Wählen Sie die Bibliotheken in den entsprechenden Versionen zum Download aus, indem Sie eine Bibliothek anklicken und in den Fenster auf der rechten Seite die richtige Version suchen.
	11. Nachdem Sie alle Bibliotheken ausgewählt haben, klicken Sie in der rechten Leiste auf das Symbol `Selected Products` und starten Sie hier die Installation.
	14.	Starten Sie Ihr Gerät neu, wenn die Installation beendet ist.

=== "Software Versionen anpassen"

	Haben Sie TwinCAT bereits in der Version Build 26 installiert, so müssen Sie mit den folgenden Schritten die zusätzlichen Pakete herunterladen:

	1. Starten Sie den *Package Manager*.
	2. Wählen Sie die Bibliotheken in den entsprechenden Versionen zum Download aus, indem Sie eine Bibliothek anklicken und in den Fenster auf der rechten Seite die richtige Version suchen.
	3. Nachdem Sie alle Bibliotheken ausgewählt haben, klicken Sie in der rechten Leiste auf das Symbol `Selected Products` und starten Sie hier die Installation.

=== "Komandozeile"

	Wenn Sie den *Package Manager* installiert haben, können Sie alle Funktionen auch über eine Konsoleneingabe durchführen. Eine ausführliche Anleitung finden Sie [hier](https://infosys.beckhoff.com/content/1031/tc3_installation/15698626059.html?id=2893125353611165226).

	Beispiel Feed hinzufügen:
	```
	tcpkg source add -n ”Beckhoff Stable Feed” -s https://public.tcpkg.beckhoff-cloud.com/api/v1/feeds/stable -u example.user@mail.com
	```
	Auflistung im Feed vorhandener Packages:
	```
	tcpkg list
	```
	Beispiel Package Installation:
	```
	tcpkg install twincat.standard.xae=4026.18.0 TF6100.OpcUaConfigurator.XAE=4.5.7
	```
