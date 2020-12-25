# WiFi Termostat
* Termostat postavený na platforme Espressif - ESP8266 a ESP32
* Mikrokontróler funguje v režime webservera, kde vizualizuje aktuálna dáta pre používateľa.
* ESP riadi na základe navolenej hysterézy a cieľovej teploty výstup - relé ovládané digitálnym vývodom.
* Pre meranie teploty ESP využíva senzor Dallas DS18B20 na OneWire zbernici v parazitnom / normálnom zapojení
* **ESP na UART vypíše pridelenú IP adresu z DHCP servera, ale aj informácie o logike a aktuálnom stave výstupu, voľnej HEAP pamäti**

Názov zložky firmvéru | Funkcie firmvéru
------------ | -------------
WiFi_Termostat  | Projekt termostatu. Možnosť nastavovať a riadiť v automatickom režime vykurovanie v domácnosti

# Schéma zapojenia (ESP8266, ESP32)
![Termostat - WiFi - ESP8266](https://i.imgur.com/hFl5T8e.png)
**Dôležité informácie:**
* Serial monitor: 115200 baud/s
* Údaje o hysteréze a referenčnej teplote uložené v EEPROM pamäti, ktorá je softvérovo emulovaná vo flash pamäti

**HTML stránky bežiace na Arduine:**
* **/** - root stránka obsahujúca formulár, aktuálny výpis logického výstupu pre relé, teplotu, možnosť zadania nových riadiach teplôt
* **/action.html** - spracúvava hodnoty z formulára, zapisuje ich do EEPROM pamäte, presmeruje používateľa späť na root stránku
* **/get_data.json** - distribuuje dáta o aktuálnej teplote, referenčnej teplote a hysteréze v JSON formáte

**Rozšírená verzia tohto projektu obsahuje:**
* Zdrojový kód (.ino)
* Manuálny režim pre relé (neobmedzená doba, natvrdo ZAP/VYP)
* Watchdog timer
* Dostupné senzory SHT21, SHT31, DHT22, BME280, BMP280 a iné
* Režim chladenia
* Ovládanie a konfigurácia po RS232 / UART nezávisle na Ethernete
* PID regulácia teploty pre termostat
* Basic OTA
* Interakcia s Amazon Alexa Echo Dot s možnosťou hlasového ovládania termostatu
* Možnosť Publishu dát na MQTT Broker / cez HTTP / HTTPS request na vzdialený webserver s uložením dát do MySQL databázy

**Pri záujme o kúpu rozšírenej verzie / základnej verzie so zdrojovým kódom .ino:**
* martinius96@gmail.com

**Rozšírené informácie k projektu v článku:**
* http://deadawp.blog.sector.sk/blogclanok/13254/wifi-termostat-esp8266-wifimanager-ota.htm
