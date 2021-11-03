# ESP32-Restricted-Airspace
Utilizing Adafruit's Airlift firmware on alternate ESP32 hardware


This is what I did to get this to work using an ESP32 DevKitC v4, however this should be applicable to any ESP32:

(edit: added instructions for using a WEMOS D1 Mini ESP32)

Download the latest ESP32 NINA firmware [from Adafruit available here](https://github.com/adafruit/nina-fw/releases/latest) (also rehosted in this repo)

Install esptool if you don't already have it, not going to go into detail, pip3 install esptool

Plug in your ESP32 and then run the following command, replacing \<port> with the port of your ESP32, and \<firmware> with the NINA firmware you downloaded:

`esptool.py --port <port> --baud 115200 write_flash 0 <firmware>`

On Linux/MacOS, \<port> will be something along the lines of /dev/tty.usb001. On Windows, it'll be something like COM3.

Once that uploads successfully to your ESP32, you're ready to start wiring. I'm using the Adafruit Feather RP2040, but **you should be able to use any CircuitPython compatible device**. You can easily redefine the pins in the Python code.

ESP32 pins are given as GPIO numbers, which should be universal across any ESP32 device. 

The only difference to be aware of on the Wemos D1 Mini ESP32 is that while GPIO14 isn't printed on the board, it is accessible via the pin marked "TMS". [See this schematic for full documentation of this board](https://i.imgur.com/lUCYIYf.png)

| ESP32  | Feather RP2040 | Used for                                |
|--------|----------------|-----------------------------------------|
| 5v     | USB            | Powering ESP32 via the other device     |
| GND    | GND            |                                         |
| GPIO18 | SCK            | SPI SCK                                 |
| GPIO23 | MI (MISO)      | SPI MISO                                |
| GPIO14 | MO (MOSI)      | SPI MOSI                                |
| GPIO5  | D10            | SPI CS/SS (Chip Select)                 |
| GPIO33 | D9             | BUSY (input from ESP32)                 |
| EN     | D6             | RST (reset signal, output to the ESP32) |

(**important** do not plug the ESP32 into USB while it is wired, you may damage your devices!)

Now plug your non-ESP32 device into USB and install the latest version of [CircuitPython from here](https://circuitpython.org/downloads)

While you're there, [grab the latest library bundle](https://circuitpython.org/libraries), you'll need it in a moment.

Once it's installed, you should see a flash drive named CIRCUITPYTHON plugged into your computer. Open it up, create a folder named lib, and copy the following from the CircuitPython library bundle to the 'lib' folder: adafruit_bus_device, adafruit_esp32spi, adafruit_requests.mpy

Once you have those, download code.py from this repository to test it all out. Put that file in the root of your CircuitPython device. 

With any luck, you'll see output similar to this:

![image](https://user-images.githubusercontent.com/6644803/139587016-3e966402-bc2f-4dcc-be83-cfb965f39584.png)


