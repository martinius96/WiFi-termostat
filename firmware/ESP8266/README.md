# Inštrukcie - ESPTOOL
Pre nahratie binárneho programu do platformy ESP je potrebné použiť nástroj ESPTOOL.EXE (ekvivalent ESPTOOL.PY)
V priečinku každého firmvéru je pripravený automatizovaný - spustiteľný .bat script (vzorovo pre port COM7), ktorý spustí priložený ESPTOOL a vykoná nahratie programu s konkrétnym firmvérom, ktorý sa v danej zložke nachádza. 
Cieľový port je nutné pozmeniť na základe COM portu, na ktorom sa prihlási vaša doska / mikrokontróler. 
Programová implementácia predpokladá, že má doska nahraté základné časti flash pamäte - tabuľka partícii a podobne... 
Binárny program je výlučne aplikáciou termostatu bez ďalších pribalených častí programu. 
Po nahratí programu sa okno programu ESPTOOL automaticky vypne a vykoná softvérový reštart ESP platformy, ktorá následne bootuje nový firmvér a pripravuje WiFi termostat na použitie.
Po úspešnom nahratí binárneho programu - firmvéru je termostat plne pripravený na prevádzku.
V prípade, že v jeho flash pamäti nie sú uložené údaje k existujúcej WiFi sieti (napríklad aj zo sketchu nahratého predtým), začne vysielať vlastné SSID - WiFi_TERMOSTAT_AP. 
Po pripojení smartfónom / počítačom do tejto WiFi siete je na adrese 192.168.4.1 dostupné webové rozhranie WiFi Manager, ktoré poskytuje možnosť konfigurácie WiFi termostatu pre vašu domácu WiFi sieť. 
V rozhraní je možné zadať meno a heslo existujúcej WiFi siete, na ktorú sa WiFi termostat pripojí.
Funkcia termostatu je spustená až po pripojení ESP mikrokontroléru do vašej LAN siete! Po pridelení IP adresy pre konektivitu vo vašej sieti.
Po pripojení na domácu WiFi sieť ESP prestáva vysielať SSID, prepne sa do STA (Station) módu a funguje už v režime termostatu Zadané údaje o WiFi sieti sú uložené do flash pamäte termostatu a už ich nie je nutné zadávať znova pri opätovnom spustení termostatu, výpadku napájania, reštartu zariadenia. 
V prípade, že daná sieť nie je dostupná, začne ESP opäť vysielať vlastné SSID: WiFi_TERMOSTAT_AP a opätovne je možné zadať prihlasovacie informácie. 
Termostat je dostupný na svojej IP adrese (ktorú vypíše každých 10 sekúnd aj na UART), respektíve aj na lokálnom mDNS --> http://WiFi-termostat.local. 
Služba mDNS funguje až po pripojení k existujúcej WiFi sieti. 
V momente, kedy termostat vysiela vlastné SSID, nie je možné na zariadenie pristúpiť cez doménové meno. 
Program funguje na základe schémy zapojenia projektu a logiky z popisu.
