18. [0590a] [Introduction to the HMC5883 compass magnetometer](#18)
19. [0590b] [HMC5883 wiring](#19)
20. [0590c] [HMC5883 sketch](#20)
21. [0590d] [HMC5883 demonstration](#21)

---

### 18. [0590a] Introduction to the HMC5883 compass magnetometer<a id="18"></a>

<img src="assets/images/1.png" width="700">

- Three-Axis Digital Compass IC HMC5883L datasheet [click me](https://www.farnell.com/datasheets/1683374.pdf)

### 19. [0590b] HMC5883 wiring<a id="19"></a>

<img src="assets/images/2.png" width="700">

<img src="assets/images/5.png" width="700">

#### Output

<img src="assets/images/6.png" width="700">

<img src="assets/images/7.png" width="700">

### 20. [0590c] HMC5883 sketch<a id="20"></a>

#### How to calibrate compass

<img src="assets/images/3.png" width="700">

#### use google to convert from degree to radian

<img src="assets/images/4.png" width="700">

- Open arduino IDE, go to sketch--> include library--> manage libraries--> search:adafruit unified sensor by adafruit--> install

- Open arduino IDE, go to sketch--> include library--> manage libraries--> search:adafruit_hmc5883 sensor by adafruit--> install

```ino
/*  HMC5883 magnetometer sensor demo sketch 1
 *
 * This sketch show the usage of the HMC5883 sensor with the
 * Adafruit HMC5883 library.
 *
 * The readings come from any 3 available analog inputs.
 *
 * North is pointed by the X axis silksreen line when heading is 0 degrees.
 *
 * This sketch was adapted from the Adafruit sample for Arduino Step by Step by Peter Dalmaris.
 *
 * Components
 * ----------
 *  - Arduino Uno
 *  - HMC5883 sensor breakout or equivelant
 *
 *  Libraries
 *  ---------
 *  - NONE
 *
 * Connections
 * -----------
 *  Break out    |    Arduino Uno
 *  -----------------------------
 *      VCC      |      5V
 *      GND      |      GND
*       SCL      |      SCL or A5
 *      SDA      |      SDA or A4
 *      DRDY     |      Not connected

 *
 * Other information
 * -----------------
 *  For information on magnetometers: https://en.wikipedia.org/wiki/Magnetometer
 *  HMC5883 datasheet: https://cdn-shop.adafruit.com/datasheets/HMC5883L_3-Axis_Digital_Compass_IC.pdf
 *  HMC5883 product page: https://www.adafruit.com/product/1746
 *
 *
 *  Created on October 8 2016 by Peter Dalmaris
 *
 */

/***************************************************************************
  This is a library example for the HMC5883 magnentometer/compass

  Designed specifically to work with the Adafruit HMC5883 Breakout
  http://www.adafruit.com/products/1746

  *** You will also need to install the Adafruit_Sensor library! ***

  These displays use I2C to communicate, 2 pins are required to interface.

  Adafruit invests time and resources providing this open source code,
  please support Adafruit andopen-source hardware by purchasing products
  from Adafruit!

  Written by Kevin Townsend for Adafruit Industries with some heading example from
  Love Electronics (loveelectronics.co.uk)

 This program is free software: you can redistribute it and/or modify
 it under the terms of the version 3 GNU General Public License as
 published by the Free Software Foundation.

 This program is distributed in the hope that it will be useful,
 but WITHOUT ANY WARRANTY; without even the implied warranty of
 MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 GNU General Public License for more details.

 You should have received a copy of the GNU General Public License
 along with this program.  If not, see <http://www.gnu.org/licenses/>.

 ***************************************************************************/

#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_HMC5883_U.h>

/* Assign a unique ID to this sensor at the same time */
Adafruit_HMC5883_Unified mag = Adafruit_HMC5883_Unified(12345);

void displaySensorDetails(void)
{
  sensor_t sensor;
  mag.getSensor(&sensor);
  Serial.println("------------------------------------");
  Serial.print  ("Sensor:       "); Serial.println(sensor.name);
  Serial.print  ("Driver Ver:   "); Serial.println(sensor.version);
  Serial.print  ("Unique ID:    "); Serial.println(sensor.sensor_id);
  Serial.print  ("Max Value:    "); Serial.print(sensor.max_value); Serial.println(" uT");
  Serial.print  ("Min Value:    "); Serial.print(sensor.min_value); Serial.println(" uT");
  Serial.print  ("Resolution:   "); Serial.print(sensor.resolution); Serial.println(" uT");
  Serial.println("------------------------------------");
  Serial.println("");
  delay(500);
}

void setup(void)
{
  Serial.begin(9600);
  Serial.println("HMC5883 Magnetometer Test"); Serial.println("");

  /* Initialise the sensor */
  if(!mag.begin())
  {
    /* There was a problem detecting the HMC5883 ... check your connections */
    Serial.println("Ooops, no HMC5883 detected ... Check your wiring!");
    while(1);
  }

  /* Display some basic information on this sensor */
  displaySensorDetails();
}

void loop(void)
{
  /* Get a new sensor event */
  sensors_event_t event;
  mag.getEvent(&event);

  /* Display the results (magnetic vector values are in micro-Tesla (uT)) */
  Serial.print("X: "); Serial.print(event.magnetic.x); Serial.print("  ");
  Serial.print("Y: "); Serial.print(event.magnetic.y); Serial.print("  ");
  Serial.print("Z: "); Serial.print(event.magnetic.z); Serial.print("  ");Serial.println("uT");

  // Hold the module so that Z is pointing 'up' and you can measure the heading with x&y
  // Calculate heading when the magnetometer is level, then correct for signs of axis.
  float heading = atan2(event.magnetic.y, event.magnetic.x);

  // Once you have your heading, you must then add your 'Declination Angle', which is the 'Error' of the magnetic field in your location.
  // Find yours here: http://www.magnetic-declination.com/
  // Mine is: -13* 2' W, which is ~13 Degrees, or (which we need) 0.22 radians
  // If you cannot find your Declination, comment out these two lines, your compass will be slightly off.
  float declinationAngle = 0.21;
  heading += declinationAngle;

  // Correct for when signs are reversed.
  if(heading < 0)
    heading += 2*PI;

  // Check for wrap due to addition of declination.
  if(heading > 2*PI)
    heading -= 2*PI;

  // Convert radians to degrees for readability.
  float headingDegrees = heading * 180/M_PI;

  Serial.print("Heading (degrees): "); Serial.println(headingDegrees);

  delay(500);
}
```

### 21. [0590d] HMC5883 demonstration<a id="21"></a>
