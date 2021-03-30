# PSX64 Adapter for C64

![](https://github.com/DL2DW/PSX64_Adapter_for_C64/blob/main/Images/REX9628.jpg)



## Introduction


A few years ago I discovered on the homepage of [Synthetic Dreams](http://www.oursyntheticdreams.com/) the [schematic ](http://www.oursyntheticdreams.com/sites/default/files/downloads/psx64schematics.png)and [firmware ](http://www.oursyntheticdreams.com/sites/default/files/downloads/psx64fw11.hex)for a very interesting joystick adapter. With this adapter you can play a game called [Shredz64](https://www.c64-wiki.de/wiki/Shredz64), which is modeled after the Guitar Hero game known from the Playstation 2. I personally didn’t even find that the interesting thing about it, but rather the fact that you could use it to connect the well-known PSX2 gamepads to the C64 for any other game.

Additionally this interface can score with a few additional features:

- Two firebuttons are supported, which is needed for Shredz64 among others
- The analog stick can be used, if of course only digitally
- Since the PSX controller has 4 firebuttons, the above two firebuttons are again assigned with continuous fire.
- You can program macros. So you can store sequences in the adapter and call them up at the push of a button (with the shoulder buttons).
- And an important feature for me personally was that you can play with the PSX2 controllers, which feel very good in my hand.

The PSX64 can, or could, also be ordered as a ready-made unit from Synthetic Dreams. However as a rather large board. And since the delivery is from abroad, with additional fees. What was more obvious than simply rebuilding this adapter. Especially since the website is no longer maintained for almost 5 years and probably the adapter is no longer sold.

Especially since I have now shrunk the adapter so far that it could also be easily installed directly in a PSX2 controller.

The construction is quite simple and could also be built on a strip grid board or a breadboard.



## The construction


I kept the board as compact as possible, but as generous for easy soldering as possible. The components are quite large, for SMD conditions. So that even a little experienced hobbyist should assemble this board quite easily.



![](https://github.com/DL2DW/PSX64_Adapter_for_C64/blob/main/Images/PSX64_Platine_unbestückt_klein.jpg)



For the assembly, besides the board, the following components are needed:

- 2x MCP41100 – SOIC-8 (U1, U2)
- ATMega8-16AU – TQFP-32 (U3)
- 16MHz crystal – 5032 (Y1)
- 4.7µF tantalum capacitor – Kemet A (C1)
- 2x 22pF capacitor – 0805 (C2, C3)
- 100nF capacitor – 0805 (C4)
- 2x 10k resistor – 0805 (R1, R2)
- 2x 1N4004 diode – SMA (D1, D2)

Actually quite manageable and above all also quite cheap. Except for the two digital potentiometers MCP41100, you can get the parts in any well sorted electronics store. The two MCP41100 are available from Mouser, Digikey and sometimes from eBay.

Additionally you need cables for the joystick connections and the corresponding plugs and sockets. For the connection of the PSX2 gamepad I used a PSX extension. You can get them for 2-3 Euro on eBay and they are much cheaper than the jack alone. Just cut the connector and solder it to the board.

For the side to the computer, I used a 10pin ribbon cable, from which I cut off one wire. Then I soldered a standard D-SUB9 socket to it. You could also use a crimp version here.

Since the PSX2 controller is very power hungry, it is not possible to tap the 5V at the C64’s control jacks. I just soldered a wire with a USB connector to it. USB power supplies are nowadays always available in the house and can be used with an external “Power Pack”, i.e. the batteries for charging cell phones, etc..

I used the circuit 1:1, except for the voltage regulator. I left out the voltage regulator, because I wanted to operate the circuit only with 5V anyway. The construction is quite simple, and should start with soldering the ATMega8. Then the two MCP41100 and the crystal. Followed by the two diodes and the remaining capacitors and resistors, the board is populated quite quickly.



![](https://github.com/DL2DW/PSX64_Adapter_for_C64/blob/main/Images/PSX64_Platine_fertig_bestück_klein.jpg)





## Flashing the firmware


The 6pin connector, shown at the bottom right of the picture, is the ISP interface to load the firmware. A 2-row pin header with 6 pins can be soldered there. The pinout corresponds to the 6pin ISP interface.

Since the firmware is flashed only once, and updates are probably no longer to be expected, I have not soldered the pin header, but only plugged it in and held it a bit tilted during flashing. Certainly not the official procedure, but for extra a pin header was not worth it to me. Especially since I wanted to wrap the board in heat shrink tubing and then the pin header would have only disturbed.

I installed the firmware again with the well known tool “avrdude”. On the website of the manufacturer there are two versions, one for the ATMega8 and then for the ATMega168. But since no features of the ATMega 168 are used, this hardware upgrade is not worthwhile,

I use an AVR ISP MKII for flashing, but any other compatible programming adapter can be used. For my programming adapter the call looks like this:

***`avrdude -c avrispmkII -p m8 -U flash:w:atmega8.hex`***

```
avrdude: AVR device initialized and ready to accept instructions
Reading | ################################################## | 100% 0.00s
avrdude: Device signature = 0x1e9307 (probably m8)avrdude: NOTE: "flash" memory has been specified, an erase cycle will be performed     To disable this feature, specify the -D option.avrdude: erasing chipavrdude: reading input file "atmega8.hex"avrdude: input file atmega8.hex auto detected as Intel Hexavrdude: writing flash (4904 bytes):
Writing | ################################################## | 100% 1.77s
avrdude: 4904 bytes of flash writtenavrdude: verifying flash memory against atmega8.hex:avrdude: load data flash data from input file atmega8.hex:avrdude: input file atmega8.hex auto detected as Intel Hexavrdude: input file atmega8.hex contains 4904 bytesavrdude: reading on-chip flash data:
Reading | ################################################## | 100% 1.36s
avrdude: verifying ...avrdude: 4904 bytes of flash verified
avrdude: safemode: Fuses OK (E:FF, H:D9, L:E1)
avrdude done. Thank you.
```

Now set the fuses so that the external 16MHz crystal can be used:

***`avrdude -c avrispmkII -p m8 -U lfuse:w:0xce:m -U hfuse:w:0xc9:m`***

```
avrdude: AVR device initialized and ready to accept instructions
Reading | ################################################## | 100% 0.00s
avrdude: Device signature = 0x1e9307 (probably m8)avrdude: reading input file "0xce"avrdude: writing lfuse (1 bytes):
Writing | ################################################## | 100% 0.01s
avrdude: 1 bytes of lfuse writtenavrdude: verifying lfuse memory against 0xce:avrdude: load data lfuse data from input file 0xce:avrdude: input file 0xce contains 1 bytesavrdude: reading on-chip lfuse data:
Reading | ################################################## | 100% 0.00s
avrdude: verifying ...avrdude: 1 bytes of lfuse verifiedavrdude: reading input file "0xc9"avrdude: writing hfuse (1 bytes):
Writing | ################################################## | 100% 0.01s
avrdude: 1 bytes of hfuse writtenavrdude: verifying hfuse memory against 0xc9:avrdude: load data hfuse data from input file 0xc9:avrdude: input file 0xc9 contains 1 bytesavrdude: reading on-chip hfuse data:
Reading | ################################################## | 100% 0.00s
avrdude: verifying ...avrdude: 1 bytes of hfuse verified
avrdude: safemode: Fuses OK (E:FF, H:C9, L:CE)
avrdude done. Thank you.
```

Damit ist die Programmierung abgeschlossen und der PSX64 Adapter fertig. 



![](https://github.com/DL2DW/PSX64_Adapter_for_C64/blob/main/Images/PSX64_Adapter_Stromversorgung_klein.jpg)



I then packed the whole thing completely in heat shrink tubing. If you want to do this in a similar way, you should always remember to put on the heat shrink before the plugs are mounted. Otherwise the tube can not be pulled over, because it would be too small for the plugs.



## Ready

The finished adapter looks like this:



![](https://github.com/DL2DW/PSX64_Adapter_for_C64/blob/main/Images/PSX64_Adapter_Schrumpfschlauch_fertig.jpg)



And the whole thing still complete with the two connectors:



![](https://github.com/DL2DW/PSX64_Adapter_for_C64/blob/main/Images/PSX64_Adapter_fertig.jpg)



As mentioned at the beginning, the board is small enough to be mounted directly into the PSX2 gamepad. But since I still use my gamepads for the PSX2, I chose the external version. Would be happy to see some pictures of it though if anyone is considering installing it.



## BOM

The brands are only representative. Comparable components from other manufacturers can also be used.



|      | Description                                                  | Part                | References | Value             | Footprint                       | Quantity Per PCB |
| ---- | ------------------------------------------------------------ | ------------------- | ---------- | ----------------- | ------------------------------- | ---------------- |
| 1    | Unpolarized capacitor                                        | C                   | C4         | 100n              | C_0805_2012Metric               | 1                |
| 2    | Unpolarized capacitor                                        | C                   | C2 C3      | 22p               | C_0805_2012Metric               | 2                |
| 3    | Polarized capacitor                                          | CP                  | C1         | 4,7µ              | CP_EIA-3216-18_Kemet-A          | 1                |
| 4    | 400V 1A General Purpose Rectifier Diode, DO-41               | 1N4004              | D1 D2      | 1N4004            | D_SMA                           | 2                |
| 5    | Generic connector, single row, 01x02, script generated (kicad-library-utils/schlib/autogen/connector/) | Conn_01x02          | J2         | 5V Power          | PinHeader_1x02_P2.00mm_Vertical | 1                |
| 6    | Generic connector, single row, 01x09, script generated (kicad-library-utils/schlib/autogen/connector/) | Conn_01x09          | J1 J3      | C64 DB9 Connector | PinHeader_1x09_P2.00mm_Vertical | 2                |
| 7    | Generic connector, double row, 02x03, odd/even pin numbering scheme (row 1 odd numbers, row 2 even numbers), script generated (kicad-library-utils/schlib/autogen/connector/) | Conn_02x03_Odd_Even | J4         | ICSP              | PinHeader_2x03_P2.54mm_Vertical | 1                |
| 8    | Resistor                                                     | R                   | R1 R2      | 10k               | R_0805_2012Metric               | 2                |
| 9    | 16MHz, 8kB Flash, 1kB SRAM, 512B EEPROM, TQFP-32             | ATmega8-16AU        | U3         | ATmega8-16AU      | TQFP-32_7x7mm_P0.8mm            | 1                |
| 10   | Single Digital Potentiometer, SPI interface, 256 taps, 100 kohm | MCP41100            | U1 U2      | MCP41100          | SOIC-8_3.9x4.9mm_P1.27mm        | 2                |
| 11   | Two pin crystal                                              | Crystal             | Y1         | 16MHz             | Crystal_SMD_5032-2Pin_5.0x3.2mm | 1                |



## PCB

The PCB can either be ordered directly from [PCBWay](https://www.pcbway.com/project/shareproject/REX_9628_Extern_Kernel_II_8.html), or you can create it yourself from the Gerber files available here.

[![PCBWay](https://www.pcbway.com/project/img/images/frompcbway.png)](https://www.pcbway.com/project/shareproject/REX_9628_Extern_Kernel_II_8.html)



If you liked my project, I would be very happy about a small coffee donation.

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/R6R62T6RN)



# License

The contents of this repository, with the exception of the Docs and Software directories, are released under the following license:

- the "Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License" (CC BY-NC-SA 4.0) full text of this license is included in the [LICENSE.CC-NC-BY-SA-4.0](https://github.com/DL2DW/PSX64_Adapter_for_C64/blob/main/LICENSE.CC-NC-BY-SA) file and a copy can also be found at https://creativecommons.org/licenses/by-nc-sa/4.0/
