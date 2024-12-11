This readme describe the process I've got through in order to unlkock a thinkpad x270 with the BIOS being locked by a supervisor password.

### Setup
A lot of the setup is well described in this link : [https://winraid.level1techs.com/t/guide-flash-bios-with-ch341a-programmer/32948](https://winraid.level1techs.com/t/guide-flash-bios-with-ch341a-programmer/32948)

1. The first step was to buy the CH341A programmer in order to access the memory of the BIOS chip, and modding the programmer in order to get an output of 3.3v from the data line instead of the 5v that it was outputting.

<img src="https://raw.githubusercontent.com/Tictactouc/x270_BIOS_Reset/master/Doc/photos/IMG_3240.jpeg" width="200">

Apparently it was caused by a conception defect prior to the v1.6 of the programmer. 
Also from what I've understood this defect is not a big deal because the current is very very low and it is very unlikely to fry the chip with this defect. 
Anyway, I really didn't want to broke the bios chip and loose the Laptop so I did the mod (also, it was a good way of exercizing my electronics skills).
A good ressource about this mod is well described here : [https://www.youtube.com/watch?v=HwnzzF645hA](https://www.youtube.com/watch?v=HwnzzF645hA)

<img src="https://raw.githubusercontent.com/Tictactouc/x270_BIOS_Reset/master/Doc/photos/IMG_3250.jpeg" width="300">


3. The next step was to download and install the software "AsProgrammer" here : [https://www.onetransistor.eu/2018/11/use-ch341a-with-asprogrammer-on-windows.html](https://www.onetransistor.eu/2018/11/use-ch341a-with-asprogrammer-on-windows.html)
   and the driver of the CH341A programmer that can be found here (the good one is CH341PAR.EXE as we'll effectively write to the chip in the next part of this readme) : [https://www.onetransistor.eu/2017/08/ch341a-mini-programmer-schematic.html
](https://www.onetransistor.eu/2017/08/ch341a-mini-programmer-schematic.html)

5. The last step for completing the setup is to locate the chip (which is a Winbond W25Q128FV) under the black plastic; right next to the CPU and to connect the clip to it (be careful with the orientation).

At this point the setup should look like this :

<img src="https://github.com/Tictactouc/x270_BIOS_Reset/assets/63233669/7479412d-8792-4a1b-9f1e-b559a1c869f9" width="500">

### Dumping and patching the BIOS EEPROM data

1. Using AsProgrammer, I made multiple dump of the data and I compared it to make sure it was not corrupted.

[Photo of the software and the files]

2. Now the real game begin, the dump need to be patched with a special tool found in the Band cap forum (all the credit go to these guys) THIS IS THE MAGICAL LINK WHERE ALL THE WITCHCRAFT HAPPEN : [https://badcaps.net/forum/showthread.php?t=87588](https://badcaps.net/forum/showthread.php?t=87588)
From what I have understood, the tool modify some data region of the firmware, notably the DXE drivers and replace them with a custom one made to exploit some vulnerability to trick the BIOS into thinking it's brand new and erase the password stored in the TPM security chip . (I will try to understand and explain it better when I will update the README).
Anyway, I've ran the tool accordingly to the steps described in the link above and applied the patch. Then I checked that the patch had made modification to the data by comparing the patched bios with the old one using WinMerge

1. After that, using AsProgrammer I've erased the bios and writted the patched one.

### Running the exploit and rewriting the old one

Now that the patched bios have been written to the bios I just followed the step described in the Band Cap forum :
(this is copied from the original post)

2. Boot the machine
3. Press ENTER/F1/etc. to enter BIOS settings
4. Enter any character when asked for Supervisor Password
5. Press enter when it shows Hardware ID
6. Press space bar 2x when asked
7. Turn off machine

And voilà the laptop was unlocked
