Aktoren
-------
***

> [⇧ **Home**](https://github.com/iotkitv4/intro)


Aktoren (Wandler; Antriebselemente) setzen die elektronischen Signale in mechanische Bewegung oder andere physikalische Grössen um und greifen damit aktiv in die Umgebung des eingebetteten Systems ein.

**Beispiele**: 
* Dioden, 7-Segement-Anzeigen, Displays
* Ventile (Pneumatik, Hydraulik)
* Motoren (Gleichstrom/Wechselstrom)
* Magnete (Manipulatoren, Lautsprecher)


**Anwendungen**:
*   In vielen Robotern kommen Standard Boards mit individuellen Shield&#039;s zum Einsatz.
*   Der Siegeszug der DIY (Do-it-yourself) 3D Druckern, wäre ohne die Arduino Mega Boards nicht denkbar gewesen.
*   LED Strips eröffnen neue Möglichkeiten für die Dekorative Beleuchtungen von Gegenständen und Räumen.

### Beispiele

* [DC Motor](#Gleichstrom-Motor) 
* [Servo](#Servo) 
* [Schrittmotor](#Schrittmotor)
* [Türöffner](#türöffner)
* [RGB LED Streifen - 5 Volt](#RGB-LED-Streifen)
* [Fernseh Simulator](#Fernseh-Simulator)


* [Übungen](#Übungen)

## Gleichstrom Motor
***

> [⇧ **Nach oben**](#beispiele)

![](https://raw.githubusercontent.com/iotkitv4/intro/main/images/actors/Motor.png) 

[Motoren](http://de.wikipedia.org/wiki/Elektromotor)

- - -

Elektromotor bezeichnet einen elektromechanischen Wandler (elektrische Maschine), der elektrische Energie in mechanische Energie umwandelt. In herkömmlichen Elektromotoren wird die Kraft, die von einem Magnetfeld auf die stromdurchflossenen Leiter einer Spule ausgeübt wird, in Bewegung umgesetzt.

Der Motor wird mittels eines float Wertes von full Speed rueckwärts (-1.0) nach full Speed vorwärts (1.0) angesprochen.

Ein ruhiger und schonender Motorlauf wird durch die Anpassung der PWM Periode (in Motor.cpp) erreicht. Diese PWM Periode bezieht sich auf die Motor Frequenz (siehe Datenblatt Motor) und wird wie folgt berechnet:

*   Periode (s) = 1 / Frequenz (Hz = 1/s)

Sourcecode aus Motor.cpp

    // Set initial condition of PWM
    _pwm.period( 1.0f / 50000 );

Ein Motor benötigt die [Motor Library](http://developer.mbed.org/users/simon/code/Motor/) und eine Verstärkerschaltung, wie z.B. eine [H-Brücke](http://de.wikipedia.org/wiki/Br%C3%BCckenschaltung). Eine H-Brücke braucht einen PWM Pin und zwei beliebige Digital Pins pro Motor. Es können zwei Motoren an die H-Bridge **M1** und **M2**  Header angeschlossen werden.

### Anwendungen 

*   Antrieb von Bahnen, Elektrokarren, Gabelstabel, Funkgesteuerte Modellautos (RC-Car), Robotern etc.

### Beispiel(e)

Das [Beispiel](main.cpp) bewegt 2 DC Motoren vorwärts und rückwärts. 

## Servo 
***

> [⇧ **Nach oben**](#beispiele)

![](https://raw.githubusercontent.com/iotkitv4/intro/main/images/actors/ServoOpen.png) ![](https://raw.githubusercontent.com/iotkitv4/intro/main/images/actors/ServoSignal.png)

[http://wiki.rc-network.de/Servo](http://wiki.rc-network.de/Servo)

- - -

Der Servo (auch Rudermaschine) hat die Aufgabe, entsprechend dem Signal, dass er vom Empfänger erhält, die Ruder (oder andere Komponenten am Modell) zu stellen.

Servo lassen sich, in der Regel, von 0 - 180° bewegen. Der entsprechende Stellwinkel wird mittels eines Wert von 0.0 bis 1.0 angegeben.

Es gibt analoge und digitale Servos. Der Unterschied liegt darin, dass digitale Servo erst anfangen den Stellwinkel zu wechseln, wenn ein sauberes Signal anliegt.

Weitere Informationen und eine Ausführliche Einführung in Englisch [An Introduction to RC Servos](http://developer.mbed.org/users/4180_1/notebook/an-introduction-to-servos/)

### Anwendungen 

*   Steuerung von Roboterarmen
*   Modellflugzeuge
*   Schalten von Weichen auf der Modelleisenbahn

### Anschlussbelegung (Servo - Shield) 

Der Servo wird mit 5V betrieben und kann direkt auf einen der GND/+5V/S Header gesteckt werden. Das orange Kabel des Servo kommt auf S (Signal).

### Beispiel(e)

Das Beispiel bewegt 4 Servos.

<details><summary>main.cpp</summary>  

    #include "mbed.h"
    #include "Servo.h"
    
    Servo servo1( MBED_CONF_IOTKIT_SERVO1 );
    Servo servo2( MBED_CONF_IOTKIT_SERVO2 );
    Servo servo3( MBED_CONF_IOTKIT_SERVO3 );
    Servo servo4( MBED_CONF_IOTKIT_SERVO4 );
    
    int main()
    {
        while (1) 
        {
            for ( float offset = 0.0f; offset < 1.0f; offset += 0.05f )
            {
                servo1.write( offset);
                servo2.write( offset);
                servo3.write( offset);
                servo4.write( offset);
                thread_sleep_for( 250 );
            }
            for ( float offset = 1.0f; offset > 0.0f; offset -= 0.05f )
            {
                servo1.write( offset);
                servo2.write( offset);
                servo3.write( offset);
                servo4.write( offset);
                thread_sleep_for( 250 );
            }        
        }
    }
    
</p></details> 


## Schrittmotor
***

> [⇧ **Nach oben**](#beispiele)

![](https://raw.githubusercontent.com/iotkitv4/intro/main/images/actors/Stepper.png)

[Schrittmotor](http://de.wikipedia.org/wiki/Schrittmotor)

- - - 

![](https://raw.githubusercontent.com/iotkitv4/intro/main/images/actors/StepperWiring.png)

Ansteuerung Schrittmotor

- - - 

Ein Schrittmotor ist ein Synchronmotor, bei dem der Rotor (drehbares Motorteil mit Welle) durch ein gesteuertes, schrittweise rotierendes, elektromagnetisches Feld der Statorspulen (nicht drehbarer Motorteil) um einen minimalen Winkel (Schritt) oder sein Vielfaches gedreht werden kann.

Ein Schrittmotor hat eine fixe Schrittanzahl pro Umdrehung. Beim verwendeten [28BYJ-48](http://arduino-info.wikispaces.com/SmallSteppers) sind es 2048 Schritte.

Zur erstmaligen Positionierung wird, in der Regel, ein Endstop Schalter verwendet. [CNC Maschinen](http://de.wikipedia.org/wiki/CNC-Maschine) besitzen zusätzlich, wegen der Verletzungsgefahr einen Notstopp Schalter mit Einrastfunktion.

Ein unipolarer Schrittmotor benötigt einen IC Treiber (wie [ULN2803N](http://www.mikrocontroller.net/part/ULN2803)) und 4 Digitale Pins. Es kann je ein Schrittmotore an den Header **Stepper1** oder **Stepper2** angeschlossen werden.

Ein bipolarer Schrittmotor kann mittels der H-Brücke angesprochen werden, siehe Gleichstrom Motor und [Stepper bipolar](https://os.mbed.com/components/Stepper-motor-bipolar/)

### Anwendungen 

*   Typische Anwendungsgebiete sind Drucker oder der Antrieb des Schreib-/Lesekopfes in einem CDROM Laufwerken. Aufgrund ihrer hohen Genauigkeit werden sie auch in computergesteuerten Werkzeugmaschinen zur Positionierung der Werkzeuge verwendet. Durch die ständig sinkenden Kosten für die Ansteuerelektronik werden sie auch zunehmend im Konsumgüterbereich verwendet. So sind in Kraftfahrzeugen der mittleren und gehobenen Kategorie heute bis über 50 Schrittmotoren im Einsatz, die Betätigung der vielen Klappen einer automatischen Heizungs- und Klimaanlage ist dafür ein Beispiel.

### Beispiel(e)

Das Beispiel bewegt 2 Schrittmotoren vorwärts und rückwärts.

<details><summary>main.cpp</summary>  

    /** Schrittmotor Beispiel 
        Schrittmotor an den mit STEPPER1 und STEPPER2 Steckern mit rotem Kabel nach + einstecken.
    */
    #include "mbed.h"
    
    // Stepper 1
    DigitalOut s1( MBED_CONF_IOTKIT_STEPPER1_1 );
    DigitalOut s2( MBED_CONF_IOTKIT_STEPPER1_2 );
    DigitalOut s3( MBED_CONF_IOTKIT_STEPPER1_3 );
    DigitalOut s4( MBED_CONF_IOTKIT_STEPPER1_4 );
    
    // Stepper 2
    DigitalOut s5( MBED_CONF_IOTKIT_STEPPER2_1 );
    DigitalOut s6( MBED_CONF_IOTKIT_STEPPER2_2 );
    DigitalOut s7( MBED_CONF_IOTKIT_STEPPER2_3 );
    DigitalOut s8( MBED_CONF_IOTKIT_STEPPER2_4 );
    
    static int s = 5;
    
    int main()
    {
        // Motordrehzahl
        printf( "Schrittmotor Test\n" );
    
        while ( 1 ) 
        {
            printf( "vorwaerts\n" );
            for ( int i = 0; i < 1024/4; i++ )
            {
                s1 = 1; s2 = 0; s3 = 0; s4 = 0;
                s5 = 1; s6 = 0; s7 = 0; s8 = 0;
                thread_sleep_for( s );
                s1 = 0; s2 = 1;
                s5 = 0; s6 = 1;
                thread_sleep_for( s );
                s2 = 0; s3 = 1;
                s6 = 0; s7 = 1;
                thread_sleep_for( s );
                s3 = 0; s4 = 1;
                s7 = 0; s8 = 1;
                thread_sleep_for( s );
            }
            thread_sleep_for( 500 );
            printf( "rueckwaerts\n" );
            for ( int i = 0; i < 1024/4; i++ )
            {
                s4 = 1; s3 = 0; s2 = 0; s1 = 0;
                thread_sleep_for( s );
                s4 = 0; s3 = 1;
                thread_sleep_for( s );
                s3 = 0; s2 = 1;
                thread_sleep_for( s );
                s2 = 0; s1 = 1;
                thread_sleep_for( s );
            }
            thread_sleep_for( 500 );
        }
    }
   
</p></details> 



## Türöffner
***

> [⇧ **Nach oben**](#beispiele)

![](https://raw.githubusercontent.com/iotkitv4/intro/main/images/actors/DoorOpener.png)

[Elektromagnetischer Türöffner](http://de.wikipedia.org/wiki/T%C3%BCrschloss)

- - -

![](https://raw.githubusercontent.com/iotkitv4/intro/main/images/actors/DoorOpenerWiring.png)

[Leistungsstufe (MOSFET o.ä.)](http://developer.mbed.org/users/4180_1/notebook/relays1/)

- - -

Türöffner gibt es auch als elektrisches Bauteil. Der Riegel wird durch einen elektromagnetischen Magnet geöffnet.

Damit genug Leistung vorhanden ist, wird eine Leistungsstufe, z.B. [MOSFET](http://de.wikipedia.org/wiki/Metall-Oxid-Halbleiter-Feldeffekttransistor), vorgeschaltet.

Der Türöffner wird an den MOSFET (Header **MOSFETs**) oder Schrittmotor Leistungsstufe (Header **STEPPER1** beim DISCO_L475VG_IOT01A Shield) angeschlossen und mittels DigitalOut angesprochen.

### Anwendungen 

*   Elektrische Türöffner
*   Schliesssysstem, z.B. in Verbindung mit [RFID Reader](http://de.wikipedia.org/wiki/RFID)

### Beispiel(e)

Das Beispiel öffnet den Türschliesser beim Druck auf den Button.

<details><summary>main.cpp</summary>  

    /** Tueroeffner */
    
    #include "mbed.h"
    
    DigitalIn button1( MBED_CONF_IOTKIT_BUTTON1 );
    DigitalOut mosfet( MBED_CONF_IOTKIT_MOSFET1 );
    DigitalOut led( MBED_CONF_IOTKIT_LED1 );
    
    int main()
    {
        while   ( 1 ) 
        {
            led = 0;
            mosfet = 0;
            if  ( button1 == 0 ) 
            {
                led = 1;
                mosfet = 1;
                thread_sleep_for( 3000 );
            }
    
        }
    }
    
</p></details>


## RGB LED Streifen 
***

> [⇧ **Nach oben**](#beispiele)

![](https://raw.githubusercontent.com/iotkitv4/intro/main/images/actors/LEDStripsTreppe.png)

[Beispiel Treppenbeleuchtung](http://developer.mbed.org/users/4180_1/notebook/relays1/)

- - - 

 ![](https://raw.githubusercontent.com/iotkitv4/intro/main/images/actors/LedStrips12V.png) |

[RGB LED Strip, siehe Wiki LadyAda](http://www.ladyada.net/wiki/products/ledstrip/index.html) 

- - -

LED Strips (RGB LED Streifen) eröffnen neue Möglichkeiten für die Dekorative Beleuchtungen von Gegenständen und Räumen.

LED Strips werden in Laufmetern mit einer definierten Anzahl von RGB LEDs pro Meter verkauft.

Es gibt unterschiedliche Arten der Ansteuerung, alle LED einer Farbe, jedes RGB LED einzeln.

Im aktuellen Beispiel verwenden wird ein 5V LED Strip mit einem Anschluss pro Farbe. Diese brauchen einen Verstärker, z.B. [MOSFET](http://de.wikipedia.org/wiki/Metall-Oxid-Halbleiter-Feldeffekttransistor).

Die LED Strip wird an den MOSFETs Header (+ ist oben) oder an die Stepper Leistungsstufe (Header **STEPPER1** beim DISCO_L475VG_IOT01A Shield) angeschlossen und benötigen für jede Farbe ein DigitalOut (An/Aus) oder PwmOut (Dimming).

### Anwendungen 

*   Raumbeleuchtung
*   Dekorative Ausleuchtung von Gegenständen

### Beispiel(e)

Das Beispiel bringt die 3 Farben (grün, rot, blau) nacheinnander und gemeinsam (= weiss) zum leuchten.

<details><summary>main.cpp</summary>  

    /** Beispiel RGB LED Strip 5 Volt Variante mit einer Leitung pro Farbe
    */
    #include "mbed.h"
    
    PwmOut green( MBED_CONF_IOTKIT_MOSFET1 );
    PwmOut red( MBED_CONF_IOTKIT_MOSFET2 );
    PwmOut blue( MBED_CONF_IOTKIT_MOSFET3 );
    
    void off()
    {
        printf( "off \n" );
        red = 0;
        green = 0;
        blue = 0;
        thread_sleep_for( 1000 );
    }
    
    void dim( PwmOut& pin )
    {
        printf( "dim\n" );
        for ( float i = 0.0f; i < 1.0f; i += .01f )
        {
            pin = i;
            thread_sleep_for( 200 );
        }
        thread_sleep_for( 1000 );
        
    }
    
    int main() 
    {
        while   ( true )
        {
            dim( red );
            off();
            dim( green );
            off();
            dim( blue );
            off();
            
            red = 1;
            thread_sleep_for( 1000 );
            off();
            
            green = 1;
            thread_sleep_for( 1000 );
            off();
            
            blue = 1;
            thread_sleep_for( 1000 );
            off();
            
            red = 1;
            blue = 1;
            green = 1;
            thread_sleep_for( 1000 );
            off();
        }
    }
    
</p></details>

### Links

* [DiiA: The global industry alliance for DALI lighting control](https://www.digitalilluminationinterface.org/)
* [Zhaga](https://www.zhagastandard.org/)

## Fernseh Simulator
***

> [⇧ **Nach oben**](#beispiele)


Mittels eine paar farbigen LEDs kann ein einfacher Fernseh Simulator erzeugt werden. Durch das Abwechslungsweise aufblicken der Farben Rot, Grün und Blau wird ein Fernseh ähnliches Lichtspiel erzeugt.

Solche Geräte werden, z.B. von Pearl verkauft.

### Beispiel(e)

Das Beispiel verwendet die Verbauten LEDs und den Zufallsgenerator vom Board.

<details><summary>main.cpp</summary>  

    /** Zahlfallszahlen erzeugen und damit Fernsehsimulator fuettern
    */
    #include "mbed.h"
    #include <time.h>
    
    DigitalOut led[] =  { DigitalOut( MBED_CONF_IOTKIT_LED1 ), DigitalOut( MBED_CONF_IOTKIT_LED2 ), DigitalOut( MBED_CONF_IOTKIT_LED3 ), DigitalOut( MBED_CONF_IOTKIT_LED4 ) };
    
    int main()
    {
        printf( "Fernsehsimulator\n" );
         
        time_t t;
        time(&t);
        srand( (unsigned int)t );              /* Zufallsgenerator initialisieren */
    
        while   ( 1 )
        {
            for ( int i = 0; i < 4; i++ )
            {
                int r = rand();
                led[i] = ( (r % (i+2) ) != 0 ) ? 1 : 0;
            }
                
            thread_sleep_for( 0.2f );          
        }
    }
    
</p></details>

### Links

* [Kommerzielle Produkte](https://www.pearl.ch/ch-kw-1-fernseh+simulator.shtml) 


## Übungen
***

> [⇧ **Nach oben**](#beispiele)

Ein paar der Übungen funktionieren nur mit dem IoTKitV3 K64F, weil der Encoder benötigt wird.

### Servo

nach links oder rechts bewegen mittels Encoder

* Anwendung: Roboterarm bewegen

<details><summary>Lösung</summary>  

    /** Servo nach links oder rechts bewegen mittels Encoder.
    */
     
    #include "mbed.h"
    #include "QEI.h"
    #include "OLEDDisplay.h"
    
    // UI
    OLEDDisplay oled( MBED_CONF_IOTKIT_OLED_RST, MBED_CONF_IOTKIT_OLED_SDA, MBED_CONF_IOTKIT_OLED_SCL );
    
    // Servo
    PwmOut servo1( MBED_CONF_IOTKIT_SERVO1 );
    
    // Encoder bestimmt die Position
    QEI wheel (MBED_CONF_IOTKIT_BUTTON2, MBED_CONF_IOTKIT_BUTTON3, NC, 624);
     
    int main()
    {
        float value = 0;
        float offset;
        oled.clear();
        oled.printf( "Servo\n" );
    
        servo1.period(0.020);          // servo requires a 20ms period
    
        while(1)
        {
            oled.cursor( 1, 0 );
            value = (float) wheel.getPulses();
            oled.printf("Value: %4.0f\n", value );
    
            // 0 = rechts, > 0 geht nach links
            if  ( value >  0.0f )
                offset = value / 100000.0f;
            else
                offset = 0.0f;
            servo1.pulsewidth(0.001f + offset);
    
            oled.cursor( 2, 0 );
            oled.printf("Pos  : %0.6f\n", offset );
    
            thread_sleep_for( 100 );
        }
    }
    
</p></details>

### Motor

mittels Encoder vor-, rückwärts Laufen lassen und zu stoppen.

<details><summary>Lösung</summary>  

    /** Servo nach links oder rechts bewegen mittels Encoder.
    */
    
    #include "mbed.h"
    #include "QEI.h"
    #include "OLEDDisplay.h"
    #include "Motor.h"
    
    // UI
    OLEDDisplay oled( MBED_CONF_IOTKIT_OLED_RST, MBED_CONF_IOTKIT_OLED_SDA, MBED_CONF_IOTKIT_OLED_SCL );
    
    // Motor
    Motor m1( MBED_CONF_IOTKIT_MOTOR1_PWM, MBED_CONF_IOTKIT_MOTOR1_FWD, MBED_CONF_IOTKIT_MOTOR1_REV ); // PWM, Vorwaerts, Rueckwarts
    
    // Encoder bestimmt die Position
    QEI wheel (MBED_CONF_IOTKIT_BUTTON2, MBED_CONF_IOTKIT_BUTTON3, NC, 624);
    // Button - Stop
    DigitalIn button1( MBED_CONF_IOTKIT_BUTTON1 );
    
    int main()
    {
        float value = 0;
        float offset;
        oled.clear();
        oled.printf( "Motor\n" );
    
        while(1)
        {
            oled.cursor( 1, 0 );
    
            // Stop
            if  ( button1 == 0 )
                wheel.reset();
    
            // aktuelle Position
            value = (float) wheel.getPulses();
    
            oled.printf("Speed: %2.2f\n", value );
    
            m1.speed( value / 10.0f );
            thread_sleep_for( 100 );
        }
    }
    
</p></details>


