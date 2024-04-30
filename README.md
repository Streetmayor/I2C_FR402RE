# I2C_FR402RE
Zweite Schulaufgabe im Fach Mikrocontroller

I2C Schnittstelle

Für die Umsetzung unseres Projekts benötigen wir zwei Mikrocontroller, die jeweils über mindestens einen SDA (Serial Data Line) und einen SCL (Serial Clock Line) Anschluss sowie fünf GPIO (General Purpose Input/Output) Pins verfügen. Zu diesem Zweck haben wir uns entschieden, zwei STM32 Nucleo-F401RE Boards einzusetzen.

Zusätzlich benötigen wir folgende Komponenten:
•	Fünf Taster zur Eingabe von Benutzerinteraktionen.
•	Sieben 10kΩ Widerstände zur Implementierung von Pull-Up-Widerständen für die Taster und die I2C Leitungen.
•	Drei LEDs, jeweils eine in den Farben Rot, Gelb und Grün, zur visuellen Rückmeldung.
•	Drei 220Ω Widerstände zur Strombegrenzung für die LEDs.
•	Mindestens ein Breadboard zur einfachen Verbindung der Komponenten.
•	Eine Auswahl an Jumperkabeln, um die Verbindungen zwischen den Komponenten herzustellen.

Aufbau der Komponenten auf dem Breadboard:
Master-Breadboard:
1.	Platzieren Sie alle Taster auf dem Breadboard.
2.	Verbinden Sie jeden der fünf Taster zwischen einen GPIO Pin und Masse.
3.	Fügen Sie einen 10kΩ Widerstand zwischen jeden Taster und den 5V Pin des Boards hinzu, um Pull-Up-Widerstände zu realisieren.
Slave-Breadboard:
1.	Platzieren Sie alle LEDs auf dem Breadboard.
2.	Verbinden Sie jeweils einen 220Ω Widerstand mit jeder LED.
3.	Leiten Sie die Anoden (das längere Bein) der LEDs zu den entsprechenden GPIO Pins.
4.	Verbinden Sie die Kathoden (das kürzere Bein) der LEDs mit Masse.
Die Verbindung zwischen Master und Slave:
Verbinden Sie die SDA und SCL Pins der beiden Boards miteinander, um die I2C-Kommunikation zwischen ihnen zu ermöglichen.

Funktionsweise:
Master:
Die Funktionalität der Taster ist wie folgt definiert: Jeder Taster steuert eine bestimmte LED. Ein Taster aktiviert die rote LED, ein anderer die gelbe und ein weiterer die grüne LED. Ein weiterer Taster schaltet alle LEDs gleichzeitig ein und aus, während der letzte Taster das I2C-Protokoll zwischen Senden und Empfangen umschaltet.
Die Taster im Programm ändern jeweils ein Bit in einem Byte, das über die I2C-Schnittstelle gesendet wird. Wenn der Taster für den Wechsel in den Empfangsmodus gedrückt wird, wird ein spezielles Bit in dem gesendeten Byte auf 1 gesetzt. Der Master schaltet dann in den Empfangsmodus, um die Daten zu lesen, die vom Slave gesendet wurden. Nach dem Auslesen gibt der Master eine Statusmeldung aus, die angibt, welche LEDs eingeschaltet sind. 
Zusätzlich wird bei jedem Tastendruck über UART eine Nachricht an das Programmiergerät gesendet, um den aktuellen Status zu melden und das Debugging zu erleichtern. Darüber hinaus gibt es eine automatische Ausgabe, die alle 5 Sekunden den aktuellen Zustand des Bytes ausgibt, um zu verfolgen, welche LEDs gerade geschaltet sind.

Slave:
Der Slave interpretiert das empfangene Byte und steuert entsprechend die Zustände seiner GPIOs, an denen die LEDs angeschlossen sind. Wenn das Bit für das Senden von Daten vom Master gesendet wird, liest der Slave den Zustand seiner GPIOs aus und sendet diese Informationen zurück an den Master.

I2C-Protokoll:
Byte = 0000 0000
0000 0000 = alle LED´s ausgeschaltet
0000 0001 = Rote LED eingeschaltet
0000 0010 = Gelbe LED eingeschaltet
0000 0100 = Grüne LED eingeschaltet
0000 1000 = Master von Senden auf Empfangen / Slave von Empfangen auf Senden umschalten
Das Protokoll interpretiert jedes Bit separat, um festzustellen, welche LEDs ein- oder ausgeschaltet werden sollen. Das bedeutet, dass bei einem Byte wie zum Beispiel 0000 0011 die rote und die gelbe LED eingeschaltet wären. Da eine LED ausgeschaltet ist, wenn das entsprechende Bit auf 0 steht, ist es nicht erforderlich, ein zusätzliches Bit für die Steuerung aller LEDs ein- oder auszuschalten, sondern lediglich die ersten drei Bits müssen auf 1 oder 0 gesetzt werden, zum Beispiel 0000 0111, um alle LEDs einzuschalten.
Dieses Verfahren ermöglicht eine effiziente Steuerung der LEDs und reduziert die Komplexität des Protokolls, indem es nur die Bits verwendet, die den Zustand der LEDs direkt steuern, ohne zusätzliche Information zu benötigen
