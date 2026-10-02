# Sicherheit
Im Folgenden werden geltende Normen und andere Grundlagen der Sicherheitsmaßnahmen am Demonstrator erklärt.

## Normen
Normen sind allgemein anerkannte Regeln. Sie können formell sein, wie Gesetze, Verordnungen oder Normen von DIN, ISO oder IEC, oder informell, wie gesellschaftliche Erwartungen an Höflichkeit, Pünktlichkeit oder Fairness. Normen entstehen durch den Austausch von Experten, Unternehmen, Wissenschaft und Politik. Normungsorganisationen wie DIN, ISO, IEC oder CEN arbeiten daran, klare Regeln auszuarbeiten, damit Produkte, Dienstleistungen und Prozesse weltweit oder regional zusammenpassen. Sie reduzieren dadurch Unsicherheiten, erhöhen Sicherheit und Qualität, schaffen Vertrauen und erleichtern Handel, Produktion und Innovation.

Normen wie DIN oder ISO müssen in der Regel nicht gesetzlich befolgt werden. Sie sind üblicherweise freiwillig. Doch es gibt Ausnahmen, bei denen Normen verbindlich sind oder eine wichtige Rolle spielen. In bestimmten Bereichen schreiben Gesetze, Verordnungen oder technische Regelwerke vor, dass bestimmte Normen einzuhalten sind. Dann ist die Umsetzung verpflichtend, und Verstöße können rechtliche Folgen haben. Daher sollte immer überprüft werden, ob eine Norm gesetzlich, vertraglich oder durch eine bestimmte Regulierung relevant ist.

## Schutzmaßnahmen
Nach *DIN EN ISO 12100* muss mit Hilfe einer **Risikoanalyse** vor dem ersten Einsatz die Gefährdung durch die Maschine bewertet werden. Als Tool dafür kann beispielsweise eine **FMEA** (eng. *Failure Mode and Effects Analysis*) verwendet werden. Basierend auf der Analyse kann ein erforderliches **Performance Level** (aus *DIN EN ISO 13849-1*), kurz **PLr**, bestimmt werden, welches die nötige Leistungsfähigkeit von Schutzmaßnahmen definiert. Es kann Werte von *a* bis *e* haben, wobei *e* das höchste ist.

> [!INFO] Info
> Für den efa-Demonstrator wurde ein erforderliches Performance Level (PLr) *d* festgelegt. Das bedeutet, dass Verletzungen, welche einem Menschen durch den Demonstrator zugefügt werden, normalerweise irreversibel oder tödlich sind.

Vor Inbetriebnahme müssen dann meist **Schutzeinrichtungen** eingebaut werden, um Menschen vor von der Maschine ausgehenden Gefahren zu schützen. Es wird hierbei zwischen *trennenden* (bspw. Gitter, siehe *DIN EN ISO 14120*) und *nichttrennenden Schutzeinrichtungen* (bspw. Sensor, siehe *DIN EN ISO 13855*) unterschieden. Im Folgenden werden die am efa-Demonstrator befindlichen Schutzeinrichtungen erklärt.

### Gitterzaun und Türverrigelung
Der Gitterzaun schafft einen klar definierten, gut sichtbaren Schutzbereich um die Gefahrenzone, wobei die Türverriegelung den Zugang dorthin regelt. Der Zaun ist nach Richtlinien der Norm *DIN EN ISO 14120* gebaut, welche den Mindestabstand zur Gefährdung, die notwendige Höhe, um ein Herüberreichen zu verhindern, und vieles mehr definiert. Da der Demonstrator trotz des Zauns erreichbar bleibt, ist eine Tür verbaut. An dieser befindet sich eine Verriegelung, welche dem notwendigen Performance Level entspricht. Läuft der Demonstrator, kann die Tür nicht einfach geöffnet werden. Über die Knöpfe an der Tür muss das Öffnen zunächst angefragt werden. Daraufhin stoppen die Komponenten. Öffnet sich die Tür, ist der Demonstrator also gestoppt und unbeabsichtigtes Anlaufen wird verhindert, solange sich Personen im Gefährdungsbereich befinden. Wird die Tür wieder geschlossen und verriegelt, kann der Demonstrator wieder starten.

### Not-Aus und Not-Halt
**Not-Aus** beschreibt die Notabschaltung der Spannungsversorgung. In der Regel wird eine solche Abschaltung durch das Bedienpersonal im akut gefährlichen Moment betätigt. Ziel ist ein sofortiges Stoppen der gefährlichen Bewegung mit einer klaren Freigabe/Reset‑Prozedur, um die Inbetriebnahme wieder zu ermöglichen. Im Fall des Demonstrators benötigt es einen kompletten Neustart.

Der Not-Aus entspricht der **Stopp-Kategorie 0** nach *DIN EN ISO 13850*. Der Stillstand erfolgt in der Regel durch unmittelbares Abschalten der elektrischen Energie zu den Motoren der Maschine. Es gibt keine garantierte, kontrollierte Abbremsung oder definierte Endlage. Nach dem Stopp kann noch Restenergie in Bauteilen oder Speichern vorhanden sein. 

**Not-Halt** meint den sicherheitsrelevanten Zustand, um eine gefährliche Bewegung zu stoppen. Im Gegensatz zum Not-Aus kann der Not-Halt auch automatisch durch sicherheitsgerichtete Einrichtungen ausgelöst werden, wenn beispielsweise eine Sicherheitsvorrichtung (z. B. Lichtschranken, Türsicherungen) ausgelöst wird. Der Fokus liegt auf dem sicheren Haltezustand der Maschine.

Der Not-Halt entspricht der **Stopp-Kategorie 1** nach *DIN EN ISO 13850*. Er sorgt für einen kontrollierten Stillstand. Die Antriebe oder die Steuereinheit bremsen die Bewegung gezielt ab. Anschließend wird die Energiezufuhr unterbrochen.

## Roboter-Räume
In der Norm *DIN EN ISO 10218-1* sind die Bereiche um einen Roboter definiert. In der folgenden Abbildung der Norm ist A) Roboter, B) Endeffektor, C) Werkstück und D) Schutzeinrichtung (hier LiDAR).

<div align="center">
	<img decoding="async" src="../../images/robot/BereicheRoboter-ISO10218-1.png" alt="Roboterbereiche ISO 10218-1" width="550" height="300"><br>
	 Darstellung der Räume [aus DIN EN ISO 10218-1:2021 - Robotik Sicherheitsanforderungen, S. 72]
</div>

1. **Maximaler Raum**: Bereich, der von den beweglichen Teilen des Roboters erreicht werden kann (inklusive Greifer und Werkstück)
2. **Eingeschränkter Raum**:  Teil des maximalen Raums, der durch Begrenzungseinrichtungen (bspw. Achs- und Raumbegrenzungen) reduziert ist
3. **Arbeitsraum**: Teil des eingeschränkten Raums, der während der Ausführung der Aufgabe benutzt wird
4. **Geschützer Raum**: Bereich, in dem Schutzeinrichtungen aktiv sind oder äußere Schutzeinrichtungen Schutz bieten

---

## Literaturempfehlung
Die folgende Literatur ist online über die Hochschulbibliothek oder per Open Access verfügbar.

- Belcaro, P. L. (2025): _9-Object-Model FMEA: Ein Denkmodell zur Erstellung von technischen Risikoanalysen_ (1st ed. 2025), Springer Fachmedien Wiesbaden, Imprint: Springer Vieweg. [https://doi.org/10.1007/978-3-658-45745-7](https://doi.org/10.1007/978-3-658-45745-7)

Folgende Normen sind über die Hochschulbibliothek zur Verfügung gestellt. Für die Leseberechtigung müssen Sie per VPN verbunden sein.

- DIN EN ISO 12100: [https://nautos.de/K53/search/item-detail/DE30104211](https://nautos.de/K53/search/item-detail/DE30104211)
- DIN EN ISO 13849-1: [https://nautos.de/K53/search/item-detail/DE30098384](https://nautos.de/K53/search/item-detail/DE30098384)
- DIN EN ISO 14120: [https://nautos.de/K53/search/item-detail/DE30060519](https://nautos.de/K53/search/item-detail/DE30060519)
- DIN EN ISO 13855: [https://nautos.de/K53/search/item-detail/DE30104424](https://nautos.de/K53/search/item-detail/DE30104424)
- DIN EN ISO 13850: [https://nautos.de/K53/search/item-detail/DE30062386](https://nautos.de/K53/search/item-detail/DE30062386)
- DIN EN ISO 10218-1: [https://nautos.de/K53/search/item-detail/DE30090868](https://nautos.de/K53/search/item-detail/DE30090868)