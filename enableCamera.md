#### Enable camera
``` bash 
1- Flash: Go to the raspberry Pi Official website
2- Go to the raspberry pi imager page
3- Pick your computer OS (Windoes, MacOS, Ubuntu and even Terminal)
4- Download
5- Open the Downloaded file and accept all the terms
6- Raspberry pi Imager should now be downloaded and available to use 
7- Click on it, and accept that it should make modifications on your computer
8- Insert an SD card using an adapter or by directly inserting it to your computer
9- First:Pick your raaspberry pi devive
10- Second: Pick your OS system (64-bit, 32-bit, legacy 32-bit, etc...). 
    We took the Legacy 32-bit since it comes with the picamera Library
11-Thirdly: pick your SD card storage
12- Flash
---------Enabling the camera--------------
1- Go to the terminal: sudo raspi-config
2-On the Rspberry Pi configuration tool, select interface options
3- select legacy camera and enable it
4- reboot (sudo reboot or using the user interface)
```
