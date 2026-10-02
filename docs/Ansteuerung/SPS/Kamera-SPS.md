# Kamera SPS-Einbindung

>[!NOTE] Hinweis
> Bevor Sie mit der SPS Programmierung beginnen, sollten Sie die [Einrichtung der Kamera](../Einrichtung/Kamera-Einrichtung.md) bereits durchgeführt haben.

## Bibliothek

>Eine Anleitung der Vision Bibliothek von Beckhoff finden Sie online oder im ILIAS-Kurs.

Das Paket *TF7000-TF7810 TwinCAT 3 Vision* mit der Bibliothek *Tc3_Vision* wird für die Steuerung der Kameras benötigt. Über den Funktionsbaustein `FB_VN_GevCameraControl` kann die Initialisierung und Bildaufnahme gesteuert werden. Die Kamera hat dabei einen Status vom Typ `ETcVnCameraState`. Die folgende Abbildung zeigt die möglichen Übergänge der States.

<div align="center">
	<img decoding="async" src="../../../images/camera/Kamera-States.png" alt="States von FB_VN_GevCameraControl" width="400" height="500"> <br>
	 State Machine von FB_VN_GevCameraControl [aus  <a href="https://infosys.beckhoff.com/content/1033/tf7xxx_tc3_vision/16934658571.html" >Beckhoff Automation</a>]
</div>

Bilder werden vom Typ `ITcVnImage` aufgenommen. Mit diesem Datentyp kann in der SPS weiter gearbeitet werden. Soll das Bild anzeigbar sein, muss es mit der Funktion `F_VN_CopyIntoDisplayableImage` in den Datentyp `ITcVnDisplayableImage` transformiert werden. In der Menüleiste unter TwinCAT/Windows/ADS Image Watch kann dann das Bild angezeigt werden.

## Programmierung
Der Funktionsbaustein kann wie folgt angewendet werden:

```
VAR
	eCamState			: ETcVnCameraState;			// camera state
	fbGevCameraControl	: FB_VN_GevCameraControl;	// GigE Vision Camera Control
	bLightCam	AT %Q* 	: BOOL;				        // light for camera
	ipImg				: ITcVnImage;				// image to work with
	ipDispImg	        : ITcVnDisplayableImage;	// image for displaying
	hr					: HRESULT	:=	S_OK;		// operation results
	bStart              : BOOL;                     // trigger image
END_VAR
--------------------------------------------------------------------------------
// get camera status
eCamState 	:= fbGevCameraControl.GetState();

CASE eCamState OF
	TCVN_CS_ERROR: // reset to initial state
		hr := fbGevCameraControl.Reset();
		
	TCVN_CS_INITIAL, TCVN_CS_INITIALIZING, TCVN_CS_INITIALIZED,	TCVN_CS_OPENING,
	TCVN_CS_OPENED, TCVN_CS_STARTACQUISITION: // change to acquiring
		hr        := fbGevCameraControl.StartAcquisition();
		bLightCam := TRUE;
		
	TCVN_CS_ACQUIRING: // recording state
		IF bStart THEN
			hr := fbGevCameraControl.TriggerImage();
		ELSE
			FW_SafeRelease(ADR(ipImg));
			hr := fbGevCameraControl.GetCurrentImage(ipImg);
			hr := F_VN_CopyIntoDisplayableImage(ipImg, ipDispImg, hr);
		END_IF
		
	TCVN_CS_TRIGGERING: // trigger image
		hr := fbGevCameraControl.TriggerImage();
END_CASE
```

## Instanziierung
Nach dem Erstellen des Projekts muss die Instanz des Lichts und die Kamerasteuerung verknüpft werden. Bei dem Licht kann wie gewohnt vorgegangen werden. Zur Verknüpfung der Kamerasteuerung muss per Doppelklick auf die SPS Instanz geklickt werden. Anschließend kann im Reiter `Symbol Initialisierung` durch das Klicken auf den Wert beim Eintrag der Kamera der *Image Provider* zugeordnet werden.
