# Greifer

| Bezeichnung		 | GPD5004N		   |
| ------------------ | --------------- |
| Typ				 | 3-Backen-Zentrischgreifer |
| Hersteller 		 | [Zimmer Group](https://www.zimmer-group.com/de/)	   |
| Anzahl             | 3               |
| Gewicht            | 0,27 kg         |
| Druckluft          | 3 bis 8 bar     |
| Greifkraft         | 500 N           |
| Greifzeit          | 0,025 s         |
| Betriebstemperatur | -10°C bis +90°C |

> Weitere Infos finden Sie im **User Manual** oder dem **Data Sheet** des Herstellers online oder im ILIAS-Kurs.

## Aufbau
Damit die Greifer die runden Produkte sicher greifen können, wurde sich für einen Greifer mit drei Backen entschieden. Die Greifer werden per Druckluft angesteuert. Die Greifer verfügen zusätzlich über Magnetfeld- und Induktive Sensoren. Der Aufbau der Greifer ist wie folgt:

1. **Zwangsgeführtes Keilhakengetriebe**: synchronisiert Bewegungen der Greifbacken
2. **Greifbacke**: wird hier montiert
3. **Klemmbock**: Aufnahme für den induktiven Näherungsschalter
4. **Integrierte Greifkraftsicherung**: Feder als Energiespeicher
5. **Abfragenut**: Befestigung der Magnetfeldsensoren
6. **Befestigung und Positionierung**: für Montage am Flansch des Roboters
7. **Antrieb**: doppelwirkender Pneumatikzylinder
8. **Steel Linear Guide**: Platz für überlange Greifbacken
9. **Doppellippendichtung**: Verhinderung Fettaustritt (IP64)

<div align="center">
	<img decoding="async" src="../../images/robot/Greifer-Aufbau.png" alt="Greifer-Aufbau" width="400" height="360"> <br>
	 Aufbau des Greifers [aus <a href="https://www.zimmer-group.com/de/produkte/komponenten/handhabungstechnik/3-backen-zentrischgreifer/serie-gpd5000/produkte/gpd5004n-00-a">Zimmer Group</a>]
</div>

## Funktionsweise
Der Greifer ist in der Variante "Feder öffnend" verbaut. Der Antrieb besteht aus einer Platte, welche über eine Verbindung das Keilhakengetriebe ansteuert. Zwischen der Platte des Antriebs befindet sich eine Feder. Wird Druckluft in den Antrieb eingeführt, so senkt sich die Platte des Antriebs ab, das Keilhakengetriebe wird nach unten und die Feder zusammen gedrückt. Somit schließen sich die Greifbacken (siehe folgende Abbildung B). Wird die Druckluft wieder abgelassen, so drückt die Feder die Platte des Antriebs und somit auch das Getriebe wieder nach oben. Das öffnet die Greifbacken (siehe folgende Abbildung A). Durch die Feder wird der Greifer also automatisch in der offenen Position gehalten.

<div align="center">
	<img decoding="async" src="../../images/robot/GreiferFunktionsweise.png" alt="Greifer-Funktionsweise" width="500" height="360"> <br>
	 Funktionsweise des Greifers (A: Normalzustand, B: Druckluftzufuhr)
</div>
