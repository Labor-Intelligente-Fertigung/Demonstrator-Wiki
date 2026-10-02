# Ansteuerung
Hier finden Sie Anleitungen zum Starten des Demonstrationsprogramms sowie der Neu- oder Umprogrammierung des Demonstrators und seiner Komponenten über einen eigenen Entwicklungs-Laptop oder -PC. 

> [!WARNING] Wichtig!
> Bevor Sie am Demonstrator arbeiten dürfen, müssen Sie eine Einweisung durch die zuständigen Mitarbeitenden der Hochschule erhalten.
> 
> Lesen Sie sich dazu die *Betriebsanleitung* aufmerksam durch! Sie enthällt sicherheitsrelevante Informationen, ohne dessen Kentnisse das Arbeiten am Demonstrator nicht gestattet ist. Als Zusammenfassung hängt eine *Betriebsanweisung* am Zaun des Demonstrators aus.

## Demonstration

=== "Vorbereitung"
    1. Bitten Sie eine/n Mitarbeiter:in der Hochschule um Erlaubnis am Demonstrator arbeiten zu dürfen.
    2. Haben Sie eine Erlaubnis, geben Sie Bescheid, dass Sie mit dem Arbeiten am Demonstrator beginnen.
    3. Drehen Sie den Türkopplungsschalter an der Außenseite des Schaltschranks nach unten, sodass die Spitze auf `On` steht. Hierfür benötigen Sie etwas Kraft.
    4. Überprüfen Sie:
        - Sind die Mover frei bewegbar?
        - Sind alle unnötigen Gegenstände im abgezäunten Bereich, insbesondere auf dem Demonstrator entfernt?
        - Sind die Buchstaben unter Kamera 2 entfernt?
    5. Legen Sie den mobilen Not-Halt außerhalb des Gitters bereit.
    6. Entfernen Sie alle Personen aus Gitterbereich.
    7. Schließen Sie die Gittertür.

=== "Starten"
    1. Verriegeln Sie die Tür, indem Sie 2 Mal den mittleren Knopf der Türverriegelung drücken.
    2. Quittieren Sie, indem Sie den oberen Knopf der Türverriegelung drücken.
    3. Klicken Sie in der HMI auf `START`.
    4. Geben Sie ein Wort ein, das Sie produziert haben möchten.

=== "Ausschalten"
    1. Stoppen Sie den Demonstrator über die HMI.
    2. Entriegeln Sie die Gittertür über die Knöpfe der Türverriegelung.
    3. Öffnen Sie die Gittertür.
    4. Drehen Sie den Türkopplungsschalter am Schaltschrank auf `Off`.

=== "Tipps"
    Wenn der Demonstrator beim Drehen des Türkopplungsschalters nicht an geht, überprüfen Sie:

    - die Sicherung `Maschinen` am Kasten neben der Labortür und schalten Sie sie ein.
    - die Not-Aus-Schalter am Demonstrator und im Labor sowie den FI-Schalter im Sicherungskasten neben der Labortür.

    Können Sie die Türverriegelung nicht Quittieren, ist vermutlich einer der Not-Halt-Schalter gedrückt. Lösen Sie diesen.

## Anleitungen zur Programmierung

<div class="grid cards" markdown>

-   :material-download:{ .lg .middle } **TwinCAT Installation**

    ---
    Software über den Package-Manager herunterladen

    [:octicons-arrow-right-24: Anleitung öffnen](./TwinCAT-Installation.md)

-   :material-lan-connect:{ .lg .middle } **Erste Schritte**

    ---
    Verbindung vom Laptop zum Demonstrator herstellen und TwinCAT starten

    [:octicons-arrow-right-24: Anleitung öffnen](./Verbindung.md)

-   :material-robot-industrial:{ .lg .middle } **Einrichtung der Komponenten**

    ---
    Einbinden der Komponenten in das TwinCAT-Projekt

    [:octicons-arrow-right-24: Anleitung öffnen](./Einrichtung/index.md)

-   :fontawesome-solid-laptop-code:{ .lg .middle } **SPS Implementierung**

    ---
    Programmierung der Funktionsweisen der Komponenten in der SPS

    [:octicons-arrow-right-24: Anleitung öffnen](./SPS/index.md)

-   :material-connection:{ .lg .middle } **Verknüpfungen**

    ---
    Übersicht über Geräte, Klemmen und deren Ein- und Ausgänge

    [:octicons-arrow-right-24: Anleitung öffnen](./Ubersicht-Verknupfungen.md)

</div>