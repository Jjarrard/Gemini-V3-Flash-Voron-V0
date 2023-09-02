# Gemini-V3-Flash-Voron-V0

Voron 0.2 Fly Gemini V3 reflash/refresh

GOAL:
The goal of this document is to refresh the Gemini V3 in a Voron V0 to ‘as shipped’ standard as per 01/09/23, with as few external links as possible. 
It's not perfect, but should get you up and printing again.

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/gemini-v3.png?raw=true)

###1. Format SD card (16-32gb) via Disk Management in Windows

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/format-card.png?raw=true)

###2. Get Raspberry Pi image tool https://www.raspberrypi.com/software/

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/rpi-imager.png?raw=true)

###3. Get FLYOS image from https://mellow.klipper.cn/#/introduction/downloadimg (Sorry I couldn't upload a 5gb img file to git)
(version in screenshot works with Gemini V3 board)
![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/fly-download-area.png?raw=true)

###5. Plug in board via the bottom left USB C port, and open Device Manager and expand Ports (COM & LPT), you should see a new COM port appear

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/com-port.png?raw=true)

###4. Get PuTTY https://www.putty.org/ and set it up with your COM port like so

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/putty-com.png?raw=true)
Give it a few seconds, You’re in


##Add wifi

5.Type nmtui into putty and press enter
Use the arrow keys to navigate and enter for OK

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/nmtui.png?raw=true)

5b. Select Activate a connection
5c. Navigate to your wifi id, press enter, then type your password, and press enter again
Your window should look like so:

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/nmtui-wifi.png?raw=true)

5d. Press esc esc to back out of this menu

5e. Type ip a and press enter
Look for an ip similar to 192.168.1.85, copy this into your browser and you should see Fluidd

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/fluidd.png?raw=true)
Errors shown will be fixed during this document

Ok, let's switch user and connect via wifi. Open a new putty instance, you can close the old window.

6. Input the new details for the board

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/putty-via-ip.png?raw=true)

Once in the console, username ‘fly’ password ‘mellow’

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/login.png?raw=true)

Cd klipper, make clean, make menuconfig 

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/klipper-make-cmds.png?raw=true)

Set the options like so

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/mainboard-makemenu.png?raw=true)

Press q then y to save and quit

Type make -j4
![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/make-j4.png?raw=true)

Remove diagonal jumper before the next step
![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/jumper.png?raw=true)

wget -O gemini-tools_install.sh https://cdn.mellow.klipper.cn/Utils/gemini-tools/gemini-tools_install.sh && sudo bash gemini-tools_install.sh gemini-v3-tools

Type password

If hid-flash error
cd ~/klipper/lib/hidflash && make

Else 

sudo gemini-v3-tools -f ~/klipper/out/klipper.bin


 sudo gemini-v3-tools -h
 sudo gemini-v3-tools -s
 sudo gemini-v3-tools -r


ls /dev/serial/by-id/*

Grab the blue text

/dev/serial/by-id/usb-Klipper_stm32f405xx_56003E001751313333363337-if00

Put it in your printer.cfg
![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/updt-printconf-w-mcu.png?raw=true)

To fix the below, change the path to the virtual directory in mainsail

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/mainsail-vst-location-error.png?raw=true)

Cd mainsail-config
nano mainsail.cfg

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/nano-mainsail.png?raw=true)

Scroll using arrow keys, change virtual sdcard to expected location
CTRL+X to save, press Y, then enter

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/mainsail-vsd-change.png?raw=true)


Do the same with ~/printer-data/mainsail/mainsail.cfg

Do the same with printer.cfg but via fluidd (because easier)

![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/vsd-prntcgf.png?raw=true)


V0 display
Somewhat stolen from https://github.com/VoronDesign/Voron-Hardware/blob/master/V0_Display/Documentation/Setup_and_Flashing_Guide.md to have a version of documentation that won't change

If your display is attached to the printer, turn it onto its side so you can see the underside of the display

1. Install the boot jumper and press the reset button on the back of the display board. This will put the board into DFU mode.

*V0 BOARD IMG*

*IMG OF BACK OF BOARD*

2. Run lsusb from the command prompt
Make sure you see an STM32 in DFU mode listed
Run dfu-util --list from the command prompt and it should say one DFU is available, make a note of the text inside the [xxxx:yyyy]

Run cd ~/klipper from the command line to enter the Klipper directory

Run make menuconfig settings should be:
*IMG*
Pay attention to the PA9/PA10 comm setting, as if you get it wrong it won’t work and show up in blue

Set the "Optional features" to:
*IMG*

Hit Q to Exit and Save
Run make clean to clean up the make environment.

Run make flash FLASH_DEVICE=xxxx:yyyy (using xxxx:yyyy from above)

You may see what appears to be an "error" after flashing your board. (Blue box)
As long as you see the File downloded successfully text (Green box) you are good to proceed.
The error (Red box) seems to be caused by the controller immediately running the uploaded code and no longer appearing as a DFU device. This is not an issue, as long as the board reports a Klipper serial name. If you see an issue with space on the chip, it may be that you still have an item ticked on the optional features step.

*IMG*

Remove the boot jumper and press the reset button on the back of the display.

After completion ls /dev/serial/by-id/* should return a device begining with /dev/serial/by-id/usb-Klipper_stm32f042x6...

Copy this serial port name (/dev/serial/by-id/usb-Klipper_stm32f042x6... )and place it in your [mcu display] section of the display config file.

Your board should now be usable with Klipper. Use the example config file to get started Best option is to copy the config file into the same directory as printer.cfg then add [include V0Display.cfg] to the end of your printer.cfg to include the file.

Camera
Look in crowsnest logs for something like video0 / video1
Change your crowsnest.conf to match the camera that has been detected as a camera


I've uploaded the macros that came on the board I recieved, use the printer cfg included in this repository as an example
UPLOAD PRINTER.CFG
UPLOAD MACROS

API key location in Fluidd for Cura/Octoprint connection
![alt text](https://github.com/Jjarrard/Gemini-V3-Flash-Voron-V0/blob/main/images/api-key.png?raw=true)

