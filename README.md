# Gemini-V3-Flash-Voron-V0

Voron 0.2 Fly Gemini V3 from scratch

GOAL:
The goal of this document is to refresh the Gemini V3 in a Voron V0 to ‘as shipped’ standard as per 01/09/23, and to have a version locked document, with no links that may change. 

*GEMINI BOARD IMG*

1. Format SD card (16-32gb) via Disk Management in Windows

*FORMAT IMAGE*

2. Get Raspberry Pi image tool https://www.raspberrypi.com/software/

*RASP PI IMAGE TOOL IMG*

3. Get FLYOS image included in github files, but also available from https://mellow.klipper.cn/#/introduction/downloadimg 
(Yes this works with Gemini V3 board)

*FLY DOWNLOAD IMG*

5. Plug in board via the bottom left USB C port, and open Device Manager and expand Ports (COM & LPT), you should see a new COM port appear

*COM PORTS IMG*

4. Get PuTTY https://www.putty.org/ and set it up with your COM port like so

*PUTTY IMG*
Give it a few seconds, You’re in


Add wifi

5.Type nmtui into putty and press enter
Use the arrow keys to navigate and enter for OK

*NETWORK TOOL IMG*

5b. Select Activate a connection
5c. Navigate to your wifi id, press enter, then type your password, and press enter again
Your window should look like so:

*WIFI PART IMG*

5d. Press esc esc to back out of this menu

5e. Type ip a and press enter
Look for an ip similar to 192.168.1.85, copy this into your browser and you should see Fluidd

*FLUID IN BROWSER IMG*

Ok, let's switch user and connect via wifi. Open a new putty instance, you can close the old window.

6. Input the new details for the board

*PUTTY IP IMG*

Once in the console, username ‘fly’ password ‘mellow’

*LOGIN IMAGE*

Cd klipper, make clean, make menuconfig 

*KLIPPER CMDS IMG*

Set the options like so

*FIRMWARE CONFIG IMG*

Press q then y to save and quit

Type make -j4
*MAKEJ4 IMG*

Remove diagonal jumper before the next step

*DIAG JUMPER IMG*

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
*PRINTER CFG IMG*

To fix the below, change the path to the virtual directory in mainsail

*MAINSAIL ERROR IMG*

Cd mainsail-config
nano mainsail.cfg

*MAINSAIL CMDS IMG*

Scroll using arrow keys, change virtual sdcard to expected location
CTRL+X to save, press Y, then enter

*NANO MAINSAIL IMG*


Do the same with ~/printer-data/mainsail/mainsail.cfg

Do the same with printer.cfg but via fluidd (because easier)

*PRINTER VIRTUAL IMG*


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

