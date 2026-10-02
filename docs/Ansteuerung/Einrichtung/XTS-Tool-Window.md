# XTS-Tool-Window

<div align="center">
	<img decoding="async" src="../../../images/XTS/XTS-Tool-Window-Main.png" alt="XTS-Tool-Window" width="700"> <br>
	 XTS-Tool-Window
</div>

Das *XTS Tool Window* ist essentiell für die Konfiguration der XTS. Das Fenster kann über die Menüleiste *TwinCAT->XTS->XTS Tool Window* geöffnet werden. Haben Sie noch kein XTS konfiguriert, sehen Sie in dem neuen Fenster nur einen grau gekachelten Hintergrund. Wichtig ist hier die Symbolleiste am oberen Rand des Fensters. Starten Sie den Konfigurator, können Sie eine Vielzahl an Objekten erstellen und Einstellungen vornehmen. Folgende Anschnitte durchlaufen Sie in der Konfiguration:

1. **Processing Units:** In einer Processing Unit übernimmt die Steuerung des XTS. Hier laufen alle erforderlichen Objekte eines XTS, wie Mover und Parts, zusammen und werden miteinander verknüpft. Pro eigenständigem XTS muss hier eine eigene Processing Unit erstellt werden. Über den *Operation Mode* können Sie wählen, ob das XTS als Hardware oder nur in Simulation angesteuert werden soll. Anschließend müssen Sie hier einstellen, welche Mover auf dem XTS montiert sind und, ob es einen Mover 1 gibt.
2. **Parts:** Hier fügen Sie die Module des XTS hinzu.
3. **Tracks:** Ein Track beschreibt die Fahrmöglichkeit der Mover. Hier können verschiedenste Einstellungen getroffen werden, wie beispielsweise die Fahrtrichtung.
4. **Station:** Hier können Stationen definiert werden, wenn es bestimmte Bereiche auf dem XTS gibt, die die Mover konkret anfahren sollen.
5. **Movers:** Die Kontroller für die Mover werden hier eingefügt. Dabei kann die Anzahl und der Kontrollertyp festgelegt werden.
6. **Control Areas:** Sollen verschiedene Bereiche des XTS unterschiedliche Kontrollerparameter haben, so können hier die Bereiche festgelegt werden. Beispielsweise könnten unterschiedliche Parameter für Geraden und Kurven festgelegt werden.
7. **Real-Time:** Im letzten Schritt muss die Task für die Steuerung des XTS auf einen Kern festgelegt werden.

Das Symbol rechts neben den Einstellungen (Zahnrad) schaltet die *Live-View* ein. Hier können Sie dann die aktuelle Position der Mover sehen, wenn sich TwinCAT im Online-Modus befindet.

## Konfiguration vornehmen
Führen Sie folgende Schritte durch, um ein XTS zu konfigurieren:

1. Öffnen oder erstellen Sie ein TwincAT Projekt, verbinden Sie sich mit dem Demonstrator und scannen Sie die Hardware in Ihr TwinCAT Projekt. 
3. Öffnen Sie das *XTS Tool Window*, indem Sie auf *TwinCAT->XTS->XTS Tool Window* klicken.
4. Klicken Sie im *XTS Tool Window* in der Symbolleiste auf das graue Zahnrad mit grünem Pfeil (XTS Konfigurator Starten). Es öffnet sich nun ein neues Fenster.

	<div align="center">
		<img decoding="async" src="../../../images/XTS/XTS-Tool-Window-StartKonfig.png" alt="XTS-Tool-Window" width="600"> <br>
		XTS-Tool-Window: Start Configurator
	</div>

5. Klicken Sie auf den grauen Pfeil nach rechts in der unteren Leiste.
6. Nun können Sie Einstellungen zu den *Processing Units* vornehmen, wie das auswählen der passenden Movertypen. Wenn Sie fertig sind, klicken Sie auf den grauen Pfeil nach rechts in der unteren Leiste.
8. Klicken Sie unter *Details* auf das grüne Plus und wählen Sie alle Module des XTS aus. Bestätigen Sie anschließend und klicken Sie auf den grauen Pfeil nach rechts in der unteren Leiste.
11. Erstellen Sie einen neuen Track, in dem Sie in der oberen Leiste auf das grüne Plus klicken. Setzen Sie unter *Details* ein Kreuz bei *Is closed*. Klicken Sie auf den grauen Pfeil nach rechts in der unteren Leiste, wenn Sie fertig sind.
14. Klicken Sie in der oberen Leiste das grüne Plus aus, um eine Station hinzuzufügen und auf den grauen Pfeil nach rechts in der unteren Leiste, wenn Sie fertig sind.
16. Geben Sie in der oberen Leiste neben dem grünen Plus die Anzahl der Mover auf dem XTS ein. Klicken Sie auf den deneben befindlichen grünen Pfeil, um die Mover hinzuzufügen. Wählen Sie den *SoftDrive* als Kontroller aus und bestätigen Sie. Wenn Sie alle Mover eingefügt haben, klicken Sie auf den grauen Pfeil nach rechts in der unteren Leiste.
20. Klicken Sie nochmal auf den grauen Pfeil nach rechts in der unteren Leiste.
21. Scrollen Sie in dem Fenster nach ganz unten. Hier wird angezeigt auf welchem Kern die Processing Unit läuft. Wichtig ist hierbei, dass als Zykluszeit 250 Mykrosekunden ausgewählt sind. Möchten Sie den Kern ändern, so können Sie in dem Reiter oben auf *System* klicken. Verschieben Sie hier die XTS Task auf den entsprechenden Kern. Klicken Sie auf den grauen Pfeil nach rechts in der unteren Leiste, wenn Sie mit den Einstellungen fertig sind.
24. Klicken Sie auf den grauen Pfeil nach rechts in der unteren Leiste, um die Konfiguration zu beenden.


