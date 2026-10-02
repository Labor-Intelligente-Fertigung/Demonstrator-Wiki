# Roboter SPS-Ansteuerung

>[!NOTE] Hinweis
> Bevor Sie mit der SPS Programmierung beginnen, sollten Sie die [Einrichtung der Roboter](../Einrichtung/Roboter-Einrichtung.md) bereits durchgeführt haben.

>Grundlagen bezüglich Robotik und den verschiedenen Koordinatensystemen finden Sie [hier](../../Grundlagen/Robotik.md). Informationen zu den Robotern erhalten Sie [hier](../../Komponenten/TX40-Staubli.md).

>Die Dokumentationen der TwinCAT-Bibliotheken können Sie online über Beckhoff oder dem ILIAS-Kurs herunterladen.

Die Roboterachsen werden unabhängig voneinander wie einzelne Motoren mit der Bibliothek *Tc2_MC2* angesteuert. Es werden Winkel in Grad angegeben zu denen die Motoren sich bewegen sollen.

## Ansteuerung der ACS-Achsen
Zur Verknüpfung mit den ACS-Achsen im Motion-Bereich muss folgendes deklariert werden:
```
stAxisAcs    	: ARRAY[1..6] OF AXIS_REF;	    // ACS axis
```

Für die Steuerung der einzelnen Roboterachsen wurde der Funktionsbaustein [FB_Axis](./Funktionsbausteine/FB_Axis.md) geschrieben. Er dient der einzelnen Achsansteuerungen und kann im ILIAS-Kurs heruntergeladen werden. Für jede Achse muss ein eigener FB instanziiert werden. Zusätzlich werden folgende Variablen benötigt:

```
fbAxisAcs		: ARRAY[1..6] OF FB_Axis;		// control of ACS axis
stMoveParam     : ST_MoveParameter;             // parameters for moving axis
nState          : INT;                          // control state
nI              : INT := 1;                     // axis counter
fPosition       : REAL;                         // position for axis
```

Die FB_Axis müssen im Programm zyklisch ausgeführt werden, bspw.:
```
FOR n:=1 TO 6 BY 1 DO
	fbAxisAcs[n](Axis:= stAxisAcs[n]);
END_FOR
```

Damit eine Achse verfahren kann und man nicht manuell alle Achsen freischalten muss, kann das über die SPS gemacht werden. Einfacherweise kann man das in einer Ablaufsteuerung (CASE-OF) machen. Hier muss die Methode `M_PowerOn` des FBs aufgerufen werden. Diese übergibt den Status `E_StateDone`, wenn die Achse freigeschaltet wurde. Anschließend kann ein Fahrbefehl in `M_MoveAbsolute` geschrieben werden. Die Eigenschaft `P_MoveAbsolute` gibt den Status der Bewegung an. Eine Ablaufsteuerung könnte so aussehen:

```
CASE nState OF
	0: // enable axis
		IF fbAxisAcs[nI].M_PowerOn(100) = E_MethodState.E_StateDone THEN
			nI := nI + 1;
		END_IF
		IF nI = 7 THEN
			nState := 10;
		END_IF
	10: // write move command
		stMoveParam.fVelocity := 5.0;
		stMoveParam.fAccel    := 1E300;
		stMoveParam.fDecel    := 1E300;
		stMoveParam.fPosition := fPosition;
		fbAxisAcs[1].M_MoveAbsolute(stMoveParam);
		nState := nState + 1;
	11: // wait until position is reached
		IF fbAxisAcs[1].P_MoveAbsolute = Done THEN
			nState := 20;
		END_IF
	20: // wait
END_CASE
```

Durch ein Schreiben von `fPosition` auf einen gewünschten Wert und `nState` auf 10, kann immer wieder eine andere Position angefahren werden.

## Ansteuerung der MCS-Achsen
Die Ansteuerung der Roboter per MCS erfordert eine Inverse Transformation, um den Achsen die genauen Befehle übergeben zu können. Dazu gibt es von Beckhoff die Bibliothek *TF5113 Kinematic Transformation L4 (Tc2_NcKinematicTransformation)*.

### Variablendeklaration
Zur Verknüpfung mit den MCS-Achsen und der kinematischen Gruppe im Motion-Bereich muss folgendes deklariert werden:

```
stAxisMcs   	: ARRAY[1..P_Robots.MAX_AXIS] OF AXIS_REF;  // MCS axis
stKinRefIn       AT %I* : NCTOPLC_NCICHANNEL_REF;		    // kinematic group
```

Auch hier muss wieder der `FB_Axis` für die MCS-Achsen deklariert werden.

```
fbAxisMcs		: ARRAY[1..6] OF FB_Axis; // control of MCS axis
```

### Berechnung MCS aus ACS
Beim Starten der SPS stehen die Werte der MCS-Achsen auf 0, da keine direkte Hardware zum Auslesen vorhanden ist. Daher müssen die Positionen über eine Vorwärtstransformation aus den Positionen der ACS-Achsen berechnet werden. Dazu sind folgende Deklarationen nötig:

```
nState          : INT;                  // control state
nI              : INT := 1;             // axis counter
fbGetActualPos	: MC_ReadActualPosition;// read axis position
stAxesPosIn 	: ARRAY[1..8] OF LREAL;	// axis values input of transformation
stAxesPosOut	: ARRAY[1..8] OF LREAL;	// axis values output of transformation
fbKinCalcTrafo	: FB_KinCalcTrafo;		// calculate transformation
uMetaInfoIn		: U_KinMetaInfo;		// dummy variable
uMetaInfoOut	: U_KinMetaInfo;		// dummy variable
fbSetActualPos	: MC_SetPosition;		// set axis values
```

Zunächst müssen mit Hilfe des `MC_ReadActualPosition` die aktuellen Werte der ACS-Achsen ausgelesen werden. Anschließend kann mit `FB_KinCalcTrafo` die Transformation berechnet werden. Mit dem `MC_SetPosition` können dann die MCS-Achsen mit der aktuellen Position beschrieben werden. Die CASE-Abfolge aus dem vorherigen Kapitel kann durch folgendes ersetzt werden:

```
CASE nState OF
	0: // read values ACS axis
		fbGetActualPos(
			Axis	:= stAxisAcs[nI], 
			Enable	:= TRUE, 
			Position=> stAxesPosIn[nI]);
		IF stAxesPosIn[nI] <> 0.0 THEN
			fbGetActualPos(Axis := stAxisAcs[nI], Enable := FALSE);
			nI := nI + 1;
		END_IF
		IF nI = 7 THEN
			nState := nState + 1;
		END_IF
	1: // calculate MCS values
		fbKinCalcTrafo(
			bExecute:= TRUE, 
			bForward:= TRUE, 
			oidTrafo:= nGroupID, 
			stAxesPosIn:= stAxesPosIn, 
			stAxesPosOut:= stAxesPosOut, 
			uMetaInfoIn:= uMetaInfoIn, 
			uMetaInfoOut:= uMetaInfoOut);
		IF fbKinCalcTrafo.bDone THEN
			nI := 1;
			nState := nState + 1;
		END_IF
	2: // set MCS axis values
		fbSetActualPos(
			Axis	:= stAxisMcs[nI], 
			Execute	:= TRUE, 
			Position:= stAxesPosOut[nI], 
			Mode	:= FALSE);
		IF fbSetActualPos.Done THEN
			fbSetActualPos(
				Axis	:= stAxisMcs[nI], 
				Execute	:= FALSE);
			nI := nI + 1;
		END_IF
		IF nI = 7 THEN
			nState := 10;
		END_IF
END_CASE
```

### Fahrbefehle berechnen
Möchte man nun den Roboter per MCS-Achsen ansteuern, benötigt man einen FB einer kinematischen Gruppe, welche die Berechnung der inversen Kinematik übernimmt. Dazu werden folgende Variablen benötigt:

```
stKinAxesList 	: ST_KinAxes;			  // axis IDs for group
fbKinConfigGroup: FB_KinConfigGroup;	  // group configuration
fbKinResetGroup	: FB_KinResetGroup;		  // group reset
eKinStatus		: E_KinStatus;			  // group state
```

In der Struktur `stKinAxesList` müssen die IDs der jeweiligen Achsen reingeschrieben werden. Mit dem Funktionsbaustein `FB_KinConfigGroup` werden die Roboterachsen entsprechend der kinematischen Transformation konfiguriert. Dazu muss die Achsenliste und die kinematische Gruppe übergeben werden. Zyklisch muss also folgendes ausgeführt werden:

```
FOR n:=1 TO 6 BY 1 DO
	fbAxisAcs[n](Axis:= stAxisAcs[n]);
	stKinAxesList.nAxisIdsAcs[n] := stAxisAcs[n].NcToPlc.AxisId;
	fbAxisMcs[n](Axis:= stAxisMcs[n]);
	stKinAxesList.nAxisIdsMcs[n] := stAxisMcs[n].NcToPlc.AxisId;
END_FOR

eKinStatus := F_KinGetChnOperationState(stKinRefIn:=stKinRefIn);
fbKinConfigGroup(
	bExecute		:= , 
	bCartesianMode	:= , 
	stAxesList		:= stKinAxesList, 
	stKinRefIn		:= stKinRefIn);
```

Nachdem alle Achsen freigeschaltet sind, muss die kinematische Gruppe aktiviert werden. Dazu muss `bEnable` und `bCartesianMode` auf TRUE gesetzt werden. Die Ablaufsteuerung kann um folgende Schritte ergänzt werden:

```
	10: // enable ACS axis
		IF stAxisAcs[nI].Status.Disabled = TRUE THEN
			IF fbAxisAcs[nI].M_PowerOn(100) = E_MethodState.E_StateDone THEN
				nI := nI + 1;
			END_IF
		ELSE
			nI := nI + 1;
		END_IF
		IF nI = 7 THEN
			nI := 1;
			nState := nState + 1;
		END_IF
	11: // enable MCS axis
		IF stAxisMcs[nI].Status.Disabled = TRUE THEN
			IF fbAxisMcs[nI].M_PowerOn(100) = E_MethodState.E_StateDone THEN
				nI := nI + 1;
			END_IF
		ELSE
			nI := nI + 1;
		END_IF
		IF nI = 7 THEN
			nI := 1;
			nState := 20;
		END_IF
	20:	// enable group
		IF eKinStatus = KinStatus_Empty THEN
			fbKinConfigGroup.bExecute 		:= TRUE;
			fbKinConfigGroup.bCartesianMode := TRUE;
		END_IF
		IF NOT (eKinStatus = KinStatus_Empty) THEN
			nState := 30;
		END_IF
```

Abschließend kann in einem Schritt 30 ein Fahrbefehl gegeben werden, wie für die ACS-Achsen bereits erklärt wurde.
