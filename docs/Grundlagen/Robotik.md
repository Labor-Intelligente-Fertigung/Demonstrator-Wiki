# Robotik
Im Folgenden werden verschiedene Grundlagen und Fachbegriffe der Robotik erklärt, die für das Verständnis der Programmierung hilfreich sind.

## Roboteraufbau
Ein Roboter (in Normen auch als *Manipulator* bezeichnet) besteht aus mehreren Segmenten (in Abbildung A-F), die durch **Gelenke/Achsen** (eng. *joints*) (in der Abbildung 1-6) verbunden sind. Die Gelenke werden dabei ausgehend von der Basis (A) durchnummeriert. Am Ende der höchsten Achse sitzt der **Flansch** an welchem der **Endeffektor**, kurz **EE**, befestigt ist. In der Regel ist das ein Greifer oder ein anderes Werkzeug, mit welchem eine *Manipulation* (das Verändern des Zustands eines Objekts) vorgenommen werden kann. Im Endeffektor kann auch ein **Tool Center Point**, kurz **TCP**, definiert werden. Dieser beschreibt einen Arbeits- oder Wirkpunkt, der sich häufig in der Mitte des EEs befindet.

<div align="center">
	<img decoding="async" src="../../images/robot/RoboterAufbau.png" alt="Roboteraufbau" width="600" height="400"> <br>
	Roboteraufbau
</div>

Der in der Abbildung gezeigte Aufbau zeigt einen **seriellen Roboter**. Hierbei sind die Achsen aufsteigend nacheinander angeordnet. Im Gegensatz dazu gibt es auch **parallele Roboteraufbauten**. Hierbei können, wie der Name schon sagt, die Segmente parallel zueinander verlaufen. Je nach Aufgabe wird der passende Aufbau ausgewählt.

Es gibt Roboter mit unterschiedlich vielen Achsen und Segmenten. Vergleichbar mit dem menschlichen Arm sind serielle Roboter mit 6-Achsen, da diese die gleiche Anzahl an unabhängigen Bewegungsmöglichkeiten, sogenannten **Freiheitsgraden**, hat. Durch seine sechs Achsen kann der Roboter sich in drei Richtungen oben/unten, vor/zurück und rechts/links bewegen, sowie auch um alle diese drei Richtungen rotieren. Dadurch hat ein solcher Roboter auch einen Freiheitsgrad von 6.

## Bewegung
Wichtig bei Robotern ist es, dass bei Spannungsverlust die Bremsen der Gelenke aktiv sind, damit der Roboter nicht in sich zusammen fällt. Aus diesem Grund werden meist elektromagnetische Bremsen verwendet, welche nur bei Spannungszufuhr gelöst sein können.

Für das Verfahren eines Roboters sind verschiedene Angaben von Nöten: Die **Position** bezeichnet in der Robotik einen Punkt im dreidimensionalen Raum, welcher typischerweise durch Koordinaten wie *x, y* und *z* angegeben wird. Der Wechsel von einer zur anderen Position wird als **Translation** bezeichnet. Die **Pose** ergänzt dies um die Orientierung des Endeffektors, also **Rotation**, die meist als *roll, pitch* und *yaw* für die jeweilige Achse bezeichnet werden. Die Rotation kann dabei in Grad, Rad oder als **Quaternionen** angegeben werden.

Die **Trajektorie** beschreibt dabei den zeitlichen Verlauf der Pose des Endeffektors im Raum, also eine zeitabhängige Bahn eines Roboterarm. Sie beschreibt die Reihenfolge der Positionen, Orientierungen und Bewegungsparameter, wie Geschwindigkeit und Beschleunigung von Start- zu Zielpunkt. In der Industrierobotik dient sie dazu, geplante Bewegungen präzise zu steuern und wiederholbar auszuführen, sodass der Endeffektor einem definierten **Pfad** mit festgelegtem Tempo folgt.

Wichtig ist es, dass **Singularitäten** bei Trajektorien vermieden werden. Eine Singularität in der Robotik ist eine Pose des Roboters, in der sich der Endeffektor in bestimmte Bewegungsrichtungen kaum oder gar nicht mehr bewegen lässt, obwohl sich die Achsen bewegen. Ein typisches Beispiel ist, wenn der Roboterarm nahezu in einer geraden Linie steht. Ist der Roboter so aufgebaut, wie in der ersten Abbildung, dann könnten sich die Gelenke 1 und 4 in die entgegengesetzten Richtungen drehen, aber der Endeffektor würde an derselben Pose bleiben. Dadurch kann es dazu kommen, dass folgende Posen nicht richtig angefahren werden können.

In der Robotik wird zwischen zwei Genauigkeiten in der Bewegung unterschieden. Die **Wiederholgenauigkeit** beschreibt die Streuung der tatsächlich erreichten Pose des Endeffektors, wenn unter gleichen Bedingungen derselbe Bewegungsbefehl mehrmals hintereinander ausgeführt wird. Es wird gemessen, wie nah die Endposition bei den einzelnen Ausführungen zueinander liegt. Ursachen sind mechanische Spielräume, Toleranzen in Getrieben, Rauschen in der Regelung, Vibrationen oder Temperaturänderungen. Die **Positionsgenauigkeit** betrachtet die absolute Abweichung der tatsächlichen Pose nach einem Bewegungsbefehl vom gewünschten Zielwert. Sie berücksichtigt Kalibrierfehler, Encoder-Fehler, mechanische Nachgiebigkeit, Thermik, Trägheiten und mögliche Fehler in der Trajektorienplanung.

<div align="center">
	<img decoding="async" src="../../images/robot/Genauigkeiten.png" alt="Genauigkeiten" width="500" height="260"> <br>
	Unterschied Genauigkeiten (A: Wiederholgenauigkeit, B: Positionsgenauigkeit)
</div>

## Home-Position
Die **Home-Position** in der Industrierobotik ist eine fest definierte Referenzpose des Roboterarms, die als Start- und Rückkehrpunkt für alle Bewegungen dient. Sie dient als sicherer, reproduzierbarer Ausgangspunkt nach dem Einschalten, nach Störungen, beim Neustart oder nach Beendigung einer Trajektorie. In dieser Position findet der Endeffektor eine sichere Stellung, die von Hindernissen oder Werkstücken freigehalten wird. Welche Pose genau als Home-Pose gewählt wird, hängt vom System, der Anwendung und dem Hersteller ab. In manchen Systemen entspricht sie der Nullposition des Koordinatensystems, in anderen liegt sie separat davon.

## Koordinatensysteme
Für die Ansteuerung von Robotern werden verschiedene Koordinatensysteme definiert, die im Folgenden erklärt werden. Anschließend wird die Umrechnung von einem zum anderen Koordinatensystem mit Hilfe von Matlab erklärt.

### Einführung
Das **Maschinenkoordinatensystem** (eng. *machine-coordinate-system*) wird als **MCS** abgekürzt. 
Es befindet sich zumeist in der Basis des Roboters. In dem Koordinatensystem wird die Endeffektor-Pose (EEP) als X-, Y- und Z-Koordinate, sowie dessen Rotation in roll-, pitch- und yaw-Winkel angegeben.

Zusätzlich ist für jede Achse ein weiteres Koordinatensystem definiert, das **Achskoordinatensystem** (eng. *axis-coordinate-system*). Es wird mit **ACS** abgekürzt. So kann für jede Achse ein Winkel festgelegt werden, mit welchem die Achsen gesteuert werden können.

<div align="center">
	<img decoding="async" src="../../images/robot/Koordinatensysteme.png" alt="Koordinatensysteme" width="700" height="300"> <br>
	Unterschied Achs- und Maschinenkoordinatensystem
</div>

Zumeist wird der Punkt angegeben, an welchen der Endeffektor des Roboters verfahren soll. Diese Angabe wird im MCS gemacht. Um einen Roboter verfahren zu können, müssen jedoch jeder Achse die benötigten Winkel als Ziel angegeben werden, also im ACS.  Die nötigen Achspositionen bei einer Zielpose im MCS sind dann aber unbekannt und es muss eine Umrechnung erfolgen. Die Umrechnung wird **Inverse Transformation** genannt. Es kann jedoch auch sein, dass die Achswinkel im ACS bekannt sind und die EEP im MCS gesucht ist. Hierfür wird die **Vorwärtstransformation** verwendet. Für die Transformationen wird der *kinematische Aufbau* des Roboters benötigt, daher werden sie auch **Kinematik** genannt. Darunter versteht man die Beschreibung der geometrischen Struktur des Roboters ausgehend von der Basis, über die Länge der Glieder und der Anzahl der Gelenke bis zum Endeffektor.

### Umrechnungen in Matlab
Für die Berechnung der Transformationen kann Matlab verwendet werden. Für die Berechnung wird eine Beschreibung der Kinematik des Roboters benötigt. Diese ist als `urdf`-File gespeichert. Zur Visualisierung werden die einzelnen Gelenke des Roboters als `stl`-Files benötigt.

> :material-folder-multiple: Die genannten Files können im ILIAS-Kurs heruntergeladen werden.

=== "Vorwärtstransformation"
	Für die Vorwärtstransformation bietet Matlab die Funktion [`getTransform`](https://de.mathworks.com/help/robotics/ref/rigidbodytree.gettransform.html). Die Funktion kann wie folgt eingesetzt werden:

	```matlab
	% --- preparation ---
	clear(); close all;
	% load robot model: change path to the path of your urdf
	tx40 = importrobot("path\tx40.urdf", DataFormat="row");

	% your robot axis angles (ACS) in °
	angles = [68.2515 27.2293 94.6346 -0.1171 57.5845 -93.2959];
	angles = (angles*pi)/180; % change from ° to rad

	% --- calculate getTransformation ---
	T_end = getTransform(tx40, angles, 'link_6'); % calculate 4x4 matrix
	T_Pos = tform2trvec(T_end) * 1000 % Positions in mm
	T_Rot = (tform2eul(T_end, 'ZYX')*180)/pi % Orientations in °

	% show solutions in figure
	figure('Name', 'Forward Kinematics');
	show(tx40, angles, PreservePlot=false); % show robot with axis angles
	hold on;
	title('Forward Kinematics');
	axis([-0.8 0.8 -0.8 0.8 -0.5 0.8]);
	```

	Zunächst muss das Robotermodell geladen werden. Hier sollten Sie *path* durch Ihren Pfad des `urdf`-Files ersetzen. Anschließend werden in `angles` als 6x1-Vektor die Gelenkwinkel des Roboters in aufsteigender Reihenfolge in Grad angegeben. Diese können Sie durch eigene Werte ersetzen. In der nächsten Zeile werden die Winkel in Radiant umgerechnet, da die Funktion `getTransform` in dieser Einheit rechnet. Der Funktion müssen dazu das Robotermodell, die Winkel und der Name der letzten Achse übergeben werden. Die Transformation wird als 4x4 Matrix angegeben, aus welcher anschließend die Translation und die Rotation ausgelesen und von Meter in Millimeter, bzw. von Radiant in Grad umgerechnet wird.

=== "Inverse Transformation"
	Für die Inverse Transformation gibt es in Matlab zwei Funktionen:

	1. [`inverseKinematics`](https://de.mathworks.com/help/robotics/ref/inversekinematics-system-object.html)
	2. [`analyticalInverseKinematics`](https://de.mathworks.com/help/robotics/ref/analyticalinversekinematics.html)

	Bei 1 wird eine und bei werden 2 alle möglichen Lösungen berechnet. Die Funktionen können wie folgt angewendet werden:

	```matlab
	% --- preparation ---
	clear(); close all;
	% load robot model: change path to the path of your urdf
	tx40 = importrobot("path\tx40.urdf", DataFormat="row");

	% your EEP (MCS) in mm
	eePose = [76.780 286.621 16.291 -179.455 -0.131 -18.391];
	eePos = [eePose(1) eePose(2) eePose(3)] * 0.001; % change from mm to m
	eeRot = ([eePose(6) eePose(5) eePose(4)] * pi) / 180; % change from grad to rad

	% --- calculate inverseKinematics ---
	ik = inverseKinematics(RigidBodyTree=tx40, SolverAlgorithm="LevenbergMarquardt");
	ee = tx40.BodyNames{end};
	eePoseM = se3(eeRot,"eul","ZYX",eePos); % get 3d homogenious transformation matrix
	weights = [1 1 1 0.8 0.8 0.8];
	initGuessConfig = [pi/2 0 0 0 0 0];
	[config,solninfo] = ik(ee,tform(eePoseM),weights,initGuessConfig);
	config

	% show solution in figure
	figure('Name', 'Solution with InverseKinematics');
	show(tx40, config, PreservePlot=false);
	hold on;
	title('Solution with InverseKinematics');
	axis([-0.8 0.8 -0.8 0.8 -0.5 0.8]);
	plotTransforms(eePoseM, 'FrameSize', 0.2);

	% --- calculate analyticalInverseKinematics ---
	kinGroup = struct("BaseName","base_link","EndEffectorBodyName","link_6");
	aik = analyticalInverseKinematics(tx40, 'KinematicGroup', kinGroup);
	generateIKFunction(aik, 'robotIK');
	eeRotQuat= eul2tform(eeRot, 'ZYX'); % transform to quaternion
	eePose = trvec2tform(eePos) * eeRotQuat; % get transformation matrix (4x4)
	ikConfig = robotIK(eePose);

	% show solutions in figure
	figure('Name', 'Configurations');
	numSolutions = size(ikConfig,1);
	for i = 1:size(ikConfig,1)
		subplot(2, ceil(numSolutions/2), i);
		show(tx40,ikConfig(i,:));
		axis([-0.8 0.8 -0.8 0.8 -0.5 0.8]);
		title(sprintf('Solution %d', i));
	end
	```

	Wie auch schon bei der Vorwärtstransformation muss das Robotermodell geladen werden. Auch hier sollten Sie *path* durch Ihren Pfad des `urdf`-Files ersetzen. Es wird die EEP benötigt. Diese ist als 6x1-Vektor angegeben, bei der die ersten drei Stellen die X-, Y- und Z-Koordinaten in Millimeter und die letzten drei Stellen die Rotationen roll(x), pitch(y) und yaw(z) in Grad sind. Da auch die Funktionen der Inversen Transformation mit Meter und Radiant rechnen, muss die EEP umgerechnet werden. 

	Die Funktion `inverseKinematik` erstellt ein Objekt zur Berechnung der Kinematik. Dazu werden das Robotermodell und ein gewünschter Solver als Parameter benötigt. Zur Berechnung der Inversen Kinematik mit diesem Objekt muss der Name der letzten Achse, die EEP als 3-dimensionale homogene Transformationsmatrix, Gewichte und eine initiale Konfiguration des Roboters angegeben werden.

	Die Funktion `analyticalInverseKinematics` erstellt ein Objekt das zur Berechnung der Inversen Kinematik benötigt wird. Dazu wird das Robotermodell und die Namen der ersten und letzten Achse als struct benötigt. Mit der Funktion `generateIKFunction` wird ein Skript für die eigentliche Berechnung der Inversen Kinematik erstellt, welches das eben erstellte Objekt verwendet. Das Skript benötigt die EEP als 4x4 Transformationsmatrix, bei welcher die Rotation in Quaternionen angegeben ist. Es berechnet als nx6 Matrix alle möglichen Konfigurationen n, die der Roboter haben kann, um die gewünschte EEP zu erreichen.

---

## Literaturempfehlung
Die folgende Literatur ist online über die Hochschulbibliothek oder per Open Access verfügbar.

- Uhlmann, E., Krüger, J. (2020): *Industrieroboter*, in: Bender, B., Göhlich, D. (eds) *Dubbel Taschenbuch für den Maschinenbau 2: Anwendungen*, Springer Vieweg, Berlin, Heidelberg. [https://doi.org/10.1007/978-3-662-59713-2_51](https://doi.org/10.1007/978-3-662-59713-2_51)
- Westcott, J. (2023): *Industrial Automation and Robotics*, Berlin, Boston: Mercury Learning and Information. [https://doi.org/10.1515/9781683929604](https://doi.org/10.1515/9781683929604)