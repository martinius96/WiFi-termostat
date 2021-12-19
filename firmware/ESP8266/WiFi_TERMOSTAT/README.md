# Inštrukcie - WiFi termostat firmvér
* Funkcionalita každého firmvéru termostatu je opísaná na: https://martinius96.github.io/WiFi-termostat/spustenie.html
* Každý priečinok s firmvérom obsahuje nástroj esptool.exe a .bat súbor pre automatizované nahratie firmvéru do mikrokontroléra (ESP8266 / ESP32)
* **V .bat súbore je nutné vykonať úpravu čísla COM portu, kde sa hlási mikrokontróler (vzorovo nastavený COM7 pre ESP8266 firmvér, COM17 pre ESP32 firmvér)**
* Aktuálny port, kde je prihlásený mikrokontróler nájdete v Správcovi zariadení
* Zariadenie je najčastejšie označené ako CH340 device, alebo CP210X v závislosti od použitého USB-UART prevodníka
* Po nahratí firmvéru sa okno príkazového riadku (CLI) automaticky zatvorí a firmvér je pripravený na použitie
* Pri nahrávaní do mikrokontroléru ESP32 je často nutné prepnúť mikrokontróler do Upload režimu držaním tlačidla BOOT, respektíve privedením GND (pulldown) na GPIO0

<p align="center">
  <img src="https://i.imgur.com/M0U6HkC.png" />
</p>

# Konfigurácia WiFi termostatu - WiFi Manager
* Po úspešnom nahratí binárneho programu - firmvéru je termostat plne pripravený na prevádzku.
* **V prípade, že vo flash pamäti nemá uložené mikrokontróler údaje o WiFi sieti, začne vysielať vlastné SSID - WiFi_TERMOSTAT_AP. **
* WiFi sieť WiFi_TERMOSTAT_AP je otvorená bez potreby zadávania hesla
* Po pripojení smartfónom / počítačom do tejto WiFi siete je na adrese 192.168.4.1 dostupné webové rozhranie WiFi Manager
* Rozhranie poskytuje možnosť konfigurácie WiFi termostatu pre vašu domácu WiFi sieť. 
* V rozhraní je možné zadať meno a heslo existujúcej WiFi siete, na ktorú sa WiFi termostat pripojí.
* Možno zadať heslo k existujúcej WiFi sieti v dosahu (výber kliknutím)
<p align="center">
  <img src="https://i.imgur.com/cJb6DR9.png" />
</p>

# Aktivita WiFi termostatu
* Funkcia termostatu je spustená až po pripojení ESP mikrokontroléru do vašej LAN siete! (bez konektivity termostat NEFUNGUJE) 
* Po pripojení na domácu WiFi sieť prestáva ESP vysielať SSID, prepne sa do STA (Station) módu a funguje už v režime termostatu 
* Údaje o WiFi sieti sú uložené do flash pamäte termostatu a už ich nie je nutné zadávať znova pri opätovnom spustení termostatu, výpadku napájania, reštartu zariadenia. 
* V prípade, že daná sieť nie je dostupná, začne ESP opäť vysielať vlastné SSID: WiFi_TERMOSTAT_AP a opätovne je možné zadať prihlasovacie informácie. 
* Termostat je dostupný na svojej IP adrese (ktorú vypíše každých 10 sekúnd aj na UART), respektíve aj na lokálnom mDNS --> http://WiFi-termostat.local - iba mDNS firmvér
* Služba mDNS funguje až po pripojení k existujúcej WiFi sieti (nefunguje teda pri režime AP s WiFi Managerom). 
<p align="center">
  <img src="https://i.imgur.com/f1mF6Fk.png" />
</p>
