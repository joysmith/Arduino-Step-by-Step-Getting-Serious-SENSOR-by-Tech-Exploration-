218. [1310a] [Introduction](#218)
219. [1310b] [Receiver and transmitter pins and wiring](#219)
220. [1310c] [Receiver and transmitter sketches](#220)
221. [1310d] [Demo](#221)

---

### 218. [1310a] Introduction<a id="218"></a>

<img src="assets/images/1.png" width="700">

### 219. [1310b] Receiver and transmitter pins and wiring<a id="219"></a>

<img src="assets/images/2.png" width="700">

<img src="assets/images/1310a+-+RF422+receiver.png" width="700">

<img src="assets/images/1310b+-+RF422+transmitter.png" width="700">

### 220. [1310c] Receiver and transmitter sketches<a id="220"></a>

- Oprn arduino Go to sketch--> include library--> manage libraries--> search: radio head--> install
- RadioHead Packet Radio library for embedded microprocessors [click me](https://www.airspayce.com/mikem/arduino/RadioHead/)

#### Transmitter code

```ino
/*  1310b - RF422 transmitter
 *
 * This sketch will transmit the "hello" message to the receiver, using
 * a low-cost 422Mhz radio module.
 *
 *
 *
 * This sketch was adapted by Peter Dalmaris for Arduino Step by Step from the
 * original that ships with the Radiohead library.
 *
 * Components
 * ----------
 *  - Arduino Uno
 *  - 433MHz transmitter XY-FST
 *  - Jumper wires
 *  - 1 x LED
 *  - 1 x 330 Ohm resistor
 *
 *  Libraries
 *  ---------
 *  - VirtualWire
 *
 * Connections
 * -----------
 * Arduino Uno and XY-FST:
 *
 *  Arduino Uno   |      XY-FST
 *  ------------------------------
 *        5V      |       VCC
 *        GND     |       GND
 *        12      |       RX
 *
 *  For documentation, please see http://www.airspayce.com/mikem/arduino/RadioHead/index.html
 *
 *  Created on October 20 2017 by Peter Dalmaris, Tech Explorations, txplore.com
 *
 */
// ask_transmitter.pde
// -*- mode: C++ -*-
// Simple example of how to use RadioHead to transmit messages
// with a simple ASK transmitter in a very simple way.
// Implements a simplex (one-way) transmitter with an TX-C1 module
#include <RH_ASK.h>
#include <SPI.h> // Not actualy used but needed to compile

const int ledPin = 7;
const int tx_pin = 12;

// Create the transmitter object.
// Even though this example ciruit only includes a transmitter, we still
// need to provide a pin allocation for the receiver. In the example,
// I allocate pin 12 to the receiver and transmitter in order to not
// waste an Arduino pin that will not actually be used. If you decide
// to attach a receiver, change the receiver pin to the actual pin used
// by the receiver.
// The constructor parameters are:
// speed, rxPin, txPin, pttPin, and pttInverted (see documentation for
// description of pin roles).
RH_ASK driver(2000, 12, tx_pin, 10);

void setup()
{
    Serial.begin(9600);  // Debugging only

    if (!driver.init())
         Serial.println("init failed");

    pinMode(ledPin, OUTPUT);

    // set initial LED state
    digitalWrite(ledPin, LOW);

    Serial.println("Transmitter started");
}

void loop()
{
    const char *msg = "hello";
    Serial.println("t");
    digitalWrite(ledPin, HIGH); // Flash a light to show transmitting
    driver.send((uint8_t *)msg, strlen(msg));
    driver.waitPacketSent();
    digitalWrite(ledPin, LOW);
    Serial.println("x");
    delay(500);
}
```

#### Receiver code

```ino
/*  1310a - RF422 receiver
 *
 * This sketch will receive the "hello" message from the transmitter, using
 * a low-cost 422Mhz radio module.
 *
 *
 * This sketch was adapted by Peter Dalmaris for Arduino Step by Step from the
 * original that ships with the Radiohead library.
 *
 * Components
 * ----------
 *  - Arduino Uno
 *  - 433MHz receiver XY-MK-5V
 *  - Jumper wires
 *  - 1 x LED
 *  - 1 x 330 Ohm resistor
 *
 *  Libraries
 *  ---------
 *  - VirtualWire
 *
 * Connections
 * -----------
 * Arduino Uno and XY-MK-5V:
 *
 *  Arduino Uno   |      XY-MK-5V
 *  ------------------------------
 *        5V      |       VCC
 *        GND     |       GND
 *        12      |       RX
 *
 *  For documentation, please see http://www.airspayce.com/mikem/arduino/RadioHead/index.html
 *
 *  Created on October 20 2017 by Peter Dalmaris, Tech Explorations, txplore.com
 *
 */
// ask_receiver.pde
// -*- mode: C++ -*-
// Simple example of how to use RadioHead to receive messages
// with a simple ASK transmitter in a very simple way.
// Implements a simplex (one-way) receiver with an Rx-B1 module

#include <RH_ASK.h>
#include <SPI.h> // Not actualy used but needed to compile

const int ledPin = 7;
const int rx_pin = 12;

// Create the receiver object.
// Even though this example ciruit only includes a receiver, we still
// need to provide a pin allocation for the transmitter. In the example,
// I allocate pin 12 to the receiver and transmitter in order to not
// waste an Arduino pin that will not actually be used. If you decide
// to attach a transmitter, change the receiver pin to the actual pin used
// by the receiver.
// The constructor parameters are:
// speed, rxPin, txPin, pttPin, and pttInverted (see documentation for
// description of pin roles).
RH_ASK driver(2000, rx_pin, 12, 10);

void setup()
{
    Serial.begin(9600);  // Debugging only

    if (!driver.init())
         Serial.println("init failed");

    pinMode(ledPin, OUTPUT);

    // set initial LED state
    digitalWrite(ledPin, LOW);

    Serial.println("Receiver started");
}

void loop()
{
    uint8_t buf[RH_ASK_MAX_MESSAGE_LEN];
    uint8_t buflen = sizeof(buf);

    if (driver.recv(buf, &buflen)) // Non-blocking
    {
      int i;
      digitalWrite(ledPin, true); // Flash a light to show received good message
      // Message with a good checksum received, dump it.
//      driver.printBuffer("Got:", buf, buflen);
      Serial.print("Got: ");

      for (i = 0; i < buflen; i++)
      {
          Serial.print(buf[i], HEX);
          Serial.print(" (");
          Serial.print(char(buf[i]));
          Serial.print(") ");
      }

      Serial.println("");
      delay(100);
      digitalWrite(ledPin, false);
    }

}
```

### 221. [1310d] Demo<a id="221"></a>

<img src="assets/images/3.png" width="700">

<img src="assets/images/4.png" width="700">
