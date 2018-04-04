# Espruino on the D1 mini Pro

The [D1 mini Pro](https://wiki.wemos.cc/products:d1:d1_mini_pro) is a mini wifi board with 16MB flash, external antenna connector and built-in ceramic antenna based on ESP-8266EX.

[Espruino](https://github.com/espruino/Espruino) is a JS interpreter for microcontroller. It's also the name/branding associated various [official boards](http://www.espruino.com/Order).

Although the 8266 isn't officially supported by Espruino, it's currently considered to be stable enough.
There is a very [comprehensive Espruino page for the ESP8266.](https://www.espruino.com/EspruinoESP8266) 

# Flashing
These steps are a summary based on information from the ESP8266 page.
    
1. Download and extract the [latest version of the Espruino firmware](http://www.espruino.com/Download) (1v96 is the latest when this was written)
2. Install [esptool](https://github.com/espressif/esptool) and necessary dependencies (e.g. python + pip)
3. Install the [USB to UART driver](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers) for your OS
4. Plug in your D1 mini Pro. Determine the port identifier or path. (e.g. For my Mac, it's /dev/cu.SLAB_USBtoUART)
5. Run the following diagnostic command. (Change the port identifier appropriately)
    
    
    esptool.py --port /dev/tty.SLAB_USBtoUART --baud 115200 flash_id
    
Expected output:
         
     Connecting........_
     Detecting chip type... ESP8266
     Chip is ESP8266EX
     Features: WiFi
     Uploading stub...
     Running stub...
     Stub running...
     Manufacturer: ef
     Device: 4018
     Detected flash size: 16MB
     Hard resetting via RTS pin...

6. Erase the flash memory

    
    esptool.py --port /dev/tty.SLAB_USBtoUART --baud 115200 erase_flash
    
Expected output:
    
    Connecting........_
    Detecting chip type... ESP8266
    Chip is ESP8266EX
    Features: WiFi
    Uploading stub...
    Running stub...
    Stub running...
    Erasing flash (this may take a while)...
    Chip erase completed successfully in 27.9s
    Hard resetting via RTS pin...
    
7. In the extracted firmware directory, use the following command, which is [discussed here](http://forum.espruino.com/conversations/315978/), to flash the Espruino firmware:
    
    
    esptool.py --port /dev/tty.SLABUSBtoUART --baud 115200 write_flash --flash_freq 80m --flash_mode qio --flash_size 4MB 0x0000 espruino_1v96/espruino_1v96_esp8266_4mb/boot_v1.6.bin 0x1000 espruino_1v96/espruino_1v96_esp8266_4mb/espruino_esp8266_user1.bin 0x3FC000 espruino_1v96/espruino_1v96_esp8266_4mb/esp_init_data_default.bin 0x3FE000 espruino_1v96/espruino_1v96_esp8266_4mb/blank.bin

Expected output:

    Connecting........
    Detecting chip type... ESP8266
    Chip is ESP8266EX
    Features: WiFi
    Uploading stub...
    Running stub...
    Stub running...
    Configuring flash size...
    Flash params set to 0x004f
    Compressed 3856 bytes to 2763...
    Wrote 3856 bytes (2763 compressed) at 0x00000000 in 0.2 seconds (effective 124.2 kbit/s)...
    Hash of data verified.
    Compressed 518516 bytes to 345913...
    Wrote 518516 bytes (345913 compressed) at 0x00001000 in 30.5 seconds (effective 136.0 kbit/s)...
    Hash of data verified.
    Compressed 128 bytes to 75...
    Wrote 128 bytes (75 compressed) at 0x003fc000 in 0.0 seconds (effective 86.9 kbit/s)...
    Hash of data verified.
    Compressed 4096 bytes to 26...
    Wrote 4096 bytes (26 compressed) at 0x003fe000 in 0.0 seconds (effective 4772.5 kbit/s)...
    Hash of data verified.
    
8. Connect to the terminal. in Linux/Mac, you can simly use the following command:


    screen /dev/tty.SLAB_USBtoUART 115200
    
9. Verify that a simple JS expression evaluates correctly. e.g. 1+1    


# Further references & comments

- The onboard LED is accessible via D2. You can toggle it using:


    D2.write(0); // or
    D2.write(1);


- [Using & connecting to Wifi](https://www.espruino.com/ESP8266_WifiUsage)
- [Espruino module & API reference](https://www.espruino.com/Reference)
    - [Wifi module](https://www.espruino.com/Reference#Wifi)
    - [ESP8266 module](https://www.espruino.com/Reference#ESP8266)
- [A sample project which uses Webpack & Babel](https://github.com/andrewwakeling/espruino-webpack-babel-sample) to transpile modern JS features that should work with Espruino
- [Official Espriuno Forum](http://forum.espruino.com/)
- [Wemos Forum - D1 boards](https://forum.wemos.cc/category/2/d1-boards)
- Be aware that you can operate as a Access Point (AP) AND be connected to another AP. (Evaluate `Wifi.getStatus()` in the terminal to see what's currently enabled.)

# Feedback

Please raise any concerns or improvements by raising an issue. Thanks!