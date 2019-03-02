# Cave_Joycon
Sample scene for the cave with joycons

## Installation
1. git clone https://github.com/HTW-Berlin-CAVE/CAVE_JoyconSampleScene.git
2. git clone https://github.com/HTW-Berlin-CAVE/cave-package.git
3. Ordner cave-package-development/Packages/de.htw.cave in CAVE_JoyconSampleScene/Cave_Joycon/Packages/ kopieren

## Steuerung
Bewegung:
* Linker Joycon Stick: Bewegen
* Rechter Joycon Stick: Rotation

Interaktion:
* Hintere Schultertaste: Ball aufheben
* Vordere Schulterstaste: Ball wegwerfen
* Während der Ball gehalten wird, ist es möglich den Ball durch rotation des Controllers ebenfalls zu rotieren

## Wichtige Scripts
### PickupThrow (im Ball GameObject)
Befasst sich mit dem Aufheben, Werfen, rotieren des Balls und dem Vibrieren der Controller. Beispiel für die Nutzung der Joycons.

## Getting Started
### Anschließen der Joycons
Zur Nutzung der Joy-Cons müssen diese angemeldet werden, indem folgende Schritte ausgeführt werden:
1. "Windows-Taste" --> Systemsteuerung --> Geräte --> Bluetooth- oder andere Geräte hinzufügen.
2. Am Joy-Con Kontroller die Synchronisationstaste kurz gedrückt halten bis die Kontroll-LEDs leuchten.
3. Den hinzuzufügenden Kontroller in der Liste auswählen, im Eingabefeld "0000" eingeben und bestätigen.
4. Schritte 1-3 wiederholen um den zweiten Joy-Con anzumelden.

*Da keine Dauerhaft Kopplung der Joy-Con Kontroller mit dem Rechner möglich ist, muss der Prozess des Registierens nach jedem Neustart des Rechners wiederholt werden.*

### Wichtige Namespaces
#### Htw.Cave.Joycon
* Beinhaltet das JoyconHelper Skript
* ##### *wichtige* statische Methoden:
  * JoyconHelper.GetLeftJoycon() returned den linken Joycon oder null, falls keiner angeschlossen ist.
  * JoyconHelper.GetRightJoycon() returned den rechten Joycon oder null, falls keiner angeschlossen ist.
  * JoyconHelper.GetJoyconCount() returned die Anzahl an angeschlossenen Joycons
#### JoyconLib
* Beinhaltet die externe API für die Joycons
* Ermöglicht Zugriff auf das Joycon-Objekt und dessen statische Methoden
* ##### *wichtige* statische Methoden:
  * Joycon.Button.*\<Button\>* : Gibt den Keycode des jeweiligen Buttons zurück
* ##### *wichtige* Methoden des Joycon-*Objektes*:
  * *\<joycon\>*.SetRumble(low_freq, high_freq, amp, time) startet die Rumblefunktion des jeweiligen Joycons
  * *\<joycon\>*.GetButton(keycode) prüft, ob der Key, dessen Keycode als Parameter übergeben wurde gedrückt wird. 
  * *\<joycon\>*.GetButtonUp(keycode) prüft, ob der Key, dessen Keycode als Parameter übergeben wurde gedrückt wurde.
  * *\<joycon\>*.GetButtonDown(keycode) prüft, ob der Key, dessen Keycode als Parameter übergeben wurde released wurde. 
  * *\<joycon\>*.GetStick() returned ein float[2]-Array, wobei [0] die x-Achse und [1] die y-Achse des Sticks ist. Der Wertebereich liegt zwischen -1 und 1.
  * *\<joycon\>*.GetGyro() returned einen Vector3, der die aktuelle Rotation des Joycons beinhaltet
  * *\<joycon\>*.GetAccel() returned einen Vector3, der immer nach "unten" aus Sicht jeweiligen Joycon aus zeigt
  
**Wichtig! GetGyro() und GetAccel() funktionieren nicht wie man denkt und scheinen in der API noch nicht korrekt implementiert zu sein**

#### RumbleFeature
```
SetRumble(160.0f, 320.0f, 0.6f, 1000)
```
160.0f = Die untere Frequenzbandgrenze in Hz
320.0f = Die obere Frequenzbandgrenze in Hz
0.6f = Die Amplitude
1000 = Die Zeit der Vibration in ms
  
### Example
```C#
using UnityEngine;
using Htw.Cave.Joycon;
using JoyconLib;

public class Example : MonoBehaviour
{
    private Joycon leftJoycon;
        
    private void Update()
    {
       //caching des Joycon-Objektes
       leftJoycon = JoyconHelper.GetLeftJoycon();
       
       // In jedem Frame neu prüfen, ob der Joycon noch angeschlossen ist
       if (leftJoycon != null)
       {
          //Einen Buttondruck testen
          if (leftJoycon.GetButtonDown(Joycon.Button.SHOULDER_2))
          {
             //DoSomething
          }
       }
    }
 }
```
