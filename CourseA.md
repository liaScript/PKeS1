<!--

author:   Konstantin Kirchheim

email:    konstantin.kirchheim@ovgu.de

version:  1.0.0

language: de_DE

narrator:  Deutsch Female

-->

# Einführung

Willkommen zurück im eLearning-System *eLab*.

**Für diese Aufgabe haben wir die Kurse 1 - PKeS Kurs A und 1 - PKeS Kurs B erstellt. Ihr habt eine E-Mail von uns erhalten, der ihr eure Kurszugehörigkeit entnehmen könnt.**

--{{1}}--
Da ihr euch während der letzten Aufgabe sowohl mit dem System, als auch mit dem Arbeitsablauf vertraut machen konntet, wird es in dieser Aufgabe darum gehen, tiefer in die Programmierung eingebetteter Systeme einzudringen. 

--{{2}}--
Ein wesentliches Merkmal eingebetteter Systeme ist, dass sie durch periphere Hardware mit ihrer Umgebung interagieren können. Dafür müssen diese Hardwarekomponenten jedoch in Software repräsentiert und angesprochen werden können. Wie auch bei einem Desktop-Computer, geschieht das durch Treiber.

--{{3}}--
Zur Einführung in die Treiberentwicklung eingebetteter Systeme, wird es in dieser Aufgabe darum gehen, drei 8-Segment-Anzeigen anzusteuern und sie durch ein [Application Programming Interface( API)](https://en.wikipedia.org/wiki/Application_programming_interface) nach außen als ein Display darzustellen.


## Themen und Ziele

--{{1}}--
Die Themen, die durch die Treiberentwicklung am Beispiel des 8-Segment-Displays adressiert werden sollen, umfassen zunächst die Ansteuern von periphären Geräten, im speziellen aber Shift-Register.

--{{2}}--
Darüber hinaus soll durch die Treiberentwicklung ein Verständnis für die abstrahierenden Funktionen von Treibern und ihren Beitrag zur Strukturierung von Programmen dargestellt werden. 

--{{3}}--
Letztlich bietet ein Treiber eine Schnittstelle, die durch weiter Programme genutzt werden kann. Daher wird auch die Thematik einer API angeschnitten werden.

**Themen:**

* Einführung in die Treiberentwicklung für eingebettete Systeme
* Arbeitsweise von Shift-Registern und 8-Segment-Anzeigen


**Ziel(e):**

* Ansteuerung von Shift-Registern
* Kapselung gerätespezifischer Informationen in abstrahierenden Funktionen (Treiberentwicklung)
* Implementierung entsprechend einer gegebenen API

## Weitere Informationen

--{{1}}--
Wie immer möchten wir euch weitere Hintergrundinformationen um das Thema Treiber und Treiberentwicklung geben.

--{{2}}--
Da es, je nach Hardware und benötigter Performanz, auch nötig sein kann Assembler zu programmieren, haben wir zusätzlich Links zur Thematik der Assemblerprogrammierung hinzugefügt.

**Treiber und Treiberentwicklung:**

* [Treiber](https://en.wikipedia.org/wiki/Device_driver)
* [Buch zur Treiberentwicklung unter Linux](http://free-electrons.com/doc/books/ldd3.pdf)
* [Application Programming Interface (API)](https://en.wikipedia.org/wiki/Application_programming_interface)

**Assembler**
* [Einführung in Assembler](https://www.tutorialspoint.com/assembly_programming/assembly_introduction.htm)
* [YouTube-Tutorial zur Assemblerprogrammierung](https://www.youtube.com/watch?v=ViNnfoE56V8)
* [AVR GCC Inline Assembler Cookbook](http://www.nongnu.org/avr-libc/user-manual/inline_asm.html)


**PKeS**
* [Schaltbelegungsplan](https://github.com/liaScript/PKeS0/blob/master/materials/robubot_stud.pdf?raw=true)
* 8-Segmente Anzeige:
  * [Wikipedia](https://en.wikipedia.org/wiki/Seven-segment_display)
  * [Datenblatt](http://www.kingbrightusa.com/images/catalog/SPEC/SA52-11SRWA.pdf)
* [Shift-Register](https://www.sparkfun.com/datasheets/IC/SN74HC595.pdf)
* [Arduinoview](https://github.com/fesselk/Arduinoview/blob/master/doc/Documetation.md)
* [Datenblatt des AVR ATmega32U4](http://www.atmel.com/Images/Atmel-7766-8-bit-AVR-ATmega16U4-32U4_Datasheet.pdf)

# Aufgabe 1

--{{1}}--
In der *ersten* praktischen Aufgabe sollt ihr einen Treiber für das Display unserer Roboterplattform implementieren. Dazu haben wir euch dieses mal lediglich die `Display.h`-Datei vorgegeben, in der einige Funktionen deklariert sind. Es gilt in dieser Aufgabe diese [Application Programming Interface (API)](https://en.wikipedia.org/wiki/Application_programming_interface) zu implementieren.

--{{2}}--
Wie schon durch Aufgabe *null* bekannt, wird es euch helfen, die Aufgabe in verschiedenen Teilaufgaben zu bearbeiten. 

--{{3}}--
In der ersten Teilaufgabe werdet ihr zunächst die Kommunikation zwischen dem Mikrocontroller und dem Display implementieren.

--{{4}}--
In der zweiten Teilaufgabe werdet ihr die Funktionalität des Treibers vervollständigen, sodass ihr sowohl Gleitkommazahlen, als auch Ganzzahlen auf dem Display darstellen könnt.

--{{5}}--
Zuletzt werdet ihr den implementierten Treiber nutzen, um vorgegebene Zahlenwerte darzustellen.

**Hinweis:**

**Verwendet bei der Bearbeitung der Aufgabe keine Funktionen aus der Arduino-Bibliothek. Lediglich die Funktionen der `Serial`-Klasse können zur Ansteuerung der seriellen Schnittstelle genutzt werden.**

**Teilaufgaben:**

* *1.1:* Implementiert die Grundfunktionalität zum Ansteuern des 8-Segment-Displays.
* *1.2:* Implementiert die verbliebenen Funktionen der vorgegebenen API zur einfachen Darstellung von Ganzzahlen und Gleitkommazahlen.
* *1.3:* Implementiert die Anbindung zum Arduinoview-Interface.


## Aufgabe 1.1 

--{{1}}--
In dieser Teilaufgabe sollt ihr zunächst den Fluss der Daten vom Mikrocontroller zu den einzelnen 8-Segment-Anzeigen verstehen, bevor ihr die Grundfunktionalität der Ansteuerung implementiert.

--{{2}}--
Wie bereits erwähnt, besteht das Display aus drei eigenständigen 8-Segment-Anzeigen.

--{{3}}--
Um deren korrekte Ansteuerung zu verstehen, solltet ihr den Datenfluss zwischen dem Mikrocontroller und den 8-Segement-Anzeigen verstehen.

--{{4}}--
Eine zentrale Rolle im Datenfluss nehmen die [Shift-Register](https://www.sparkfun.com/datasheets/IC/SN74HC595.pdf) ein. Daher solltet ihr diese eingehend studieren.

--{{5}}--
Nachdem nun die theoretischen Fragen zu den von dieser Aufgabe betroffenen Bauteilen geklärt sein sollten, können wir mit der Implementierung beginnen. 
Wie schon angedeutet, soll die in `Display.h` deklariert API implementiert werden. Dazu solltet ihr eine Datei `Display.cpp` anlegen. Um euren Quellcode entsprechend euren Vorstellungen zu strukturieren, könnt ihr natürlich jederzeit weitere Dateien anlegen.

--{{6}}--
Zunächst soll die Funktion `initDisplay()` implementiert werden. 

--{{7}}--
Zuletzt soll eine grundlegende Funktionalität (`writeToDisplay(uint8_t data[3])`) zum Ausgeben von drei Byte implementiert werden. 

**Ziel:**

Das Ziel in dieser Aufgabe ist es, den Datenfluss, der zur Ansteuerung des Displays notwendig ist, zu verstehen und den Mikrocontroller entsprechend zu konfigurieren. Am Ende der Teilaufgabe sollte es euch dadurch möglich sein, einen von euch gewählten *default*-Wert auf dem Display auszugeben.


**Teilschritte:**

1. Verständnis des Datenflusses. 


2. Implementierung der Funktion `initDisplay()`

3. Implementierung der Funktion `writeToDisplay(uint8_t data[3])`

## Aufgabe 1.2 

--{{1}}--
In der letzten Teilaufgabe haben wir eine grundlegende Funktionalität zur Ausgabe von drei Bytes auf dem Display implementiert. In dieser Teilaufgabe soll diese Funktion genutzt werden, um Variablen der Standarddatentypen `int` und `float` auszugeben.

--{{2}}--
Dazu soll die Funktion `writeValueToDisplay(int value)`, sowie ihre Überladung mit dem `float`-Datentypen, implementiert werden. Bei beiden Funktionen muss jedoch der darstellbare Wertebereich beachtet werden! Versucht so viel Stellen wie möglich auf dem Display anzuzeigen. Was passiert/sollte passieren, wenn wir den Funktionen einen nicht-darstellbaren Wert übergeben?

--{{3}}--
Darüber hinaus muss eine Transformation von der mikrocontroller-internen Wertedarstellung zur Bitrepresentation auf dem Display implementiert werden. Dafür kann es hilfreich sein, eine (mehr oder weniger) standardisierten [Darstellung von Zeichen für 7-Segment-Anzeigen](https://en.wikipedia.org/wiki/Seven-segment_display) als Zwischenrepräsentation zu implementieren. Ausgehend von dieser kann eine gerätespezifische Konvertierung geschehen. Welche Vorteil hätte dies für zukünftige Projekte?


**Ziel:**

Vervollständigt die Implementierung der API in dem ihr die Funktionen `writeValueToDisplay(int value)` und `writeValueToDisplay(float value)` zum Anzeigen von Ganz- und Gleitkommazahlen implementiert.

**Teilschritte:**

1. Implementiert eine Konvertierung von `int` zur Bitrepräsentation der Ganzzahl entsprechend der Ansteuerung des Displays

2. Implementiert eine Konvertierung von `float` zur Bitrepräsentation der Gleitkommazahl entsprechend der Ansteuerung des Displays.


## Aufgabe 1.3

--{{1}}--
Durch die Teilaufgabe 1.2 sollte die Implementierung des Treibers für unser Display abgeschlossen sein. Nun können wir ihn zur Darstellung von Zahlen testen. Daher soll in dieser Aufgabe das Arudinoview-Interface genutzt werden, um Zahlen zu generieren, die ihr dann über euren Treiber auf dem Display darstellt.

--{{2}}--
Wir haben euch bereits einen Slider vorgegeben, durch den ihr Werte in dem Bereich von -200 bis 1000 generieren könnt. Diese werden in der Callback-Funktion `CallbackSL` als [null-terminierter String](https://www.tutorialspoint.com/cprogramming/c_strings.htm) übergeben. 

--{{3}}--
Da wir allerdings auch unsere Ganzzahl-Implementierung der API testen möchten, solltet ihr in einem ersten Schritt dem Arduinoview-Interface noch eine CheckBox hinzufügen. Durch die soll es möglich sein, zwischen der `int` und der `float`-Darstellung dynamisch zu wechseln.

--{{4}}--
In dem finalen, zweiten Schritt könnt ihr die durch die Funktion `CallbackSL` übermittelten Werte in `int` bzw. `float`-Datentypen konvertieren und sie mittels den Funktionen `writeValueToDisplay(int value)` und `writeValueToDisplay(float value)` darstellen.

--{{5}}--
Zusätzlich kann es hilfreich sein dem Interface noch ein Text-Input zu direkten Übergabe von Werten hinzuzufügen. Alternativ könnt ihr natürlich auch Werte von der seriellen Schnittstelle einlesen.

**Ziel:**

Erweitert das vorgegebene Arduinoview-Interface um eine CheckBox um die durch den Slider vorgegebenen Werte dynamisch als `int` oder als `float` auf dem Display auszugeben.

**Teilschritte:**

1. Fügt eine CheckBox zum Wechseln zwischen `float` und `int`-Darstellung
2. Zeigt die Ganz- und Gleitkommazahlen, die durch den Slider vorgegeben werden, auf dem Display an.

# Quizze

--{{1}}--
Wie auch in der letzten Aufgabe, haben wir noch ein paar kurze Fragen an euch, die ihr in Vorbereitung der Abgabe der Aufgabe bei den Tutoren klären solltet.

## C/C++

**Welche der folgenden Funktionen könnten in einem hypothetischen C Programm parallel zu der Funktion `void fun1(int a)` definiert sein?**

[[ ]] `int fun1(int a)`
[[ ]] `void fun1(float a)`
[[X]] `void fun2(float a)`
[[ ]] `void fun1(void)`

**Welche der folgenden Funktionen könnten in einem hypothetischen C++ Programm parallel zu der Funktion `void fun1(int a)` definiert sein?**

[[ ]] `int fun1(int a)`
[[X]] `void fun1(float a)`
[[X]] `void fun2(float a)`
[[X]] `void fun1(void)`
[[[

Im Gegensatz zu C, können in C++ Funktionen [überladen](https://www.tutorialspoint.com/cplusplus/cpp_overloading.htm) werden. Somit können Funktionen mit verschiedenen Argumenten definiert werden. Der Datentyp des Rückgabewertes kann jedoch nicht überladen werden!

]]]


**Gegeben sei folgende C-Code Zeile:**

``` c
A = A & ~(1<<n);
```

**wobei A ein Controller-Register und n eine integer Zahl zwischen 0 und 7 sei. Welcher Wert steht am Ende in A ...**

[( )] eine 1 am n-ten Bit, alle anderen Werte sind 0
[( )] überall 0
[( )] eine 0 am n-ten Bit, alle anderen Werte sind 1
[(X)] eine 0 am n-ten Bit, alle anderen Werte sind unverändert

## Shift-Register und 8-Segment-Display

**Welche Aussage zu RS- bzw- D-Flip-Flops ist korrekt?**

[(X)] Die Ausgänge können in einen unbestimmten Zustand gelangen.
[( )] RS-Flip-Flops können nur aus NAND-Gattern hergestellt werden.
[( )] D-Flip-Flops sind immer taktgesteuert.

**Warum wird neben dem *Clock*-Eingang noch ein *Latch*-Eingang für das Shift-Register benötigt?**

[(X)] Durch den *Latch*-Eingang können die Daten an die Ausgänge des Shift-Registers angelegt werden.
[( )] Durch den *Latch*-Eingang kann der Inhalt aller Flip-Flops durch ein Steuerungskommando zurückgesetzt werden.
[( )] Der *Latch*-Eingang ist eine alternative zum *Clock*-Eingang.

**Welche Ausgänge des [Shift-Register](https://www.sparkfun.com/datasheets/IC/SN74HC595.pdf) sind nach dem folgenden Timing-Diagramm aktiv? Nehmt an, dass zuvor alle Ausgänge inaktiv(0) waren. (1: aktiv, 0: inaktiv. Reihenfolge: QA, QB, QC, QD, QE, QF, QG, QH)**

![Timin1](https://raw.githubusercontent.com/liaScript/PKeS1/master/materials/timing_1.png)

[( )] (00100001)
[( )] (10010000)
[(X)] (00010000)

![Timin2](https://raw.githubusercontent.com/liaScript/PKeS1/master/materials/timing_2.png)

[( )] (00000000)
[( )] (10100001)
[(X)] (01010000)

![Timin3](https://raw.githubusercontent.com/liaScript/PKeS1/master/materials/timing_3.png)

[( )] (00100000)
[(X)] (00000000)
[( )] (00010000)

 
**Welches Muster wird auf dem Display dargestellt, wenn die drei Shift-Register die Werte `0b01001011`, `0b01001011` und `0b01001011` beinhalten?**

[(X)] 444
[( )] 555
[( )] 666

**Das Datenblatt des verwendeten Shift-Registers SN54HC595 (RICHTIG?) spezifiziert eine maximale Clock-Frequenz für den Betrieb unter 25 Grad Celsius. Welche Aussage gilt für die Konfiguration des Bauteils, wie sie in den Übungen verwendet wird?**

[( )] <36 MHz
[( )] <31 MHz
[(X)] < 6 MHz

**Laut Datenblatt sollte zwischen dem Schreiben auf der Datenleitung SER und dem steigenden Flankenwechsel auf der SRCLK im ungünstigsten Fall eine Zeit von 150 ns vergehen. Wie viele NOP Befehle müssen ausgeführt werden, um diese Verzögerung zu generieren.**

[( )] 1 bei einer Taktrate von 8 MHz
[(X)] 2 
[( )] 4 
[( )] 6

**Welche der folgenden Aussagen gilt NICHT als Merkmal von RISC Controller im Vergleich mit CISC-Systemen:**

[( )] kleinere Befehlssatz
[(X)] komplexere Befehle 
[( )] meist Load/Store Architekturen
[( )] extrem schnell auszuführenden Befehle

## Fragebogen

Nachdem Sie nun bereits die ersten beiden Aufgaben (Übung 0 und Übung 1) bearbeitet haben, möchten wir Sie um eine kurze Einschätzung zur Aufgabe für Übung 1 bitten. 

**Inwiefern stimmen Sie folgenden Aussagen zur Arbeit mit dem Remote Lab zu?**

**Die Aufgabe, die in Übung 1 mit dem RemoteLab zu bearbeiten war, ...**

[(:gar nicht)(:2)(:3)(:4)(:voll und ganz)]
    [                                                  ] war interessant.
    [                                                  ] hat mir Spaß gemacht.
    [                                                  ] hat mich neugierig auf die weitere Beschäftigung mit den Inhalten der LV gemacht.
    [                                                  ] hat mich motiviert, mich mit den Inhalten der LV zu beschäftigen.
    [                                                  ] hat mir gefallen.
	[                                                  ] Solche Aufgaben würde ich gerne öfter bearbeiten.


[(:Stimme gar nicht zu)(:2)(:3)(:4)(:Stimme voll zu)]
    [                                                  ] Inhaltlich war die Aufgabe sehr komplex.
    [                                                  ] Das Problem, welches es zu lösen galt, war sehr schwierig.
    [                                                  ] In der Aufgabe wurden sehr komplizierte Konzepte behandelt.
    [                                                  ] Ich habe sehr viel kognitive Anstrengung investiert, um mit der Komplexität der Inhalte umzugehen.
    [                                                  ] Die Aufgabenstellungen und Erklärungen in der Aufgabe waren schwer verständlich formuliert.
	[                                                  ] Die Aufgabenstellungen und Erklärungen waren für das Lernen ineffektiv.
    [                                                  ] Ich habe sehr viel kognitive Anstrengung investiert, um unklare Aufgabenstellungen und Erklärungen zu verstehen.
    [                                                  ] Diese Aufgabe hat wirklich dazu beigetragen, dass ich die Inhalte, die behandelt wurden, besser verstanden habe.
    [                                                  ] Die Aufgabe hat mich dabei unterstützt, die übergeordneten Problemstellungen zu verstehen.
    [                                                  ] Die Aufgabe hat mein Wissen über die zugrundeliegenden Konzepte verbessert.
    [                                                  ] Ich weiß nun sehr viel besser, wie solche Problemstellungen zu behandeln sind.
	[                                                  ] Ich habe sehr viel kognitive Anstrengung investiert, um die Inhalte besser zu verstehen.


 **Inwieweit stimmen Sie den folgenden Aussagen zur Bedienung des Remote Lab-Systems zu?**

[(:stimme gar nicht zu)(:2)(:3)(:4)(:stimme voll zu)]
    [                                                  ] Das System ist unhandlich zu bedienen.
    [                                                  ] Die Oberfläche des Remote Labs ist hilfreich strukturiert.
    [                                                  ] Es ist einfach die Bedienung des Remote Lab-Systems zu erlernen.
    [                                                  ] Mit dem System zu arbeiten, ist oft frustrierend.
    [                                                  ] Die Arbeitsschritte im Remote Lab sind einprägsam, ich habe keine Schwierigkeiten mich an diese zu erinnern.
	[                                                  ] Die Bedienung des Remote Lab ist intuitiv.
    [                                                  ] Mit dem Remote Lab-System zu arbeiten, erfordert (unabhängig von den konkreten Lernaufgaben) ein hohes Maß an geistiger Anstrengung.
    [                                                  ] Die Interaktion mit dem Remote Lab-System ist klar und verständlich.
    [                                                  ] Das Remote Lab gibt klare und verständliche Rückmeldungen.
    [                                                  ] Die Fehlermeldungen des Remote Labs sind bei der Lösung von Problemen hilfreich.	
    [                                                  ] Es hat mich einige Anstrengung gekostet, zu verstehen, wie das System funktioniert.
    [                                                  ] Ich finde, dass das Remote Lab-System insgesamt einfach zu bedienen ist.
	[                                                  ] Für mich war die Nutzung des Remote Labs selbsterklärend.
    [                                                  ] Das Remote Lab hat zuverlässig funktioniert.
    [                                                  ] Die Roboter haben zuverlässig funktioniert.



          
**Hatten Sie Schwierigkeiten bei der Bearbeitung der Aufgabe? Bitte beschreiben Sie diese kurz:**

[[___ ___ ___ ___]]


**Hätten Sie sich an einer bestimmten Stelle Unterstützung gewünscht? Wenn ja, wobei hätten Sie Unterstützung benötigt?**

[[___ ___ ___ ___]]


**Welche Hilfsmittel (z.B. Webseiten etc.) haben Sie bei der Bearbeitung der Aufgabe genutzt? Bitte geben Sie wenn möglich die URL an!**

[[___ ___ ___ ___]]

**Welche zusätzliche Hilfsmittel für diese Aufgabe sollten bereits im RemoteLab integriert sein?**

[[___ ___ ___ ___]]


**Welcher Gruppe waren Sie bei der Bearbeitung der Aufgabe in Übung 1 zugeordnet?**

[(1)] 1 - PKeS Kurs A
[(2)] 1 - PKeS Kurs B

**Haben Sie die Gruppe während der Bearbeitungszeit gewechselt?**

[(1)] Ja
[(2)] Nein

**Wenn ja, warum? ...**

[[___ ___ ___ ___]]
