# Beveiliginssysteem-WiFi
Projectomschrijving.

Ik ga 3 verschillende microcontrolers gebruiken die met wifi kunnen verbinden. Men kan via de app blink het gehele systeem aan zetten.
Ik ga gebruik maken van de RFID, een servo motor, buzzer, oled, led's, BH1750. 

Werking:
Het is de bedoeling dat de servomotor de gesloten deur voorsteld. Deze kan alleen opengaan via de RFID. Ook zal er een beveiliging zijn tegen vuur, dit word met de temperatuursensor gedaan. Als alles inorde is brand er een groene led, als e temperatuur boven de 25 graden komt gaat de buzzer afgaan en een rode led branden en op het oled/blink een melding geven dat er vuur is ontstaan.
De esp word gebruikt voor de Oled, Led's, Blynk. De eqsp D1 mini word gebruikt voor de buzzer die je dan ergens kan ophangen. de arduino nano iot worde gebruikt voor de servomotor en de RFID
