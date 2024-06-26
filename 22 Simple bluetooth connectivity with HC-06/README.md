191. [1218a1] [Introduction to the HC-06](#191)
192. [1218a2] [HC-06 Wiring](#192)
193. [1218a3] [HC-06 Pairing](#193)
194. [1218b] [HC-06 Reading sensor data](#194)
195. [1218c1] [HC-06 with SoftwareSerial](#195)

---

### 191. [1218a1] Introduction to the HC-06<a id="191"></a>

we are sending data from laptop to arduino
laptop is transmitter and arduino become receiver

### 192. [1218a2] HC-06 Wiring<a id="192"></a>

<img src="assets/images/1218ab+-+Bluetooth+with+the+HC06.png" width="700">

<img src="assets/images/1.png" width="700">

#### NOTE: Unplug bluetooth when uploading sketch

<img src="assets/images/2.png" width="700">

#### type "H" and enter on serial monitor to switch on LED

<img src="assets/images/3.png" width="700">

### 193. [1218a3] HC-06 Pairing<a id="193"></a>

<img src="assets/images/4.png" width="700">

#### Laptop bluetooth connectivity

<img src="assets/images/5.png" width="700">

<img src="assets/images/6.png" width="700">

<img src="assets/images/7.png" width="700">

#### select port

<img src="assets/images/8.png" width="700">

#### type "H" and enter on serial monitor to switch on LED

<img src="assets/images/9.png" width="700">

```ino
/*  1218c - Bluetooth connection demo with the HC-06
 *
 * This sketch shows you how to control an LED from the serial monitor and to
 * display readings from an analog sensor (photoresistor) using a wireless serial
 * connection with the HC-06 module.
 *
 * The HC-06 is using Software Serial implemented on pins 10 and 11.
 *
 * Once you have the connections completed, with the HC-06 not powered, upload
 * this sketch via the USB cable. Then disconnect the USB cable, and power up the
 * HC-06. Connect the HC-06 to your computer as you do with any Bluetooth device.
 * The pairing key is "1234".
 *
 * In the Arduino IDE, select the HC-06 from the list of available devices. Then,
 * start the serial monitor. Ensure that there is no line ending in the monitor.
 * In the input field, type "H" to turn the LED on, and press Enter. The LED should
 * turn on. Send any other character to turn off the LED.
 *
 * The Arduino will send regular updates with the value of the photoresistor.
 *
 * This sketch was written by Peter Dalmaris for Arduino Step by Step.
 *
 * Components
 * ----------
 *  - Arduino Uno
 *  - 1 x 330 Ohm resistor for the LED
 *  - 1 x LED
 *  - 1 x 10 kOhm resistor for the photoresistor
 *  - 1 x photoresistor
 *  - 1 x HC-06 Bluetooth module
 *  - An external power supply for the Arduino
 *  - Breadboard
 *  - Jumper wires
 *
 *  Libraries
 *  ---------
 *  - SoftwareSerial
 *
 * Connections
 * -----------
*  Arduino Uno and HC-06:
 *
 *  Arduino Uno   |     HC-06
 *  ------------------------------
 *        5V      |       VCC
 *        GND     |       GND
 *        11      |       RXD
 *        10      |       TXD
 *
 * Coneect the 330 Ohm resistor in series with the LED.
 *
 * Connect the anode of the LED to Arduino digital pin 8, and
 * its cathode to GND.
 *
 * Connect the photoresistor and the 10 kOhm resistor in series. Connect
 * the junction of the photoresistor and resistor to analog pin A0 on the
 * Arduino. Connect GND and 5V to the other two ends of the
 * resistor-photoresistor segment (it does not matter which goes where
 * for this example).
 *
 * You can use the Arduino IDE serial monitor to connect to the
 * Arduino's USB serial port.
 *
 * For the Bluetooth connection, you will have to use a second serial
 * terminal. On Mac OS or Linux, you can use the "screen" utility.
 *
 * Start the terminal and type "screen /dev/cu.HC-06-DevB 9600 -L". The
 * connection will stream text from the Arduino, and you can hit any key
 * send text back to the Arduino. To quit screen, type Ctr-A followed by
 * Ctr-B.
 *
 * On Windows, you can use a utility like Putty.
 *
 *  Created on October 16 2017 by Peter Dalmaris
 *
 */

#include <SoftwareSerial.h>

SoftwareSerial mySerial(10, 11); // RX, TX

char val; // variable to receive data from the serial port
int ledpin = 8; // LED connected to pin 8 (on-board LED)

int sensorpin = 0; //Analog pin

unsigned long lastConnectionTime = 0;
const unsigned long postingInterval = 1000;


void setup() {

  pinMode(ledpin, OUTPUT);  // pin 8 (on-board LED) as OUTPUT

  // set the data rate for the SoftwareSerial port
  mySerial.begin(9600);
  Serial.begin(9600);
  mySerial.println("Hello, world? -via BT-");
  Serial.println("Hello, world? -via USB-");
}

void loop() {

  if( mySerial.available() )       // if data is available to read
  {
    val = mySerial.read();         // read it and store it in 'val'
  }
  if( val == 'H' )               // if 'H' was received
  {
    digitalWrite(ledpin, HIGH);  // turn ON the LED
  } else {
    digitalWrite(ledpin, LOW);   // otherwise turn it OFF
  }
  delay(100);                    // wait 100ms for next reading

  if((millis() - lastConnectionTime > postingInterval)) {
            int ana_A = analogRead(sensorpin);
            mySerial.print("Sensor reading: ");
            mySerial.println(ana_A);
            Serial.print("Sensor reading: ");
            Serial.println(ana_A);
            lastConnectionTime = millis();
  }
}
```

Note-

- we can use the same serial monitor from laptop to send character to arduino, to make LED turn on
- The bluetooth default password is "1234"
- We are inputing character from serial monitor, not like from sensor, push button

### 194. [1218b] HC-06 Reading sensor data<a id="194"></a>

We are now sending data from arduino to laptop serial monitor
arduino acting as transmitter as well as receiver

<img src="assets/images/1218c+-+Bluetooth+with+the+HC-06.png" width="700">

<img src="assets/images/10.png" width="700">

#### Laptop bluetooth connectivity

<img src="assets/images/5.png" width="700">

<img src="assets/images/6.png" width="700">

<img src="assets/images/7.png" width="700">

#### select port

<img src="assets/images/8.png" width="700">

#### type "H" and enter on serial monitor to switch on LED

<img src="assets/images/9.png" width="700">

<img src="assets/images/11.png" width="700">

<img src="assets/images/12.png" width="700">

```ino

/*  1218b - Bluetooth connection demo with the HC-06
 *
 * This sketch shows you how to control an LED from the serial monitor and to
 * display readings from an analog sensor (photoresistor) using a wireless serial
 * connection with the HC-06 module.
 *
 * Once you have the connections completed, with the HC-06 not powered, upload
 * this sketch via the USB cable. Then disconnect the USB cable, and power up the
 * HC-06. Connect the HC-06 to your computer as you do with any Bluetooth device.
 * The pairing key is "1234".
 *
 * In the Arduino IDE, select the HC-06 from the list of available devices. Then,
 * start the serial monitor. Ensure that there is no line ending in the monitor.
 * In the input field, type "H" to turn the LED on, and press Enter. The LED should
 * turn on. Send any other character to turn off the LED.
 *
 * The Arduino will send regular updates with the value of the photoresistor.
 *
 * This sketch was written by Peter Dalmaris for Arduino Step by Step.
 *
 * Components
 * ----------
 *  - Arduino Uno
 *  - 1 x 330 Ohm resistor for the LED
 *  - 1 x LED
 *  - 1 x 10 kOhm resistor for the photoresistor
 *  - 1 x photoresistor
 *  - 1 x HC-06 Bluetooth module
 *  - An external power supply for the Arduino
 *  - Breadboard
 *  - Jumper wires
 *
 *  Libraries
 *  ---------
 *  - None
 *
 * Connections
 * -----------
*  Arduino Uno and HC-06:
 *
 *  Arduino Uno   |     HC-06
 *  ------------------------------
 *        5V      |       VCC
 *        GND     |       GND
 *        1       |       RXD
 *        2       |       TXD
 *
 * Coneect the 330 Ohm resistor in series with the LED.
 *
 * Connect the anode of the LED to Arduino digital pin 8, and
 * its cathode to GND.
 *
 * Connect the photoresistor and the 10 kOhm resistor in series. Connect
 * the junction of the photoresistor and resistor to analog pin A0 on the
 * Arduino. Connect GND and 5V to the other two ends of the
 * resistor-photoresistor segment (it does not matter which goes where
 * for this example).
 *
 *  Created on October 16 2017 by Peter Dalmaris
 *
 */

char val;       // variable to receive data from the serial port
int ledpin = 8; // LED connected to pin 8

int sensorpin = 0; //Analog pin

unsigned long lastConnectionTime = 0;
const unsigned long postingInterval = 1000;


void setup() {

  pinMode(ledpin, OUTPUT);  // pin 8 as OUTPUT
  Serial.begin(9600);       // start serial communication at 9600bps
}

void loop() {

  if( Serial.available() )       // if data is available to read
  {
    val = Serial.read();         // read it and store it in 'val'
  }
  if( val == 'H' )               // if 'H' was received
  {
    digitalWrite(ledpin, HIGH);  // turn ON the LED
  } else {
    digitalWrite(ledpin, LOW);   // otherwise turn it OFF
  }
  delay(100);                    // wait 100ms for next reading

  if((millis() - lastConnectionTime > postingInterval)) {
      int ana_A = analogRead(sensorpin);
      Serial.print("Sensor reading: ");
      Serial.println(ana_A);
      lastConnectionTime = millis();
  }
}
```

### 195. [1218c1] HC-06 with SoftwareSerial<a id="195"></a>

In this sketch we have changed the serial pin 10, 11 from 1,2 to avoid conflict while uploading sketch, now we dont need to unplug bluetooth from arduino board to upload sketch

<img src="assets/images/13.png" width="700">

```ino
/*  1218c - Bluetooth connection demo with the HC-06
 *
 * This sketch shows you how to control an LED from the serial monitor and to
 * display readings from an analog sensor (photoresistor) using a wireless serial
 * connection with the HC-06 module.
 *
 * The HC-06 is using Software Serial implemented on pins 10 and 11.
 *
 * Once you have the connections completed, with the HC-06 not powered, upload
 * this sketch via the USB cable. Then disconnect the USB cable, and power up the
 * HC-06. Connect the HC-06 to your computer as you do with any Bluetooth device.
 * The pairing key is "1234".
 *
 * In the Arduino IDE, select the HC-06 from the list of available devices. Then,
 * start the serial monitor. Ensure that there is no line ending in the monitor.
 * In the input field, type "H" to turn the LED on, and press Enter. The LED should
 * turn on. Send any other character to turn off the LED.
 *
 * The Arduino will send regular updates with the value of the photoresistor.
 *
 * This sketch was written by Peter Dalmaris for Arduino Step by Step.
 *
 * Components
 * ----------
 *  - Arduino Uno
 *  - 1 x 330 Ohm resistor for the LED
 *  - 1 x LED
 *  - 1 x 10 kOhm resistor for the photoresistor
 *  - 1 x photoresistor
 *  - 1 x HC-06 Bluetooth module
 *  - An external power supply for the Arduino
 *  - Breadboard
 *  - Jumper wires
 *
 *  Libraries
 *  ---------
 *  - SoftwareSerial
 *
 * Connections
 * -----------
*  Arduino Uno and HC-06:
 *
 *  Arduino Uno   |     HC-06
 *  ------------------------------
 *        5V      |       VCC
 *        GND     |       GND
 *        11      |       RXD
 *        10      |       TXD
 *
 * Coneect the 330 Ohm resistor in series with the LED.
 *
 * Connect the anode of the LED to Arduino digital pin 8, and
 * its cathode to GND.
 *
 * Connect the photoresistor and the 10 kOhm resistor in series. Connect
 * the junction of the photoresistor and resistor to analog pin A0 on the
 * Arduino. Connect GND and 5V to the other two ends of the
 * resistor-photoresistor segment (it does not matter which goes where
 * for this example).
 *
 * You can use the Arduino IDE serial monitor to connect to the
 * Arduino's USB serial port.
 *
 * For the Bluetooth connection, you will have to use a second serial
 * terminal. On Mac OS or Linux, you can use the "screen" utility.
 *
 * Start the terminal and type "screen /dev/cu.HC-06-DevB 9600 -L". The
 * connection will stream text from the Arduino, and you can hit any key
 * send text back to the Arduino. To quit screen, type Ctr-A followed by
 * Ctr-B.
 *
 * On Windows, you can use a utility like Putty.
 *
 *  Created on October 16 2017 by Peter Dalmaris
 *
 */

#include <SoftwareSerial.h>

SoftwareSerial mySerial(10, 11); // RX, TX

char val; // variable to receive data from the serial port
int ledpin = 8; // LED connected to pin 8 (on-board LED)

int sensorpin = 0; //Analog pin

unsigned long lastConnectionTime = 0;
const unsigned long postingInterval = 1000;


void setup() {

  pinMode(ledpin, OUTPUT);  // pin 8 (on-board LED) as OUTPUT

  // set the data rate for the SoftwareSerial port
  mySerial.begin(9600);
  Serial.begin(9600);
  mySerial.println("Hello, world? -via BT-");
  Serial.println("Hello, world? -via USB-");
}

void loop() {

  if( mySerial.available() )       // if data is available to read
  {
    val = mySerial.read();         // read it and store it in 'val'
  }
  if( val == 'H' )               // if 'H' was received
  {
    digitalWrite(ledpin, HIGH);  // turn ON the LED
  } else {
    digitalWrite(ledpin, LOW);   // otherwise turn it OFF
  }
  delay(100);                    // wait 100ms for next reading

  if((millis() - lastConnectionTime > postingInterval)) {
            int ana_A = analogRead(sensorpin);
            mySerial.print("Sensor reading: ");
            mySerial.println(ana_A);
            Serial.print("Sensor reading: ");
            Serial.println(ana_A);
            lastConnectionTime = millis();
  }
}
```
