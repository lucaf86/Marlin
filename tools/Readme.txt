To flash optiboot on Malyan M150 and free about 3K of flash:


1) disconnect all the cables from the Melzi board, unscrew it and take it apart
2) solder a 6pin header for the ISP, the pads near the lcd connector
3) move the jumper in the middle of the board to select the 5V from usb in order to power the board from the ISP connector using 5V from the programmer
4) take an Arduino uno and flash it with the "Arduino as ISP" example. I don't have any programmer so I opted for this solution instead
5) with the Arduino powered off, connect the ISP interface (5V, GND, MISO, MOSI, CLK, RESET). Arduino reset held to GND with a capacitor of 1uF to avoid autoreset, while the reset pinn of the ISP connector on the Melzi was connected to D10 pin of the Arduino
6) double check each pin connection, I decline any responsibility in case of incorrect connections
7) downloaded avrdude (I used an old version, avrdude version 5.11) and connect the Arduino to the PC, this will power on also the Melzi (if you have switched the jumper), but no led will be on
8) open a cmd line and run avrdude to check that everything is ok (change -P with you com port number):
Code:

    avrdude.exe -v -p atmega1284p -c avrisp -P com10 -b 19200

9) if no error, continue and check your fuses using the command:
Code:

   avrdude.exe -v -p atmega1284p -c avrisp -P com10 -b 19200 -U lfuse:r:-:i -U hfuse:r:-:i -U efuse:r:-:i

the output should be
Code:

    Reading | ################################################## | 100% 0.05s
    avrdude.exe: Device signature = 0x1e9705
    avrdude.exe: safemode: lfuse reads as D6
    avrdude.exe: safemode: hfuse reads as DA
    avrdude.exe: safemode: efuse reads as FD

10) now for safety, make a dump of the current flash, just in case you want to revert back:
Code:

    avrdude.exe -v -p atmega1284p -c avrisp -P com10 -b 19200 -U flash:r:ispM150Dump.bin:r

11) Now change the high fuse and flash optiboot
Code:

    avrdude.exe -v -p atmega1284p -c avrisp -P com10 -b 19200 -u -U hfuse:w:0xDE:m -U flash:w:optiboot_atmega1284p_M150.hex

12) disconnetc the Arduino from the PC, disconnect the ISP, move back the jumper on the Melzi and reconnet all the cables of the printer
13) power on the 3d printer by keeping pressed the button, and connect the usb to flash the firmware, just remember to change the baude rate to 115200 as specified by daniel.snowsurf  https://www.rcgroups.com/forums/showpost.php?p=52367715&postcount=3534
