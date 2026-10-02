# Lichtschranken

| Bezeichnung			| WTB8-P2211 (6033215)								|
| --------------------- | ------------------------------------------------- |
| Typ					| Laser-Abstandssensor								|
| Hersteller			| Sick												|
| Anzahl                | 4                                                 |
| Funktionsprinzip      | Reflexions-Lichttaster mit Hintergrundausblendung |
| Abmessung (B x H x T) | 11 x 31 x 20 mm                                   |
| Schaltabstand         | 20 - 100 mm                                       |
| Wellenlänge           | 650 nm                                            |
| Orange LED			| Aktiver Schaltausgang								|
| Grüne LED				| Stabilitätsanzeige								|

> Weitere Infos finden Sie auch im **Data Sheet** des Herstellers online oder im ILIAS-Kurs.

## Funktionsprinzip
Lichtschranken bestehen aus einer Lichtquelle, dem Sender, und mindestens einem Sensor für dieses Licht, dem Empfänger. Sender und Empfänger befinden sich im selben Gehäuse parallel zueinander. Der Sender sendet das Licht aus dem Gehäuse hinaus. Dieses wird von dem zu detektierende Objekt reflektiert und so an den Empfänger zurückgesendet. Dabei wird die Entfernung über die Zeit berechnet, die das Licht benötigt, um vom Sender zum Empfänger zu gelangen. Dieses Messprinzip wird auch **Time of Flight (ToF)** genannt.

Durch die Hintergrundausblendung ist die Lichtschranke in der Lage dunkle Objekte vor hellem Hintergrund zu erkennen. Dies wird ermöglicht, indem zwei Empfänger untereinander angeordnet sind und somit eine Differenz der Energie des empfangenen Lichts berechnet wird.

## Einstellmöglichkeiten
Sind die Ausgabewerte einer Lichtschranke falsch, kann der Sensor über Drehknöpfe an der Oberseite richtig eingestellt werden. Sie benötigen folgendes Werkzeug:

- Kreuzschlitzschraubendreher
- Produkt oder anderen Gegenstand

<div class="xts-numbered-figure" markdown>
<div align="center">
	<img decoding="async" src="../../images/Lichtschranken-Einstellung.png" alt="Lichtschranken Einstellung" width="200" height="200"> <br>
	 Einstellmöglichkeiten der Lichtschranken [adaptiert aus <a href="https://www.sick.com/media/pdf/5/45/645/dataSheet_WTB8-P2211_6033215_en.pdf">Sick</a>]
</div>
<div class="xts-numbered-figure__numbers" markdown>

1. LED-Anzeige grün: Stabilität
2. LED-Anzeige orange: Schaltausgang aktiv
3. Einstellung Schaltabstand
4. Hell-/Dunkeldrehschalter: L = hellschaltend, D = dunkelschaltend

</div> </div>

=== "Schaltabstand verändern"
	Hier ist ein Beispiel für die Einstellung des Schaltabstandes bei hellschaltendem Sensor. Führen Sie folgende Schritte durch:

	1. Schalten Sie den Demonstrator ein.
	2. Legen Sie das Produkt oder den Gegenstand in gewünschtem Abstand vor den Sensor.
	3. Leuchtet die orange LED, können Sie diesen Schritt überspringen. Drehen Sie gegen den Uhrzeigersinn an der Stellschraube (3), bis die orange LED.
	4. Entfernen Sie das Produkt oder den Gegenstand.
	5. Leuchtet die orange LED nicht mehr, können Sie diesen Schritt überspringen. Drehen Sie im Uhrzeigersinn an der Stellschraube (3), bis die orange LED aus ist.
	6. Wiederholen Sie die Schritte 1 bis 4, bis die orange LED die gewünschten Werte ausgibt.

=== "Hell-/Dunkelschaltung ändern"
	Soll der Sensor aktiv sein, wenn ein Gegenstand davor liegt, muss der Sensor hellschaltend eingestellt sein. Drehen Sie dazu die Stellschraube (4) bis zum Anschlag auf der linken Seite (L). Soll der Sensor deaktiviert sein, wenn ein Gegenstand davor liegt, muss der Sensor dunkelschal-tend eingestellt sein. Drehen Sie dazu die Stellschraube (4) bis zum Anschlag auf der rechten Seite (D).
