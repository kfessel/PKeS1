<!--

author:   Konstantin Kirchheim

email:    konstantin.kirchheim@ovgu.de

version:  1.0.0

language: de_DE

narrator:  Deutsch Female

-->

# Einführung

--{{1}}--
Willkommen zurück im eLearning-System *eLab*. Da ihr euch während der letzten Aufgabe sowohl mit dem System, als auch mit dem Arbeitsablauf vertraut machen konntet, wird es in dieser Aufgabe darum gehen, tiefer in die Programmierung eingebetteter Systeme einzudringen. 

--{{2}}--
Ein wesentliches Merkmal eingebetteter Systeme ist, dass sie durch periphäre Hardware mit ihrer Umgebung interagieren können. Dafür müssen diese Hardwarekomponenten jedoch in Software repräsentiert und angesprochen werden können. Wie auch bei einem Desktop-Computer, geschieht das durch Treiber.

--{{3}}--
Zur Einführung in die Treiberentwicklung eingebetteter Systeme, wird es in dieser Aufgabe darum gehen, die Funktionalität zur Ansteuerung von einem, bzw. von drei, 8-Segment-Displays zu Implementieren.


## Themen und Ziele

**Themen:**

* Ansteuerung von Displays und Shift-Registern
* Kapselung Gerätespezifischer Informationen in abstrahierenden Funktionen (Treiberentwicklung)
* Implementierung entsprechend einer gegebenen API

**Ziel(e):**

* Programmierung von Hardwareschnittstellen
* Bereitstellen von API Funktionen

## Weitere Informationen

**Treiber und Treiberentwicklung:**

* [Treiber](https://en.wikipedia.org/wiki/Device_driver)
* [AVR GCC Inline Assembler Cookbook](http://www.nongnu.org/avr-libc/user-manual/inline_asm.html)
* [YouTube-Tutorial zur Assemblerprogrammierung](https://www.youtube.com/watch?v=ViNnfoE56V8)
* [API](https://en.wikipedia.org/wiki/Application_programming_interface)

**PKeS**
* [Schaltbelegungsplan](https://github.com/liaScript/PKeS0/blob/master/materials/robubot_stud.pdf?raw=true)
* 8-Segmente Anzeige:
  * [Wikipedia](https://en.wikipedia.org/wiki/Seven-segment_display)
  * [Datenblatt] (http://www.kingbrightusa.com/images/catalog/SPEC/SA52-11SRWA.pdf)
* [Shift-Register](https://www.sparkfun.com/datasheets/IC/SN74HC595.pdf)
* [Arduinoview] (https://github.com/fesselk/Arduinoview/blob/master/doc/Documetation.md)
* [Datenblatt des AVR ATmega32U4] (http://www.atmel.com/Images/Atmel-7766-8-bit-AVR-ATmega16U4-32U4_Datasheet.pdf)

# Aufgabe 1

--{{1}}--
In der *ersten* praktischen Aufgabe sollt ihr einen Treiber für das Display unserer Roboterplattform implementieren. Dazu haben wir euch diesesmal lediglich die `Display.h`-Datei vorgegeben, in der einige Funktionen deklariert sind. Es gilt in dieser Aufgabe diese *API* zu implementieren.

--{{2}}--
Wie schon durch Aufgabe *null* bekannt, wird es euch helfen, die Aufgabe in zwei Teilaufgaben zu bearbeiten. 

--{{3}}--
In der ersten Teilaufgabe werdet ihr daher zunächst die Kommunikation zwischen dem Mikrocontroller und dem Display implementieren.

--{{4}}--
In der zweiten Teilaufgabe werdert ihr die Funktionalität des Treibers vervollständigen, sodass ihr sowohl Gleitkommazahlen, als auch Ganzzahlen auf dem Display darstellen könnt.

--{{5}}--
Zu letzt werdert ihr den implementierten Treiber nutzen um vorgegebene Zahlenwerte darzustellen.

**Teilaufgaben:**

* *1.1:* Implementiert die Grundfunktionalität zum Ansteuern des 8-Segment-Displays.
* *1.2:* Implementiert die restlichen Funktionen der vorgegebenen API zur einfachen Darstellung von Ganzzahlen und Gleitkommazahlen.
* *1.3:* Implementiert die Anbindung zum Arduinoview-Interface.


## Aufgabe 1.1 

--{{1}}--
In dieser Teilaufgabe sollt ihr zunächst den Datenfluss von dem Mikrocontroller zu dem Display verstehen und in einer grundlegende Ansteuerung implementieren.

--{{2}}--
Wie ihr sicherlich schon bemerkt habt, besteht das Display selbst aus drei eigenständigen 8-Segment-Anzeigen.

--{{3}}--
Um zu verstehen, wie diese angeschlossen sind, solltet ihr den [Schaltbelegungsplan](https://github.com/liaScript/PKeS0/blob/master/materials/robubot_stud.pdf?raw=true) eingehend studieren. Hilfreich ist es, die jeweiligen Ein- und Ausgänge der Komponenten *gedanklich* miteinander zu verbinden. Ihr könnt dazu bei den Ausgängen des Mikrocontrollers beginnen und euch bis zum Anschluss jedes einzelnen 8-Segment-Displays vorarbeiten, oder den Weg umgekehrt gehen. Beantwortet für euch die Frage: Durch welche Leiter muss ein einzelnes Bit geleitet werden, um schließlich ein einzelnes Segment eines bestimmten Displays ein- oder auszuschalten?

--{{4}}--
Während der Untersuchung des Datenflusses, sind euch sicherlich die [Shift-Register](https://www.sparkfun.com/datasheets/IC/SN74HC595.pdf) als zentrale Bausteine angeschlossen. Um im zweiten Teilschritt die Funktion `initDisplay()` implementieren zu können, solltet ihr auch dieses Datenblatt studieren. Wie funktionieren Shift-Register? Wie sind unsere verschaltet und was muss daher bei der Kommunikation mit ihnen (auch zeitlich) beachtet werden?

--{{5}}--
Nachdem nun die theoretischen Fragen zu den von dieser Aufgabe betroffenen Bauteilen geklärt sein sollten, können wir mit der Implementierung beginnen. 

--{6}}--
Wie schon angedeutet, soll die in `Display.h` deklariert API implementiert werden. Dazu solltet ihr eine Datei `Display.cpp`, sowie beliebige weitere Dateien, anlegen.

--{{7}}--
Zunächst müssen, wie schon bei der vorherigen Aufgabe, die entsprechenden PINs als Ausgänge konfiguriert werden. Darüber hinaus sollte das Display mit einem *default*-Wert gefüllt werden. Diese Funktionalität solltet ihr mit der Funktion `initDisplay()` abdecken.

--{{8}}--
Zu letzt soll eine grundlegende Funktionalität (`writeToDisplay(uint8_t data[3])`) zum Ausgeben von drei Byte implementiert werden. Nehmt dabei an, dass die bits der drei Werte des Arrays `data` bereits die richtige Bitreihenfolge beachten.

**Ziel:**
Das Ziel in dieser Aufgabe ist es, den Datenfluss, der zur Ansteuerung des Displays notwendig ist, zu verstehen und den Mikrocontroller entsprechend zu konfigurieren. Am Ende der Teilaufgabe sollte es euch dadurch mögich sein, einen *default*-Wert auf dem Display auszugeben.


**Teilschritte:**

1. Verständniss des Datenflusses.
   * Wie wird ein einzelnes Segment angesteuert?
     ![8SegmentDisplay]() => ![ShiftRegister]() => ![ConnectionBetweenShiftRegister]()
   
   * Zusätzlich solltet ihr das [Datenblatt des Displays](http://www.kingbrightusa.com/images/catalog/SPEC/SA52-11SRWA.pdf) studieren um zu verstehen, wie die Eingänge des Displays den einzelnen LEDs zugeordnet sind.
   
2. Implementierung der Funktion `initDisplay()`
   * Welche PINs müssen als Ausgänge definiert werden?
   * Wie können wir einen *default*-Wert, z.B.: '===', in die Shift-Register übertragen?
   * Wodurch können wir die Anzeige der Werte auf dem Display veranlassen?
   
3. Implementierung der Funktion `writeToDisplay(uint8_t data[3])`
   * Übertragt die drei Byte des Arrays in die Shift-Register und zeigt die Werte auf dem Display an.


## Aufgabe 1.2 

--{{1}}--
In der letzten Teilaufgabe haben wir eine grundlegende Funktionalität zur Ausgabe von bereits korrekt definierten Bytes auf dem Display implementiert. In dieser Teilaufgabe soll nun die Frage beantwortet werden, wie wir von der Darstellung von Standarddatentypen, in diesem Fall `int` und `float`, zu dieser entsprechenden Bitdarstellung kommen.

--{{2}}--
Dazu soll die Funktion `writeValueToDisplay(int value)` sowie ihre Überladung mit dem `float`-Datentypen implementiert werden. Bei beiden Funktionen muss jedoch der darstellbare Wertebereich beachtet werden! Versucht so viel Stelen wie möglich auf dem Display anzuzeigen. Was passiert/sollte passieren, wenn wir den Funktionen einen nicht-darstellbaren Wert übergeben?

--{{3}}--
Für die erfolgreiche Bearbeitung der Aufgabe kann es hilfreich sein, eine (mehr oder weniger) standardisierten [Darstellung von Zeichen für 7-Segment-Anzeigen](https://en.wikipedia.org/wiki/Seven-segment_display) als Zwischenrepräsentation zu implementieren.


**Ziel:**
Vervollständigt die Implementierung der API in dem ihr die Funktionen `writeValueToDisplay(int value)` und `writeValueToDisplay(float value)` zum Anzeigen von Ganz- und Gleitkommazahlen.

**Teilschritte:**
1. Implementiert eine konvertierung von `int` zur Bitrepräsentation der Ganzzahl entsprechend der Ansteuerung des Displays
  0. (optional) Konvertiert die Zahl zunächst in eine Standardrepräsentation wie sie zum Beispiel [hier](https://en.wikipedia.org/wiki/Seven-segment_display) vorgestellt wird.
  1. Generiert eine Bitdarstellung in `uint8_t` zur Übergabe an die Funktion `writeToDisplay`.
2. Wiederholt die Schritte aus **1.** für den `float`-Datentyp.




## Aufgabe 1.3

--{{1}}--
Durch die Teilaufgabe 1.2 sollte die Implementierung des Treibers für unser Display abgeschlossen sein. Nun können wir ihn zur Darstellung von Zahlen testen. Daher soll in dieser Aufgabe das Arudinoview-Interface genutzt werden um Zahlen zu generieren, die ihr dann über euren Treiber auf dem Display darstellt.

--{{2}}--
Wir haben euch bereits einen Slider vorgegeben, durch den ihr Werte in dem Bereich von -200 bis 1000 generieren könnt. Diese werden in der Callback-Funktion `CallbackSL` als [null-terminierter String](https://www.tutorialspoint.com/cprogramming/c_strings.htm) übergeben. 

--{{3}}--
Da wir allerdings auch unsere Ganzzahl-Implementierung der API testen möchten, solltet ihr in einem ersten Schritt dem Arduinoview-Interface noch eine CheckBox hinzufügen. Durch die soll es möglich sein, zwischen der `int` und der `float`-Darstellung dynamisch zu wechseln.

--{{4}}--
In dem finalen, zweiten Schritt könnt ihr die durch die Funktion `CallbackSL` übermittelten Werte in `int` bzw. `float`-Datentypen konvertieren und sie mittels den Funktionen `writeValueToDisplay(int value)` und `writeValueToDisplay(float value)` darstellen.

--{{5}}--
Zusätzlich kann es hilfreich sein dem Interface noch ein Text-Input zu direkten Übergabe von Werten hinzuzufügen. Alternativ könnt ihr natürlich auch Werte von der seriellen Schnittstelle einlesen.


**Teilschritte:**
1. Fügt eine CheckBox zum Wechseln zwischen `float` und `int`-Darstellung
2. Zeigt die Ganz- und Gleitkommazahlen auf dem Display an.

