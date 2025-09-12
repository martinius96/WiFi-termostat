# WiFi Termostat - ESP8266 / ESP32

**WiFi Termostat** je inteligentný termostat postavený na platforme Espressif – **ESP8266** a **ESP32**. Projekt poskytuje webové rozhranie a JSON API pre monitorovanie a ovládanie teploty v domácnosti.

📄 Rozšírený popis projektu, schéma zapojenia a dokumentácia: [martinius96.github.io/WiFi-termostat](https://martinius96.github.io/WiFi-termostat/)

---

## Funkcie a vlastnosti
- Mikrokontrolér funguje ako **webserver**, ktorý poskytuje:
  - HTML stránky – informačné a funkcionálne
  - JSON stránku s aktuálnymi dátami
- **JSON stránka** distribuuje informácie o:
  - aktuálnej teplote
  - hysteréze
  - cieľovej teplote
- ESP automaticky riadi **výstupné relé** podľa nastavených parametrov.
- Teplotu meria senzor **Dallas DS18B20** (OneWire, normálne/parazitné zapojenie).
- ESP na **UART** vypíše:
  - pridelenú IP adresu z DHCP
  - stav relé
  - voľnú HEAP pamäť
- Projekt môže fungovať celoročne, aj ako **WiFi teplomer**, ak je odpojený výstup na kotol.

---

## Štruktúra firmvéru

| Názov zložky firmvéru | Funkcie |
|-----------------------|---------|
| `WiFi_TERMOSTAT` | Základný termostat. Automatické riadenie vykurovania. |
| `WiFi_TERMOSTAT_mDNS` | Základný termostat + mDNS záznam pre jednoduché spustenie v LAN. |
| `WiFi_TERMOSTAT_MANUAL_experimental` | Termostat s možnosťou manuálneho ovládania výstupu cez web (režim Auto/Manual). |
| `WiFi_TERMOSTAT_OTA` | Termostat + mDNS + možnosť aktualizácie firmvéru cez **Web OTA Updater**. |

---

## Schéma zapojenia

**ESP8266:**  
![ESP8266 Termostat](https://i.imgur.com/hFl5T8e.png)  

**ESP32:**  
![ESP32 Termostat](https://i.imgur.com/PtMinUm.png)  

**Dôležité:**
- Serial monitor: **115200 baud**
- Údaje o hysteréze a cieľovej teplote sú uložené v softvérovo emulovanej EEPROM.

---

## WiFi konfigurácia termostatu

- ESP vysiela vlastné **SSID `WiFi_TERMOSTAT_AP`**, kým nezíska údaje o domácej WiFi sieti.
- Priraďuje klientom IP adresy v rozsahu: `192.168.4.2 - 192.168.4.254`
- Rozhranie **WiFiManager** na `192.168.4.1` umožňuje nastaviť WiFi meno a heslo.
- Po pripojení do domácej WiFi:
  - ESP prestane vysielať SSID
  - Prepne sa do **STA módu** a termostat začne fungovať.
- Dáta WiFi sa ukladajú do **flash pamäte** a nie je potrebné ich zadávať znova.
- Podpora **mDNS/OTA** umožňuje prístup cez lokálnu doménu: `http://wifi-termostat.local`  

![WiFi AP](https://i.imgur.com/cJb6DR9.png)  
![UART WiFi Manager](https://i.imgur.com/bikirYM.png)  
![WiFi Manager - konfigurácia](https://i.imgur.com/2oizcO6.png)  
![IP adresa a mDNS](https://i.imgur.com/f1mF6Fk.png)

---

## HTML stránky na ESP

| URL | Popis |
|-----|-------|
| `/` | Root stránka s formulárom, stavom relé, aktuálnou teplotou a možnosťou zadania novej cieľovej teploty |
| `/action.html` | Spracovanie formulára, zápis do EEPROM, presmerovanie späť na root |
| `/get_data.json` | JSON výstup s aktuálnymi dátami: teplota, cieľová teplota, hysteréza |


---

## JSON klienti

- Klienti na platformách Arduino, ESP8266, ESP32 dokážu:
  - Pripojiť sa k termostatu
  - Získať dáta zo `/get_data.json`
  - Spracovať a archivovať dáta (MySQL, cloud)
  - Riadiť perifériu (solenoid, ventilátor, notifikácie)
- JSON klient sa pripája každých **15 sekúnd** cez websocket
- MQTT implementácia umožňuje publikovať dáta na **IoT Industries Slovakia broker**
  - Hlavný topic: `termostat`
  - Subtopicy: `hysteresis`, `actual_temp`, `target_temp`
- Podpora **MQTTS** pre šifrované spojenie (ESP8266/ESP32)
- Možnosť prispôsobiť **súkromnému brokeru** s autentizáciou  

![JSON klient](https://i.imgur.com/UEnHDb2.png)
