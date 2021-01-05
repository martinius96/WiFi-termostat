# WiFi Termostat
* Termostat postavený na WiFi platforme Espressif - ESP8266 a ESP32
* Mikrokontróler funguje v režime webservera, na ktorom bežia webové - HTML stránky
* Stránky majú informatívny (výpis), alebo aj funkcionálny charakter (spracovanie dát, zápis do EEPROM)
* Používateľ systému získa informácie o nastavenej teplote, hysteréze a aktuálne nameranej teplote, stave výstupu i voľnej HEAP pamäti
* ESP riadi automaticky na základe navolenej hysterézy a cieľovej teploty výstup - relé ovládané digitálnym vývodom.
* Pre meranie teploty ESP využíva senzor Dallas DS18B20 na OneWire zbernici v parazitnom / normálnom zapojení
* **ESP na UART rozhranie vypíše pridelenú IP adresu z DHCP servera, ale aj informácie o logike a aktuálnom stave výstupu, voľnej HEAP pamäti**
* Projekt môže fungovať celoročne, aj ako WiFi teplomer v prípade, že je odpojený výstup ku riadeniu kotla.

Názov zložky firmvéru | Funkcie firmvéru
------------ | -------------
WiFi_TERMOSTAT  | Projekt termostatu. Možnosť nastavovať a riadiť v automatickom režime vykurovanie v domácnosti
WiFi_TERMOSTAT_mDNS  | Projekt termostatu. Možnosť nastavovať a riadiť v automatickom režime vykurovanie v domácnosti. mDNS záznam pre možnosť spustenia termostatu na lokálnej doméne v rámci LAN siete
WiFi_TERMOSTAT_OTA  | Projekt termostatu. Možnosť nastavovať a riadiť v automatickom režime vykurovanie v domácnosti. mDNS záznam pre možnosť spustenia termostatu na lokálnej doméne v rámci LAN siete s možnosťou aktualizácie firmvéru prostredníctvom Web OTA Updater služby.

# Schéma zapojenia (ESP8266, ESP32)
![Termostat - WiFi - ESP8266](https://i.imgur.com/hFl5T8e.png)
![Termostat - WiFi - ESP32](https://i.imgur.com/PtMinUm.png)
**Dôležité informácie:**
* Serial monitor: 115200 baud/s
* Údaje o hysteréze a referenčnej teplote uložené v softvérovo emulovanej EEPROM pamäti, nakoľko mikrokontróler nemá fyzickú EEPROM pamäť.

# WiFi konfigurácia termostatu
* Termostat vysiela vlastné SSID WiFi_TERMOSTAT_AP, pokým nezíska údaje o existujúcej WiFi sieti, na ktorú sa dokáže pripojiť
* Prideľuje klientom IP adresu v sieti 192.168.4.0 / 24 v rozsahu 192.168.4.2 - 192.168.4.254
* Na adrese 192.168.4.1 poskytuje WiFi termostat rozhranie WiFiManager, kde je možné zadať meno a heslo existujúcej WiFi siete, na ktorú sa pripojí
* Po pripojení na domácu WiFi sieť ESP prestáva vysielať SSID, prepne sa do STA (Station) módu a funguje už v režime termostatu
* Zadané údaje o WiFi sieti sú uložené do flash pamäte termostatu a už ich nie je nutné zadávať znova
* V prípade, že daná sieť nie je dostupná, začne ESP opäť vysielať vlastné SSID --> WiFi_TERMOSTAT_AP
* Ak sa používa firmvér s označením mDNS / OTA, existuje v sieti mDNS príznak pre termostat --> http://wifi-termostat.local
* mDNS musí byť podporovaná zariadením s ktorým sa ku termostatu pripájate a zároveň služba musí byť spustená i na sieti
* Lokálny mDNS príznak funguje nezávisle na IP adrese termostatu, nie je tak nutné hľadať konkrétnu IP, na ktorej je termostat dostupný
* ![WiFi termostat - prístupový bod](https://i.imgur.com/cJb6DR9.png)
* ![WiFi termostat - UART - spustenie WiFi managera](https://i.imgur.com/bikirYM.png)
* ![Konfigurácia WiFi termostatu na domácu WiFi sieť - WiFi Manager](https://i.imgur.com/M3dqgf5.png)
* ![WiFi termostat - Pridelená IP v LAN sieti, termostat funkčný, mDNS záznam](https://i.imgur.com/f1mF6Fk.png)

**HTML stránky bežiace na platforme ESP8266 / ESP32:**
* **/** - root stránka obsahujúca formulár, aktuálny výpis logického výstupu pre relé, teplotu, možnosť zadania nových riadiach teplôt
* **/action.html** - spracúvava hodnoty z formulára, zapisuje ich do EEPROM pamäte, presmeruje používateľa späť na root stránku
* **/get_data.json** - distribuuje dáta o aktuálnej teplote, referenčnej teplote a hysteréze v JSON formáte

**Rozšírená verzia tohto projektu obsahuje:**
* Zdrojový kód (.ino)
* Manuálny režim pre relé (neobmedzená doba, natvrdo ZAP/VYP)
* Watchdog timer
* Dostupné senzory SHT21, SHT31, DHT22, BME280, BMP280 a iné
* Režim chladenia
* PID regulácia teploty pre termostat
* Basic OTA
* Interakcia s Amazon Alexa Echo Dot s možnosťou hlasového ovládania termostatu
* Možnosť Publishu dát na MQTT Broker / cez HTTP / HTTPS request na vzdialený webserver s uložením dát do MySQL databázy

**Pri záujme o kúpu rozšírenej verzie / základnej verzie so zdrojovým kódom .ino:**
* martinius96@gmail.com

**Rozšírené informácie k projektu v článku:**
* http://deadawp.blog.sector.sk/blogclanok/13254/wifi-termostat-esp8266-wifimanager-ota.htm
