# Chicken Coop
-----------

## Discription

## Youtube Video
https://youtu.be/x2WiCnN1jPc

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
## Schematic
https://prnt.sc/rn0r9r

------

## Breadboard Photo
https://prnt.sc/rn0vf0

------
## App Screenshots
### Dashboard
https://prnt.sc/rn0y9k

### Actoren
https://prnt.sc/rn0ys9

### Sensors
https://prnt.sc/rn0z6u

## Code
```c++

#define BLYNK_PRINT Serial

#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>
#include <SPI.h>
#include <Stepper.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <Adafruit_BMP280.h>

#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64 // OLED display height, in pixels
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);


Adafruit_BMP280 bmp;
#define SEALEVELPRESSURE_HPA (1013.25)

const float STEPS_PER_REV = 32;
const float GEAR_RED = 64;
const float STEPS_PER_OUT_REV = STEPS_PER_REV * GEAR_RED;
int StepsRequired;
Stepper steppermotor(STEPS_PER_REV, 33, 32, 15, 14);

int lightpin = 34;
int temp = bmp.readTemperature();
int ledRood = 4;
int ledGroen = 5;
int buzzer = 2;
int ventillator = 18;
int statusmotor = 0;
int status2 = 0;

char auth[] = "26qC7ceJXDg7U9-IRecpDGOXo8vnn4GW";
char ssid[] = "WiFi-2.4-B60C";
char pass[] = "92A15B666A";


void setup()
{
  Serial.begin(9600);

  pinMode(ledRood, OUTPUT);
  pinMode(ledGroen, OUTPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(ventillator, OUTPUT);

  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println(F("SSD1306 allocation failed"));
    for (;;);
  }
  

  if (!bmp.begin(0x77)) {
    Serial.println(F("Could not find a valid BMP280 sensor, check wiring!"));
    while (1) delay(10);
  }

  bmp.setSampling(Adafruit_BMP280::MODE_NORMAL,
                  Adafruit_BMP280::SAMPLING_X2,
                  Adafruit_BMP280::SAMPLING_X16,
                  Adafruit_BMP280::FILTER_X16,
                  Adafruit_BMP280::STANDBY_MS_500);


  Blynk.begin(auth, ssid, pass, "server.wyns.it", 8081);


}

void loop()
{

  display.clearDisplay();
  // Temperatuur
  display.setTextSize(1);
  display.setTextColor(WHITE);
  display.setCursor(0, 0);
  display.println("Temperatuur:");
  display.setTextSize(1);
  display.setTextColor(WHITE);
  display.setCursor(0, 10);
  display.println(bmp.readTemperature());
  // Druk
  display.setTextSize(1);
  display.setTextColor(WHITE);
  display.setCursor(0, 20);
  display.println("Druk:");
  display.setTextSize(1);
  display.setTextColor(WHITE);
  display.setCursor(0, 30);
  display.println(bmp.readPressure() / 100.0F);
  display.display();
  // Licht
  display.setTextSize(1);
  display.setTextColor(WHITE);
  display.setCursor(0, 40);
  display.println("Licht:");
  display.setTextSize(1);
  display.setTextColor(WHITE);
  display.setCursor(0, 50);
  display.println(analogRead(lightpin));

  display.display();

  // SERIAL PRINT
  Serial.print(F("Temperature = "));
  Serial.print(bmp.readTemperature());
  Serial.println(" *C");

  Serial.print(F("Pressure = "));
  Serial.print(bmp.readPressure() / 100.0F);
  Serial.println(" hPa");

  Serial.print("Light = ");
  Serial.print(analogRead(lightpin));
  Serial.println(" lux");


  //BLYNK
  Blynk.run();
  Blynk.virtualWrite(V4, analogRead(lightpin));
  Blynk.virtualWrite(V1, bmp.readTemperature());
  Blynk.virtualWrite(V2, bmp.readPressure() / 100.0F);
  WidgetLED led1(V6); //register to virtual pin 1
  WidgetLED led2(V7);
  WidgetLED led3(V8);
  WidgetLED led4(V9);
  WidgetLED led5(V10);
  WidgetLED led6(V11);


  //PROCES
  if (bmp.readTemperature() < 26 && bmp.readTemperature() > 0) {
    digitalWrite(ledGroen, HIGH);
    digitalWrite(ledRood, LOW);
    digitalWrite(buzzer, LOW);
    digitalWrite(ventillator, LOW);
    led1.off();
    led2.off();
    led3.on();
    led4.off();


  } if (bmp.readTemperature() > 26) {
    digitalWrite(ledGroen, LOW);
    digitalWrite(ledRood, HIGH);
    digitalWrite(buzzer, LOW);
    digitalWrite(ventillator, HIGH);
    led3.off();
    led1.on();
    led2.off();
    led4.on();

  } while (bmp.readTemperature() < 0) {
    digitalWrite(ledRood, HIGH);
    digitalWrite(buzzer, HIGH);
    digitalWrite(ledGroen, LOW);
    led3.off();
    led2.on();
    led4.on();
    delay(500);
    digitalWrite(ledRood, LOW);
    digitalWrite(buzzer, LOW);
    digitalWrite(ledGroen, LOW);
    led3.off();
    led2.off();
    led4.off();
    delay(500);
  }

if(analogRead(lightpin) > 3000 && statusmotor == 0){
  status2 = 2;
  statusmotor = 1;
  delay(100);
  
  } if (analogRead(lightpin) < 3000 && statusmotor == 0){
    status2 = 3;
    statusmotor = 1;
    delay(100);
    }

  
while(statusmotor = 1){
  if (analogRead(lightpin) > 3000 && status2 == 2){
    StepsRequired  =  - STEPS_PER_OUT_REV / 2;
    steppermotor.setSpeed(700);
    steppermotor.step(StepsRequired);
    led5.on();
    led6.off();
    delay(3000);
    status2 = 3;
    return;
    while(1);

  } if (analogRead(lightpin) < 3000 && status2 == 3){
    StepsRequired  = STEPS_PER_OUT_REV / 2;
    steppermotor.setSpeed(700);
    steppermotor.step(StepsRequired);
    led5.off();
    led6.on();
    delay(3000);
    status2 = 2;
    return;
    while(1);

  }
}


}
```
