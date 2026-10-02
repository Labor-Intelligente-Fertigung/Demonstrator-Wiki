# TwinCAT
Auf dieser Seite finden Sie eine Erklärung zur Software TwinCAT der Firma Beckhoff Automation.

> :material-web: Alle Informationen finden Sie auch auf der [Webseite von Beckhoff](https://www.beckhoff.com/de-de/produkte/automation/twincat/).

## Einführung
TwinCAT 3 (The Windows Control and Automation Technology) ist eine softwarebasierte Automatisierungsplattform des Unternehmens Beckhoff. Sie dient der Entwicklung, Programmierung und Visualisierung von Steuerungs- und Automatisierungssystemen. Einige Hauptmerkmale von TwinCAT sind:

- **Programmierung:** TwinCAT unterstützt verschiedene Programmiersprachen, die den Normen der IEC 61131-3 entsprechen, darunter auch Strukturierter Text (ST), Ladder Diagram (LD) und Function Block Diagram (FBD).
- **Integration:** Die Software erlaubt die nahtlose Integration in Microsoft Windows-Betriebssysteme, was bedeutet, dass sie auf Standard-PC-Hardware ausgeführt werden kann.
- **Echtzeitfähigkeit:** Mit TwinCAT können Sie Echtzeitanwendungen erstellen, die eine hohe Performance und kurze Reaktionszeiten erfordern.
- **Visualisierung:** Die Plattform beinhaltet Werkzeuge zur visualisierten Darstellung von Prozessen und zur Benutzerinteraktion.
- **Kommunikation:** TwinCAT unterstützt verschiedene Kommunikationsprotokolle, einschließlich EtherCAT, OPC UA und vielen weiteren, die eine vielseitige Integration in bestehende Systeme ermöglichen.

TwinCAT 3 teilt sich auf in das Engineering und die Runtime. Diese werden im Folgenden genauer erklärt.

### Engineering
TwinCAT XAE (eXtended Automation Engineering) ermöglicht das Konfigurieren, Programmieren und Debuggen von Applikationen. Für die Programmierung stehen außer den IEC 61131-3-Programmiersprachen auch C/C++, MATLAB und Simulink zur Verfügung. Zudem bietet das Programm integrierte Debugging-Möglichkeiten für den Programmcode und Diagnosefunktionalitäten für die Steuerungshardware. Es gibt viele Zusatzpakete, mit denen die Basis erweitert werden kann, wie beispielsweise ein Softwareoszilloskop oder Bildverarbeitungstools.

Das Engineering ist meist nur auf einem Entwicklungs-PC und nicht auf dem IPC einer Maschine oder Anlage installiert, da diese im Regelfall nicht umprogrammiert werden muss. Die Entwicklungsumgebung des Engineerings kann eine *Visual Studio Integration* sein. Hierfür muss die Software Visual Studio von Microsoft vor der Installation des Engineerings installiert sein. Dann kann das Engineering direkt in Visual Studio geöffnet werden. Eine andere Möglichkeit ist die Installation einer *Isolated Engineering Shell*. Hierbei ist eine minimale Version von Visual Studio enthalten, jedoch ist die Software vor der Installation nicht notwendig und wird direkt mit installiert. Anschließend kann die Shell direkt geöffnet werden.

Das Basis-Engineering ist modular aufgebaut und besteht aus mehreren Komponenten. Der Unterschied zwischen den Benutzeroberflächen der beiden Installationsarten ist minimal, daher wird hier nicht darauf eingegangen. Die Entwicklungsumgebung ist in der folgenden Abbildung zu sehen. Die verschiedenen Fenster können frei an anderen Stellen angeordnet werden.

<div align="center">
	<img decoding="async" align="center" src="../../images/Entwicklungsumgebung.png" alt="Entwicklungsumgebung" width="700" height="450"> <br>
	 Aufbau TwinCAT Engineering
</div>
  
In der *Menüleiste* können Projekte geöffnet oder neu erstellt werden. Es können zudem Einstellungen zum Projekt oder der Entwicklungsumgebung vorgenommen werden.

Die *Symbolleiste* bietet zahlreiche Buttons für die Ausführung von Operationen, wie das Speichern oder Erstellen eines Projekts.

Im *Editorfenster* können Dateien geschrieben und verändert werden. Das Fenster teilt sich in zwei Bereiche auf. Der *Deklarationsteil* befindet sich oben. Hier werden alle benötigten Variablen deklariert. Der untere Abschnitt wird *Methodenteil* genannt. Hier wird die Funktionsweise implementiert. Je nachdem, welche Datei Sie gerade bearbeiten, kann es sein, dass einer der Teile nicht bearbeitet werden kann.

Im *Projektmappen-Explorer* sind die verschiedenen Komponenten der Software zu sehen. Folgende Komponenten können in einem Projekt vorhanden sein:

- **System:** Dieses Tool dient der Konfiguration des Systems. Es enthält unter anderem die Echtzeiteinstellungen, die Konfiguration der Tasks, die ADS-Routen sowie die Typdefinitionen und benötigten Lizenzen des Projektes.
- **Motion:** Dieses Tool enthält die Konfiguration für die Verknüpfung mit Antriebstechnologien, wie Motoren. Es handelt sich um die Komponente für die numerische Steuerung (NC, Numerical Control), die speziell für Anwendungen in der Bewegungstechnik und Maschinensteuerung entwickelt wurde.
- **PLC:** (eng. Programmable Logic Controller, deut. SPS – Speicherprogrammierbare Steuerung) Dieser Teil der Software, der für die Programmierung und Ausführung von Steuerungsanwendungen verantwortlich ist. PLCs sind so konzipiert, dass sie Eingabesignale von verschiedenen Sensoren und Geräten empfangen und darauf basierend Logikoperationen ausführen, um bestimmte Ausgabesignale an Aktoren wie Motoren, Pumpen oder andere Steuergeräte zu senden.
- **Safety:** Dies realisiert eine sicherheitsgerichtete Laufzeitumgebung. Hier können daher spezielle Safety Klemmen und deren Funktionsweise programmiert werden.
- **C++:** Diese Komponente realisiert auf einem Industrie-PC eine Echtzeitausführung von C++-Code. Das heißt, dass zur Programmierung die Programmiersprache C++ unterstützt wird, die durch das TwinCAT eine Anbindung an die Echtzeit erfährt.
- **E/A:** Hier werden zyklische Daten von verschiedenen Feldbussen in Prozessabbildern gesammelt. Die Geräte mit dessen Komponenten, wie Klemmen oder Antriebe, sind hier aufgelistet und können verwaltet werden. Über einen sogenannten Free Run werden die Geräte getriggert, sodass deren Werte übertragen und hier eingesehen werden können.

Das *Meldungsfenster* gibt Meldungen, Warnungen und Fehlermeldungen aus. Haben Sie eine Fehler-meldung, können Sie in den meisten Fällen das Projekt nicht ausführen und müssen zunächst den Fehler beheben.

Das *Eigenschaftenfenster* und den *Werkzeugkasten* werden Sie zu Beginn nicht benötigen. Hiermit können beispielsweise Visualisierungen erstellt werden.

Die *Informations- und Statusleiste* zeigt den Status der TwinCAT-3 Runtime. Wenn gerade ein Editorfenster aktiv ist, werden die aktuelle Position des Cursors und der eingestellte Editiermodus angezeigt. Im Onlinebetrieb sehen Sie den augenblicklichen Status des Programms. 

### Runtime
TwinCAT XAR (eXtended Automation Runtime) ist eine echtzeitfähige Laufzeit, in welcher der Programmcode ausgeführt werden kann, um eine Maschine zu steuern. Realtime bezieht sich auf die Fähigkeit eines Systems, Aufgaben und Prozesse innerhalb eines festgelegten Zeitrahmens mit deterministischen Reaktionen auszuführen. Die Runtime ist auf dem System installiert, welches eine Maschine oder Anlage steuert. Meist ist es nicht notwendig auf einem solchen System auch das Engineering zu installieren, da die Programmierung häufig auf externen Geräten durchgeführt wird. Folgende Betriebs-Modi können von einem System angenommen werden:

- **Konfig-Modus:** In diesem Modus findet die Programmierung statt. Es können Geräte eingelesen werden und Werte über den Free Run eingelesen werden. Sie können diesen Modus explizit auswählen.
- **Run-Modus:** Diesen Modus können Sie starten, wenn Sie die SPS starten und das Projekt ausgeführen wollen. Das System wird in diesem Zustand auch als *Online* beschrieben.
- **Stop-Modus:** Wenn der Modus gewechselt wird, wird dieser Zustand kurzzeitig angenommen. Bei Fehlern kann es vorkommen, dass die Software diesen Zustand annimmt.
- **Exception-Modus:** Bei Fehlern kann es vorkommen, dass die Software diesen Zustand annimmt.

## Tutorials

<div class="grid cards" markdown>

-   :simple-youtube:{ .youtube .lg .middle } **SPS-Programmierung für Einsteiger**

	---
	Videotutorials zu den Basics in TwinCAT

    [:fontawesome-solid-external-link: Zur Playlist](https://www.youtube.com/playlist?list=PL2LjUivoqcmUNF4wfaZdWQEZm9ptpIFuw)

-   :b:{ .youtube .lg .middle } **Beckhoff Infosys**

	---
	Wiki von Beckhoff zu TwinCAT

    [:fontawesome-solid-external-link: Zur Seite](https://infosys.beckhoff.com/content/1033/tcinfosys3/index.html?id=2683277694279723185)

</div>