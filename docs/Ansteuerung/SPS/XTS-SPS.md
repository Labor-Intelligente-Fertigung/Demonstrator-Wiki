# XTS SPS-Ansteuerung

>[!NOTE] Hinweis
> Bevor Sie mit der SPS Programmierung beginnen, sollten Sie die [Einrichtung des XTS](../Einrichtung/XTS-Tool-Window.md) bereits durchgeführt haben.

>Die Dokumentationen der TwinCAT-Bibliotheken können Sie online über Beckhoff oder dem ILIAS-Kurs herunterladen.

Für die Programmierung des XTS wird das Paket *TF5850 TwinCAT 3 XTS* benötigt. Die enthaltene Bibliothek *Tc3_XTS_Utility* wird zum Lesen oder Setzen von Parametern des XTS verwendet. Die Mover des XTS werden wie einfache Achsen angesteuert. Hierfür wird die Bibliothek *Tc2_MC2* benötigt. Des Weitern wird hier das Paket *TF5410 TwinCAT 3 Motion Collision Avoidance* verwendet. Die enthaltene Bibliothek *Tc3_McCollisionAvoidance* dient der Kollisionsvermeidung einzelner Mover einer Gruppe, indem ein Mindestabstand zu den Achsen eingehalten wird. Die Gruppe wird über die Bibliothek *Tc3_CoordinatedMotion* verwaltet.

## Initialisierung
Bevor die einzelnen Mover auf dem XTS bewegt werden können, muss eine Initialisierung erfolgen. Hierbei muss der Funktionsbaustein `FB_TcIoXtsEnvironment` mit Parametern der Struktur `ST_XtsEnvironmentConfiguration` initialisiert werden. Anschließend kann auf alle Parameter der Processing Unit des XTS mit einem Aufruf der entsprechenden Methode zugegriffen werden.

Folgende Deklarationen werden demnach benötigt:

```
nStartup			: INT;
fbEnv				: FB_TcIoXtsEnvironment;	        // XTS environment  
stEnvConfig			: ST_XtsEnvironmentConfiguration;   // XTS configuration
fbEStopXTS			: FB_TcIoXtsXpuPart;     // XTS Processing Unit of XTS part
bEStopXTS			: BOOL;
```

Die Initialisierung kann dann in einer Ablaufsteuerung wie folgt erfolgen:

```
CASE nStartup OF
	0: // set properties
		stEnvConfig.bEnableInitXpu 			:= TRUE;
 		stEnvConfig.bEnableInitInfoServer 	:= TRUE;
 		stEnvConfig.bEnableInitCaGroup 		:= FALSE;
        // update properties
		fbEnv.P_XtsEnvironmentConfiguration := stEnvConfig;
		nStartup := nStartup + 1;
		
	1: // init environment
		IF fbEnv.Init(TRUE) THEN	    // get object IDs from the TcCOM objects
 			fbEnv.Init(FALSE);		    // clear memory
 			nStartup := nStartup + 1;
 		END_IF
		
	2: // check if init succeeded
		IF fbEnv.P_IsInitialized THEN
			nStartup := nStartup + 1;
		END_IF

    3: // check XTS params
        // XpuTcIo = XTS Processing Unit TwinCatInOut
		IF NOT fbEnv.XpuTcIo(1).GetAreAllPositionsValid() 
            OR NOT fbEnv.XpuTcIo(1).GetIsTeachingValid() 
            OR NOT (fbEnv.XpuTcIo(1).GetDetectedMoverCount() = 
            fbEnv.XpuTcIo(1).GetExpectedMoverCount()) THEN
			; // wait
		ELSE
			nStartup := nStartup + 1;
		END_IF

    4: // init e-stop control with Id from part 1 inside XTS Processing Unit
		fbEStopXTS1.Init(bExecute := TRUE, nObjectId := 16#010102B0);
		nStartup := nStartup + 1;

    5: // finished
        ;
END_CASE
```

Folgende Parameter könnten anschließend aus der XTS Processing Unit ausgelesen werden:

```
bAllPositionsValid		:= fbEnv.XpuTcIo(1).GetAreAllPositionsValid();
bTeachingValid			:= fbEnv.XpuTcIo(1).GetIsTeachingValid();		
nDetectedMoverCount		:= fbEnv.XpuTcIo(1).GetDetectedMoverCount();
nExpectedMoverCount		:= fbEnv.XpuTcIo(1).GetExpectedMoverCount();
nPartCount	            := fbEnv.XpuTcIo(1).GetPartCount();
nTrackCount	            := fbEnv.XpuTcIo(1).GetTrackCount();
nModuleCount            := fbEnv.XpuTcIo(1).PartTcIo(1).GetModuleCount();
fTrackLength            := fbEnv.XpuTcIo(1).PartTcIo(1).GetLength();
```

## Collision Avoidance Group erstellen
Um Kollisionen von Movern zu vermeiden, kann eine Collision Avoidance Group (CA-Group) eingerichtet werden. 

1. Klicken Sie per Rechtsklick im Projektmappen-Explorer auf *MOTION/Objects* und wählen Sie `Neues Element hinzugefügen...` aus.
2. Wählen Sie im neuen Fenster unter *Motion Control* `CA Group` aus und bestätigen Sie auf OK.
3. Klicken Sie die neue Gruppe mit einem Doppleklick an.
4. Wechseln Sie zum Reiter *Parameter (Init)* und geben Sie folgende Parameter ein.

	<div align="center">
		<img decoding="async" src="../../../images/XTS/CAGroupParams.png" alt="Parameter CA-Group" width="450"> <br>
		Parameter CA-Group
	</div>

## Mover Ansteuerung
Für die Steuerung der Collision Avoidance Group und der einzelnen Mover wurden die Funktionsbausteine [FB_CaGroup](./Funktionsbausteine/FB_CaGroup.md) und [FB-Mover](./Funktionsbausteine/FB_Mover.md) geschrieben. Für das XTS müssen diese Bausteine initialisiert und zyklisch aufgerufen werden.

```
stMover	        : ARRAY [1..10] OF AXIS_REF;
stGroupRef	    : AXES_GROUP_REF;
fbMover		    : ARRAY[1..10] OF FB_Mover;
fbCaGroup		: FB_CaGroup;
nI              : INT;
-----------------------------------------------------------------
fbCaGroup(refCaGroupRef:=stGroupRef);
FOR nI:= 1 TO 10 DO
	fbMover[nI](
		nMoverID	:= TO_UDINT(nI), 
		refMover	:= stMover[nI], 
		fTrackLength:= fbEnv.XpuTcIo(1).PartTcIo(1).GetLength());
END_FOR	
```

Beim Bauen der SPS werden dann Instanzen der Referenzen hergestellt. Diese müssen mit den Achsen der Mover aus *MOTION* und der CA-Group verknüpft werden.

Zu Beginn sollten dann alle Mover auf dem XTS in die CA-Group aufgenommen werden. Anschließend können die einzelnen Achsen der Mover und die gesamte Gruppe freigeschaltet werden.

```
bAllAxisInGroup     : BOOL;
bAllMoversEnabled   : BOOL;
-----------------------------------------------------------------
IF nStartup = 5 THEN
    // check e-stop
    bEStopXTS1 := (fbEStopXTS1.GetDriveState() = DriveState.Fault);

    CASE nMoverState OF
        0: // add movers to collision avoidance group 
            bAllAxisInGroup := TRUE;
            FOR nI:=1 TO 10 DO
                bAllAxisInGroup := bAllAxisInGroup 
                    AND fbMover[nI].M_AddToGroup(TRUE, fbCaGroup.P_GroupRef) 
                    = E_MethodState.E_StateDone ;
            END_FOR
            IF bAllAxisInGroup THEN
                FOR nI:=1 TO 10 DO
                    fbMover[nI].M_AddToGroup(FALSE, fbCaGroup.P_GroupRef);
                END_FOR		
                nMoverState := nMoverState + 1;	
            END_IF

        1: // enable axis
            bAllMoversEnabled := TRUE;
            FOR nI:=1 TO 10 DO
                bAllMoversEnabled := bAllMoversEnabled 
                    AND fbMover[nI].M_PowerOn(100) 
                    = E_MethodState.E_StateDone 
                    AND fbMover[nI].P_AxisStatus.ControlLoopClosed;	
            END_FOR
            IF bAllMoversEnabled THEN
                nMoverState := nMoverState + 1;
            END_IF

        2: // enable group
            IF fbCaGroup.M_Enable() = E_MethodState.E_StateDone THEN
                nMoverState := 10;
            END_IF
    END_CASE
END_IF
```

Wenn `nMoverState` dann bei 10 ist, können die Mover bewegt werden. Hierzu können die Bewegungsparameter gesetzt und anschließend der Methode `M_MoveModulo` der Mover-Funktionsbausteine übergeben werden. Nachdem geschaut wurde, dass auch alle Mover sich bewegen, kann geprüft werden, welcher der Mover als erstes die Position erreicht. Alle anderen Mover stellen sich mit dem definierten Abstand hinten an und werden sich weiter bewegen, wenn der Weg vor ihnen frei ist.

```
stMoveParameter		: ST_MoveParameter;
bAllMoverActive     : BOOL;
nMoverID            : INT;
-----------------------------------------------------------------
10: // send all movers to position
	stMoveParameter.fPosition 	:= 2000.0;
	stMoveParameter.eDirection 	:= Tc2_MC2.MC_Positive_Direction;	
	FOR nI:=1 TO 10 DO
		fbMover[nI].M_MoveModulo(stMoveParameter);			
	END_FOR
	nMoverState := nMoverState + 1;
		
11: // movers active or done	
	bAllMoverActive := TRUE;
	FOR nI:=1 TO 10 DO
		bAllMoverActive := bAllMoverActive AND (fbMover[nI].P_MoveModulo.Active 
            OR fbMover[nI].P_MoveModulo.Done);	
	END_FOR
	IF bAllMoverActive THEN
		nMoverState := nMoverState + 1;
	END_IF

12: // look for a mover stopped at the position (window +- 1)
    FOR nI:=1 TO nMovers DO					
		IF fbMover[nI].P_CurrModuloPos < 2000.0 + 1 
          AND fbMover[n].P_CurrModuloPos > 2000.0 - 1 
          AND fbMover[n].P_GetVelo = 0.0 THEN
			nMoverID := nI;
			nMoverState := 20;
			EXIT;
		END_IF					
	END_FOR	
```

Die Achsen der Mover könnten im Fehlerfall über die SPS wie folgt resettet werden:

```
bAllAxisReset       : BOOL;
-----------------------------------------------------------------
bAllAxisReset:= TRUE;
FOR nI:=1 TO 10 DO
	bAllAxisReset := bAllAxisReset AND fbMover[nI].M_Reset(TRUE) = E_MethodState.E_StateDone;
END_FOR
IF bAllAxisReset THEN
	// continue
END_IF
```
