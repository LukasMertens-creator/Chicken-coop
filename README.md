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
Display | Oled | ![oled](https://user-images.githubusercontent.com/61006702/77226007-e6c0e100-6b74-11ea-9dff-47438e23a81a.jpg)

Because the OLED display uses I2C communication protocol, wiring is very simple. You can use the following table as a reference.

Pin | ESP32 
------|------
Vin	| 3.3V
GND	| GND
SCL	| GPIO 22
SDA	| GPIO 21

---



