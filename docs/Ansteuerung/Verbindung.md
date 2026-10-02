# Verbindung aufbauen
Um mit dem Programmieren starten zu können, müssen Sie zunächst eine Verbindung von Ihrem Gerät zum Demonstrator aufbauen. Das wird Ihnen auf dieser Seite erklärt.

<div class="grid cards" markdown>

-   **Daten zum PC des Demonstrators**

    ---

    Name: CP-2C71E2

	IP-Adresse: 169.254.86.55

	Benutzername: Administrator

	Passwort: 1

</div>

## Vorbereitung

<div class="grid cards" markdown>

-   :material-lan-connect:{ .lg .middle } **TwinCAT Grundlagen**

    ---
    Erklärung der Software und Basics der SPS-Programmierung

    [:octicons-arrow-right-24: Anleitung öffnen](../Grundlagen/TwinCAT.md)

-   :material-download:{ .lg .middle } **TwinCAT Installation**

    ---
    Software über den Package-Manager herunterladen

    [:octicons-arrow-right-24: Anleitung öffnen](./TwinCAT-Installation.md)

</div>

## Inbetriebnahme

### Vorbereitung

> [!WARNING] Wichtig!
> - Der Zutritt zum umzäunten Bereich und die Arbeit am Demonstrator erfolgt ausschließlich mit **Erlaubniss** der Mitarbeitenden der Hochschule.
> - Bevor Sie am Demonstrator arbeiten dürfen, müssen Sie eine **Einweisung** durch die zuständigen Mitarbeitenden erhalten.
> - Geben Sie einem/r Mitarbeitenden bescheid, wenn Sie mit der Arbeit am Demonstrator beginnen. 
> - Betreiben Sie den Demonstrator immer unter **Aufsicht**.
> - Es gilt die Laborordnung für das Labor *Industrielle Fertigung*.

1. Bitten Sie eine/n Mitarbeiter:in der HSBI um Erlaubnis am Demonstrator arbeiten zu dürfen.
2. Haben Sie eine Erlaubnis, geben Sie Bescheid, dass Sie mit dem Arbeiten am Demonstrator beginnen.
3. Drehen Sie den Türkopplungsschalter an der Außenseite des Schaltschranks nach unten, sodass die Spitze auf `On` steht. Hierfür benötigen Sie etwas Kraft.
4. Überprüfen Sie:
	- Sind die Mover frei bewegbar?
	- Sind alle unnötigen Gegenstände im abgezäunten Bereich, insbesondere auf dem Demonstrator entfernt?
	- Sind die Buchstaben unter Kamera 2 entfernt?
5. Legen Sie den mobilen Not-Halt außerhalb des Gitters bereit.
6. Legen Sie zum Programmieren außerhalb des Gitters das Ethernet-Kabel bereit. 
7. Entfernen Sie alle Personen aus Gitterbereich.
8. Schließen Sie die Gittertür.
9. Verbinden Sie das Ethernet-Kabel mit Ihrem Gerät.

### Verbindung aufbauen

=== "Über TwinCAT"
	1. Öffnen Sie ein TwinCAT Projekt.
	2. Wechseln Sie in der Konfig-Modus, beispielsweise durch das Klicken auf das blaue Symbol in der oberen Leiste.
	3. Klicken Sie in der oberen Leiste auf den Eintrag `Lokal`.
	4. Klicken Sie in der Auswahl auf `Zielsystem wählen...`.
	5. Klicken Sie in dem neuen Fenster auf der rechten Seite auf `Suchen (Ethernet)...`.
	6. Klicken Sie oben rechts auf `Broadcast Suche`.
	7. Drücken Sie im neuen Fenster auf OK.
	8. Der Demonstrator sollte nun als neuer Eintrag angezeigt werden. Klicken Sie diesen an.
	9. Klicken Sie in dem Fenster unten auf `Route hinzufügen`.
	10. Geben Sie Benutzernamen und Passwort ein und drücken Sie auf OK.
	11. Schließen Sie das Fenster.
	12. Wählen Sie in dem verbleibenden Fenster den Namen des Demonstrators aus.
	13. Drücken Sie auf OK.

=== "Über Routes"
	1. Klicken Sie in der Symbolleiste Ihres Geräts auf das TwinCAT-Symbol.
	2. Klicken Sie auf *Router/Routes editieren*.
	3. Klicken Sie auf `Add...`.
	4. Wählen Sie im neuen Fenster in der unteren Leiste `Advanced Settings` aus.
	5. Wählen Sie hier unter *Address Info* `IP Address` aus.
	6. Klicken Sie in der oberen Leiste `Broadcast Search` aus und bestätigen Sie mit OK.
	7. Jetzt sollte in der Liste der PC des Demonstrators auftauchen. Klicken Sie den Eintrag an und klicken Sie dann auf `Add Route`.
	8. Haben Sie jetzt bei dem Eintrag unter *Connected* einen Haken, ist die Verbindung aufgabaut. Sie können beide Fenster mit einem Klick auf `Close` schließen.

=== "Erneut verbinden"
	Wenn Sie sich bereits ein anderes Mal mit dem Demonstrator verbunden haben, ist die Route noch eingespeichert. Haben Sie in einem Projekt bereits auf dem Demonstrator gearbeitet, müssen Sie keine Änderungen vornehmen.

