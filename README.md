# WiFi Termostat - ESP8266 / ESP32
* Termostat postavený na WiFi platforme Espressif - ESP8266 a ESP32
* Rozšírený popis k projektu, schéma zapojenia, dokumentácia: https://martinius96.github.io/WiFi-termostat/
* Mikrokontróler funguje v režime webservera, na ktorom bežia webové - HTML stránky a JSON stránka
* Stránky majú informatívny (výpis), alebo aj funkcionálny charakter (spracovanie dát, zápis do EEPROM)
* JSON stránka distribuuje informácie o aktuálne nameranej teplote, hysteréze a cieľovej teplote
* Používateľ systému získa informácie o nastavenej teplote, hysteréze a aktuálne nameranej teplote, stave výstupu i voľnej HEAP pamäti
* ESP riadi automaticky na základe navolenej hysterézy a cieľovej teploty výstup - relé ovládané digitálnym vývodom.
* Pre meranie teploty ESP využíva senzor Dallas DS18B20 na OneWire zbernici v parazitnom / normálnom zapojení
* **ESP na UART rozhranie vypíše pridelenú IP adresu z DHCP servera, ale aj informácie o logike a aktuálnom stave výstupu, voľnej HEAP pamäti**
* Projekt môže fungovať celoročne, aj ako WiFi teplomer v prípade, že je odpojený výstup ku riadeniu kotla.

Názov zložky firmvéru | Funkcie firmvéru
------------ | -------------
WiFi_TERMOSTAT  | Projekt termostatu. Možnosť nastavovať a riadiť v automatickom režime vykurovanie v domácnosti
WiFi_TERMOSTAT_mDNS  | Projekt termostatu. Možnosť nastavovať a riadiť v automatickom režime vykurovanie v domácnosti. mDNS záznam pre možnosť spustenia termostatu na lokálnej doméne v rámci LAN siete
WiFi_TERMOSTAT_MANUAL_experimental  | Projekt termostatu. Možnosť nastavovať a riadiť v automatickom režime vykurovanie v domácnosti. Experimentálna možnosť manuálneho ovládania výstupu SW tlačidlom z webservera. Prepínanie režimov automat / manual.
WiFi_TERMOSTAT_OTA  | Projekt termostatu. Možnosť nastavovať a riadiť v automatickom režime vykurovanie v domácnosti. mDNS záznam pre možnosť spustenia termostatu na lokálnej doméne v rámci LAN siete s možnosťou aktualizácie firmvéru prostredníctvom Web OTA Updater služby.

# Schéma zapojenia (ESP8266, ESP32)
![Termostat - WiFi - ESP8266](https://i.imgur.com/hFl5T8e.png)
![Termostat - WiFi - ESP32](https://i.imgur.com/PtMinUm.png)
**Dôležité informácie:**
* Serial monitor: 115200 baud/s
* Údaje o hysteréze a referenčnej teplote uložené v softvérovo emulovanej EEPROM pamäti, nakoľko mikrokontróler nemá fyzickú EEPROM pamäť.

# WiFi konfigurácia termostatu
* **Mikrokontróler vysiela vlastné SSID WiFi_TERMOSTAT_AP, pokým nezíska údaje o existujúcej WiFi sieti, na ktorú sa dokáže pripojiť**
* Prideľuje klientom IP adresu v sieti 192.168.4.0 / 24 v rozsahu 192.168.4.2 - 192.168.4.254
* Na adrese 192.168.4.1 poskytuje WiFi termostat rozhranie WiFiManager, kde je možné zadať meno a heslo existujúcej WiFi siete, na ktorú sa pripojí
* **Po pripojení na domácu WiFi sieť ESP prestáva vysielať SSID, prepne sa do STA (Station) módu a funguje už aj s logikou termostatu**
* Zadané údaje o WiFi sieti sú uložené do flash pamäte termostatu a už ich nie je nutné zadávať znova
* V prípade, že daná sieť nie je dostupná, začne ESP opäť vysielať vlastné SSID --> WiFi_TERMOSTAT_AP
* Ak sa používa firmvér s označením mDNS, respektíve i OTA, existuje v sieti mDNS príznak pre termostat --> http://wifi-termostat.local
* mDNS musí byť podporovaná zariadením s ktorým sa ku termostatu pripájate a zároveň služba musí byť spustená i na sieti
* Lokálny mDNS príznak funguje nezávisle na IP adrese termostatu, nie je tak nutné hľadať konkrétnu IP, na ktorej je termostat dostupný
* ![WiFi termostat - prístupový bod](https://i.imgur.com/cJb6DR9.png)
* ![WiFi termostat - UART - spustenie WiFi managera](https://i.imgur.com/bikirYM.png)
* ![Konfigurácia WiFi termostatu na domácu WiFi sieť - WiFi Manager](https://i.imgur.com/2oizcO6.png)
* ![WiFi termostat - Pridelená IP v LAN sieti, termostat funkčný, mDNS záznam](https://i.imgur.com/f1mF6Fk.png)

**HTML stránky bežiace na platforme ESP8266 / ESP32:**
* **/** - root stránka obsahujúca formulár, aktuálny výpis logického výstupu pre relé, teplotu, možnosť zadania nových riadiach teplôt
* **/action.html** - spracúvava hodnoty z formulára, zapisuje ich do EEPROM pamäte, presmeruje používateľa späť na root stránku
* **/get_data.json** - distribuuje dáta o aktuálnej teplote, referenčnej teplote a hysteréze v JSON formáte

**Rozšírená verzia tohto projektu obsahuje:**
* Zdrojový kód (.ino)
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
* http://deadawp.blog.sector.sk/blogclanok/13275/wifi-termostat-esp8266-1021-json-clients.htm

# JSON klienti - sťahovanie dát z termostatu
* Programové implementácia pre klientov na platforme Arduino, ESP8266, ESP32, ktorí sa dokážu pripojiť k WiFi termostatu
* Dokážu načítať dáta, ktoré termostat distribuuje - hysteréza, cieľová teplota, nameraná teplota na podstránke /get_data.json
* Dáta z JSON formátu klient deserializuje, vyparsuje pre ďalšie použitie, archiváciu, upload do MySQL databázy, cloud
* Možnosť na základe dát riadiť perifériu (solenoid radiátora, ohrev, ventilátor, notifikácie o teplote pre Android, iOS zariadenia)
* Pripájanie JSON klienta sa realizuje každých 15 sekúnd k termostatu cez websocket
* V rozšírenej implementácii JSON klienta o MQTT sa dáta posielajú na dostupný free MQTT Broker - IoT Industries Slovakia
* Dáta sa Publishujú do hlavného topicu termostat, pričom je každá entita rozdelená subtopicom
* Subtopicy sú: hysteresis, actual_temp, target_temp
* JSON klient má k daným subtopicom prihlásený Subscribe prostredníctvom čítania hlavného topicu termostat/#
* **Tento MQTT Broker je verejný a tak môžu byť dáta zmenené, prepísané, čítané akýkoľvek používateľom služby**
* **Ak do svojho mikrokontroléru nahrá firmvér JSON klienta bez zmeny akýkoľvek iný používateľ, bude vám prepisovať dáta v preddefinovanom topicu**
* Možno prispôsobiť pre váš MQTT broker a systém inteligentnej domácnosti, kde môžete mať dáta z termostatu - Hassio, Domoticz, MQTT Mosquitto, Loxone
* Pre súkromný MQTT broker je možné využiť aj autentizáciu menom a heslom, viz. dokumentácia: https://pubsubclient.knolleary.net/api
* K dispozícii je aj firmvér pre JSON MQTT klientov s označením MQTTS - využívajú šifrované spojenie cez socket s MQTT brokerom
* Tento typ firmvéru je dostupný iba pre ESP8266 a ESP32. Arduino s Ethernetom nepodporuje šifrovanie
![JSON klient - Arduino, ESP8266, ESP32](https://i.imgur.com/UEnHDb2.png)
