# Instructions - WiFi thermostat firmware
* You can find a description of the functions of individual firmware at: https://martinius96.github.io/WiFi-termostat/en/spustenie.html
* Each firmware folder contains the esptool.exe tool and a .bat file for automated firmware upload to the ESP8266 / ESP32 microcontroller
* ** It is necessary to change the COM port in the .bat file (set for COM7 in ESP8266 firmware, or COM17 for ESP32 firmware) **
* The current port where the microcontroller is logged in can be found in the Device Manager
* The device is most often marked as CH340 device or CP210X depending on the used USB-UART converter
* After uploading the firmware, the command line window (CLI) will close automatically and the firmware is ready for use
* When uploading to the ESP32 microcontroller, it is often necessary to switch the microcontroller to Upload mode by holding down the BOOT button or setting GND to GPIO0

<p align="center">
  <img src="https://i.imgur.com/M0U6HkC.png" />
</p>

# WiFi thermostat configuration - WiFi Manager
* After successful loading of the binary program - firmware, the thermostat is fully ready for operation.
* If the microcontroller does not have data about the WiFi network stored in the flash memory, it will start transmitting its own SSID - WiFi_TERMOSTAT_AP.
* WiFi network WiFi_TERMOSTAT_AP is open without entering a password
* After connecting to a WiFi network with a smartphone / computer, the WiFi Manager web interface is available at 192.168.4.1
* The interface provides the ability to configure a WiFi thermostat for your home WiFi network.
* In the interface it is possible to enter the name and password of the existing WiFi network to which the WiFi thermostat will connect.
<p align="center">
  <img src="https://i.imgur.com/cJb6DR9.png" />
</p>

# WiFi thermostat activity
* The thermostat function is only started after connecting the ESP microcontroller to your LAN network!
* After assigning an IP address for connectivity on your network.
* After connecting to the home WiFi network, ESP stops broadcasting SSID, switches to STA (Station) mode and already works in thermostat mode
* WiFi network data is stored in the thermostat's flash memory and it is no longer necessary to enter it again when restarting the thermostat, power failure, device restart.
* If the network is not available, ESP will start broadcasting its own SSID: WiFi_TERMOSTAT_AP again and the login information can be re-entered.
* The thermostat is available on its IP address (which it prints every 10 seconds also on the UART), or on the local mDNS -> http: //WiFi-thermostat.local - only mDNS firmware
* The mDNS service works only after connecting to the existing WiFi network (it does not work in AP mode with WiFi Manager).
<p align="center">
  <img src="https://i.imgur.com/f1mF6Fk.png" />
</p>
