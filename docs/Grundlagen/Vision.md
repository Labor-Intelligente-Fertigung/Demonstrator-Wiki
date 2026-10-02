# Vision
In der Bildverarbeitung werden mehrere Konzepte angewendet, welche im Folgenden beschrieben werden.

## Multi-Channel-Histogramm
Ein Multi-Channel-Histogramm eines Bildes ist eine grafische Darstellung der **Verteilung von Helligkeitswerten oder Farben** innerhalb der verschiedenen Kanäle eines Bildes. In der Regel bezieht sich dies auf Farbbilder, die aus mehreren Farbkanälen bestehen, wie z. B. dem RGB-Farbschema, das Rot, Grün und Blau umfasst. Ein Multi-Channel-Histogramm zeigt die Häufigkeit von Pixelwerten für jeden dieser Kanäle getrennt an. Das bedeutet, dass für ein RGB-Bild drei separate Histogramme erstellt werden. Jedes Histogramm zeigt, wie viele Pixel einen bestimmten Wert in diesem Kanal haben. Durch die Analyse dieser Histogramme können wichtige Informationen über das Bild gewonnen werden, wie z. B. Helligkeitsverteilung, Farbsättigung oder Kontrastverhältnis. Multi-Channel-Histogramme sind daher nützlich in der Bildverarbeitung, insbesondere für Aufgaben wie Bildverbesserung, Klassifizierung und Segmentierung.

Zur Erstellung eines Histogramms $H$ erfolgt zunächst das Laden des Bildes und dessen Aufteilung in Kanäle (bsp. rot, grün, blau). Hier stellen die Pixelwerte die Intensität der jeweiligen Farbe dar. Nun wird gezählt, wie viele Pixel des Bildes $f_E$ an jeder Position $\left(x,y\right)$ die jeweiligen Intensitätswert $i$ haben. Hierbei sind $M$ und $N$ die Breite und Höhe des Bildes.

$$
H(i)=\sum_{x=1}^{M}\sum_{y=1}^{N}\left[f_E\left(x,y\right)=i\right]
$$

Die folgende Abbildung zeigt ein beispielhaftes Histogramm des Rot-Kanals eines Bildes.

<div align="center">
	<img decoding="async" src="../../images/Histogramm.png" alt="Histogramm" width="500" height="150"> <br>
	 Histogramm des Rot-Kanals eines Bildes
</div>

## Bildsegmentierung
Eine Bildsegmentierung wird vorgenommen, um ein **Binärbild** zu erstellen. Die Pixel eines Binärbildes können nur zwei verschiedene Werte annehmen und unterteilt ein Bild meist in Vor- und Hintergrund. Ein Binärbild vereinfacht dadurch die Speicherung und Verarbeitung. Der Ausgang ist ein Graustufenbild. Die Pixel an jeder Position $\left(x,y\right)$ aus dem Ursprungsbild $f_E$ müssen dem Binärbild $f_A$ zugeordnet werden. Den Pixeln werden meist die Werte $0$ oder $1$ zugeordnet, je nachdem, ob sie über einem Schwellwert $T$ liegen.

$$
f_A\left(x,y\right)= \begin{cases} 1,wenn f_E(x,y)≥T & \\ 0,wenn f_E(x,y)<T \end{cases}
$$

## Schwellwertfindung Otsu
Mit Otsu kann der Schwellwert $T$ bestimmt werden, der für die **Bildsegmentierung** verwendet wird. Dafür muss zunächst ein normiertes Histogramm des Bilds erstellt werden. Es werden zwei Normalverteilungen angenommen, dessen Varianzen $\sigma$ und Mittelwerte $\mu$ mit Hilfe des Histogramms berechnet werden können. Anschließend kann der Schwellwert über ein Optimierungskriterium $J\left(T\right)$ gefunden werden. Ist der Wert maximal, so ist der ideale Schwellwert gefunden.

$$
J\left(T\right)=\frac{\left(\mu_1-\mu_2\right)^2}{\sigma_1^2+\sigma_2^2}
$$

Die folgende Abbildung zeigt zwei Normalverteilungen als Beispiel, bei denen der Schwellwert bestimmt wurde.

<div align="center">
	<img decoding="async" src="../../images/Otsu.png" alt="Schwellwertfindung" width="450" height="200"> <br>
	 Schwellwertfindung mit Otsu
</div>

## Satz-von-Green
Der Satz von Green wird zur **Berechnung von Flächen** verwendet. Diese wird über ein Kurvenintegral ausgedrückt.

## Hu-Momente
Hu-Momente oder Hu-Invariante sind eine Sammlung von sieben mathematischen Invarianten, die in der Bildverarbeitung und **Mustererkennung** verwendet werden, um die Form von Objekten in Bildern zu charakterisieren. Diese Invarianten sind unabhängig von der Skalierung, Rotation und Translation des Objekts. Das bedeutet, dass sie helfen können, die Form eines Objekts zu identifizieren, egal wie es im Bild dargestellt wird. Sie können zudem verwendet werden, um festzustellen, wie ähnlich sich zwei Formen sind. Sind die Momente ähnlich, so ähneln sich auch die Objekte.

Die Hu-Momente basieren auf der Berechnung der zentralen Momente (gewichtete Mittelwerte) $\mu_{i,j}$ eines Graustufenbildes. Zunächst muss der Schwerpunkt $(\bar{x},\bar{y})$ des Bildes berechnet werden. Dieses wird aus den nicht zentrierten Momenten $M_{i,j}$ berechnet. Die Formel bildet sich zudem aus der Position $\left(x,y\right)$ im Bild. Die Funktion $f\left(x,y\right)$ beschreibt dessen Grauwert. Basierend auf dem zentralen Moment werden die sieben Invarianten berechnet, welche in der Literatur nachgeschlagen werden können.

$$
M_{i,j}=\sum_{x=0}^{M}\sum_{y=0}^{N}{x^iy^jf\left(x,y\right)} \\
(\bar{x},\bar{y})=\left(\frac{M_{10}}{M_{00}},\ \frac{M_{01}}{M_{00}}\right) \\
\mu_{i,j}=\sum_{x=0}^{M}\sum_{y=0}^{N}{\left(x-\bar{x}\right)^i\left(y-\bar{y}\right)^jf\left(x,y\right)}
$$

## Opening
Das Opening in der Bildverarbeitung ist ein morphologischer Prozess, der vor allem dazu dient, kleine Objekte in einem Bild zu entfernen oder Störungen zu beseitigen. Besonders nützlich ist es, um **Rauschen** wie beispielsweise kleine weiße Punkte in einem binären Bild, zu entfernen und die Form von Objekten zu glätten. Es findet Anwendung in verschiedenen Bereichen der Bildverarbeitung, wie z. B. in der Objektverfolgung, der Mustererkennung und der Bildsegmentierung.

Opening besteht aus zwei grundlegenden Operationen: einer **Erosion**, gefolgt von einer **Dilatation**. Bei der Erosion wird das Bild „geschrumpft“, wobei die Ränder der Objekte im Bild verkleinert werden. In der einfachsten Variante ersetzt sie dabei jeden Bildpunkt durch den dunkelsten Pixel innerhalb einer definierten Umgebung. Dies führt dazu, dass dunkle Bereiche des Bilds vergrößert und helle verkleinert werden. Dadurch, dass kleine Objekte weniger Pixel als das Struktur-Element haben, werden diese entfernt. In der Dilatation wird das Bild wieder „aufgebläht“, was bedeutet, dass die Ränder der verbleibenden Objekte wieder erweitert werden. Dabei wird entgegengesetzt zur Erosion vorgegangen. Dies hilft, die Form der angesehenen Objekte zu bewahren, während entfernte kleine Störungen nicht mehr vorhanden sind.

---

## Literaturempfehlung
Tiefergehende Literatur steht Ihnen online über die Bibliothek zur Verfügung:

- M. Werner, Digitale Bildverarbeitung: *Grundkurs mit neuronalen Netzen und MATLAB®-Praktikum*, 1st ed. 2021, Wiesbaden: Springer Fachmedien Wiesbaden, Imprint: Springer Vieweg, 2021, [Online] [https://doi.org/10.1007/978-3-658-22185-0](https://doi.org/10.1007/978-3-658-22185-0)
- A. Nischwitz, M. Fischer, P. Haberäcker, und G. Socher, *Bildverarbeitung: Band II des Standardwerks Computergrafik und Bildverarbeitung*, 4th ed. 2020, Wiesbaden: Springer Fachmedien Wiesbaden, Imprint: Springer Vieweg, 2020, [Online] [https://doi.org/10.1007/978-3-658-28705-4](https://doi.org/10.1007/978-3-658-28705-4)
