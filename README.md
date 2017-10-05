# ESP8266 Probe Sniffer

## To flash firmware using binaries and esptool
1) Connect a Micro-USB cable to the conference badge
2) Clone this repository on a Ubuntu/Debian Linux machine
3) Go to the `firmware` directory of this repo.
4) Install esptool by running `sudo apt-get install esptool`
5) Run the following commands:

```
sudo esptool -vv -cd nodemcu -cb 115200 -cp "/dev/ttyUSB0" -ca 0x00300000 -cf spiffs.bin
sudo esptool -vv -cd nodemcu -cb 115200 -cp "/dev/ttyUSB0" -cf firmware.bin  
```
If either of these do not work make sure you specify the correct USB device.



# For development with PlatformIO
## Dependencies
* python and pip (required by platformio)
* platformio-core (install with ```sudo pip install platformio```)
* all other dependecies are taken care of by the ```platformio.ini``` file

## To flash this firmware using PlatformIO
1. clone this repository
2. connect ESP8266 board to computer with micro USB cable
3. cd into the cloned repository 
4. run following command to upload the SPIFFs files, ```pio run -t uploadfs```
5. run following command to upload firmware ```pio run -t upload```
6. if either of these do not work try specifying the USB device like so ```pio run -t upload --upload-port /dev/ttyUSB0``` or whatever port your USB device defaulted to.

## Active working log
This repository is heavily derived from the following examples:  
* http://github.com/kalanda/esp8266-sniffer
* https://github.com/squix78/esp8266-oled-ssd1306/ 
* https://github.com/esp8266/Arduino/tree/master/libraries/DNSServer

Currently, the main changes/additions include:  
* Rejection of broadcast probes (considered uninteresting)  
* hexDump function to view captured packets  
* an SSD1306 i2c screen to display SSIDs of captured probe requests
* structures for organizing SSID and MAC address data
* logic for identifying news SSIDs and unique probe requests
* logic for sorting SSIDs by # of unique requests and average RSSIs
* captive portal with logon form  
* clickButton library for debouncing interrupts
* disable buttons after selection of SSID
* turn off channel hopping before setting up as AP
* moved captive portal setup and loop to separate ino file
* captured credentials saved to EEPROM and dumped over serial on reset
* clear EEPROM on reset by removing power
* screen timeouts (light sleep after X seconds of inactivity) on SSID list and captive portal screens
* GPIO interrupt to wake up from sleep 
* asynchronous web server for captive portal
* full website stored in SPIFFS

## From original repo
This is only an easy experiment which uses the ESP8266 wifi module to look for near smartphones around you. You can do this very easily with any computer and some software but this is a good way to learn the possibilities of these tiny ESP8266 modules.

**VERY IMPORTANT:** *This code is only for educational purposes. We don’t want to listen for any private communication and we don't do it. All packets that you can listen with this code are public packets without any encryption or secure layer on it, continuously broadcasted to the air by smartphones. Please, check which country's laws applies to you before use this code.*

## Introduction

Some time ago I saw this [video](https://youtu.be/DbqkBAjId_U?t=405) of *Chema Alonso* about how insecure are our mobile phones. He explains, among other things, that your phone is constantly searching for all WiFi networks which you already connect  in the past (unless you did remove as "saved"), saying to anyone who is listening for those public packets where you have been before, and of course, your unique device MAC address.

Those public packets are named as "probe requests" and are used by smartphones to connect faster to wifi networks than if it waits for the network send a Beacon frame to announce the SSID.

This program just listen for those "probe requests" and prints to serial port the information. For now only shows the RSSI (bigger values are near devices), the MAC address of the device and the SSID (if available) of the wifi network which is looking for. Something like that:

## Useful links

- [Bins that track mobiles banned by City of London Corporation](http://www.telegraph.co.uk/technology/news/10237811/Bins-that-track-mobiles-banned-by-City-of-London-Corporation.html)
- [Where’ve you been? Your smartphone’s Wi-Fi is telling everyone](http://arstechnica.com/information-technology/2014/11/where-have-you-been-your-smartphones-wi-fi-is-telling-everyone/)
- [Passive wifi tracking](http://edwardkeeble.com/2014/02/passive-wifi-tracking/)
- [IEEE 802.11 frame format](http://www.studioreti.it/slide/802-11-Frame_E_C.pdf)
- [esp8266 forum](http://www.esp8266.com/viewtopic.php?f=6&t=1589)
