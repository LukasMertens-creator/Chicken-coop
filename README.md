# Chicken Coop
-----------

## Discription

### -Hardware-
I have made a chicken coop with a security system of the temperature. There is also a roll system, When it gets light the roll goes up, when it gets dark, the roll goes down. When the temperature is to high the red led wil go on and a fan will turn on. When there is no data line, there will be an alarm that turns on.
### Blynk
There is also an app connected called Blynk. Blynk is the dashboard of my system, there you can see al the values. You can see when there is an error. the led in the app will go on. If the motor is up you can see it.

## Libraries
Title | Include | Link 
------|---------|------
Adafruit_SSD1306 | Adafruit_SSD1306.h | https://github.com/adafruit/Adafruit_SSD1306 
Adafruit-GFX-Library | Adafruit_GFX.h | https://github.com/adafruit/Adafruit-GFX-Library 
Adafruit-BMP280 | Adafruit_BMP280.h | https://github.com/adafruit/Adafruit_BMP280_Library
Blynk | BlynkSimpleEsp32.h | https://github.com/blynkkk/blynk-library/blob/master/src/BlynkSimpleEsp32.h
Wifi | WiFi.h | https://github.com/arduino-libraries/WiFi
WifiClient | WiFiClient.h | https://github.com/esp8266/Arduino/blob/master/libraries/ESP8266WiFi/src/WiFiClient.h
Wire | Wire.h | https://github.com/esp8266/Arduino/tree/master/libraries/Wire
SPI | SPI.h | https://github.com/esp8266/Arduino/tree/master/libraries/SPI 
Stepper | Stepper.h | https://github.com/arduino-libraries/Stepper

## Components

---
Display -- Oled ![oled](https://user-images.githubusercontent.com/61006702/77226007-e6c0e100-6b74-11ea-9dff-47438e23a81a.jpg)

the Oled uses I2C to communicate. The Oled is used for displaying data from the sensors.

Pin | ESP32 
------|------
Vin	| 3.3V
GND	| GND
SCL	| GPIO 22
SDA	| GPIO 21

---

Sensor -- BMP280 ![bmp280](https://user-images.githubusercontent.com/61006702/77226020-225bab00-6b75-11ea-9e40-b6a22e855fad.jpg)

This is an sensor which also works with I2C. This sensor measures the presure and the temperature.

Pin | ESP32 
------|------
Vcc	| 3.3V
GND	| GND
SCL	| GPIO 22
SDA	| GPIO 21

---


Sensor -- Photoresistor ![Photoresistor](https://5.imimg.com/data5/LR/UR/ZM/SELLER-40584703/ldr-photoresistor-photo-light-sensitive-resistor-light-dependent-resistor-250x250.jpg)

This sensor Photo resistors, also known as light dependent resistors (LDR), are light sensitive devices most often used to indicate the presence or absence of light, or to measure the light intensity.

Read more http://www.resistorguide.com/photoresistor/


Pin | ESP32 
------|------
Vcc	| GPIO 34
GND	| GND

---


Actor -- Step Motor ![Step Motor](https://image.allekabels.nl/image/1072345-0/stappenmotor-12vdc-60ma-hoek-7.5-85-stappen-stappenmotor-12vdc-60ma-hoek-7.5-85-stappen.jpg)

A stepper motor, also known as step motor or stepping motor, is a brushless DC electric motor that divides a full rotation into a number of equal steps. I used it for my roll.


Pin | ESP32 
------|------
IN1	| GPIO33
IN2	| GPIO15
IN3	| GPIO32
IN4	| GPIO14
Vcc	| 5V
Gnd	| GND

---


