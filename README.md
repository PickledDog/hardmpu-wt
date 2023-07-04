# hardmpu-wt
 HardMPU with wavetable module support

![Assembled HardMPU](/img/assembled.jpg)

## Overview
This is a clone of ab0tj's HardMPU board. I wanted one with a "Wave Blaster" header, so... here it is! If you don't need to install a wavetable module on your HardMPU, rather use ab0tj's original board. In particular, to quote the HardMPU repo, `the circuit design is free for non-commercial use.` The presence of this implementation does not permit commercial exploitation. If you build this, you may do so only for your own personal use. All [software](https://github.com/ab0tj/HardMPU/) is bound by the terms of the GPL. 

## Initial setup
The ATmega needs to be programmed before use. This can be done using any AVR programmer (AVRISP, STK500, USBtinyISP, USBasp etc), or a universal programmer (TL866, T48).

### Using an AVR programmer
This method programs the ATmega chip in-place (with the board fully assembled), through the J4 ISP connector. The listed programmers work similarly, but AVRDUDE will need to be told what programmer you are using. This example uses the [programmer from Pololu](https://www.pololu.com/product/3172).

Program the board loose (not plugged into a computer!) with the programmer configured to power the target at 5V. The required firmware can be found [in ab0tj's repository](https://github.com/ab0tj/HardMPU/tree/master/bin) (download all files), and [AVRDUDE](https://github.com/avrdudes/avrdude/) is used to install the firmware onto the ATmega.

You will need to modify the `program.bat` to reflect your programmer and environment. For example, to use a Pololu programmer detected at COM4, change (per-line):
```
-c usbasp-clone
```
to
```
-c stk500v2 -P COM4
```
For Mac and Linux, change the BATch file to a shell script and adapt it accordingly (use `/dev/ttyUSB0` instead of `COM4` etc).

### Using a TL866
If you're using a TL866 or T48, the firmware is programmed directly to the ATmega chip before installing it - the J4 connector isn't used here, and can be omitted. Download `HardMPU.hex` from [ab0tj's repository](https://github.com/ab0tj/HardMPU/tree/master/bin) and open it with the standard XGpro tool. Go to the **Config** tab and set the fuses as follows:

![ATmega1284 fuse config](/img/fuses.png)

With the firmware loaded and the fuses set, go ahead and program the ATmega chip.

## User port
The optional User port (J2) allows adventurous builders to add features to the board, either internally or externally. It breaks out 4 GPIO pins, an I²C interface, the "Internal" serial interface (shared with the Wavetable header), and fused power and ground pins. Either regular or right-angled header (as pictured) can be installed in the J2 position; a hole will need to be cut in the bracket for external access. You will need to write your own code to use these interfaces - maybe add a status display using a common I²C LCD? The possibilities are not particularly limited!

## Part selection
The design permits "nicer" parts than necessary - for example, the audio jack footprint accepts both switched and unswitched sockets. Not all pads and holes in the board need to be filled. The OPA2134 can be substituted for other common dual opamps (like the NE5532). Some connectors are optional - J2 and J5 in particular are seldom-used. An [ATmega1284](https://www.mouser.com/ProductDetail/556-ATMEGA1284-PU) can be used in place of the ATmega1284P if the latter isn't in stock; you will need to update `program.bat` accordingly.

## Parts list
Bill Of Materials and part references are below. The specified parts are just the ones I used, and can be substituted as needed - Mouser links provided for convenience and reference. You will need to drill a single hole in the specified bracket for the audio jack - refer to the PCB file for size and location.

| Reference | Value | Qty | Mouser link |
| --------- | ----- | --- | ----------- |
| C1-C10 | 0.1uF ceramic | 10 | [KEMET C315C104M5U5TA](https://www.mouser.com/ProductDetail/80-C315C104M5U-TR) |
| C11, C12 | 22pF ceramic | 2 | [KEMET C315C220K2G5TA](https://www.mouser.com/ProductDetail/80-C315C220K2G) |
| C13, C14 | 0.47uF ceramic | 2 | [TDK FG18X7R1H474K](https://www.mouser.com/ProductDetail/810-FG18X7R1H474KRT0) |
| C15, C16 | 1uF ceramic | 2 | [TDK FG18X7R1E105K](https://www.mouser.com/ProductDetail/810-FG18X7R1E105KRT0) |
| C17 | 22μF electrolytic | 1 | [Panasonic ECE-A1AKS220](https://www.mouser.com/ProductDetail/667-ECE-A1AKS220) |
| F1 | 500mA PPTC fuse | 1 | [Bourns MF-R050-0-17](https://www.mouser.com/ProductDetail/652-MF-R050-0-17) |
| J1 | D-Sub DA-15 female | 1 | [Amphenol L77SDA15SA4CH4F](https://www.mouser.com/ProductDetail/523-L77SDA15SA4CH4F) |
| J2 | 2x5 right-angle header | 1 | [Amphenol 10129382-910002BLF](https://www.mouser.com/ProductDetail/649-1012938291002BLF) |
| J3 | 3.5mm jack socket | 1 | [CUI SJ1-3553NG](https://www.mouser.com/ProductDetail/490-SJ1-3553NG) |
| J4 | 2x3 box header | 1 | [Wurth 61200621621](https://www.mouser.com/ProductDetail/710-61200621621) |
| J5 | 2x5 box header | 1 | [Wurth 61201021621](https://www.mouser.com/ProductDetail/710-61201021621) |
| J6 | 2x13 header | 1 | [Amphenol 67997-226HLF](https://www.mouser.com/ProductDetail/649-67997-226HLF) |
| J7 | CD-Audio socket | 1 | [Molex 70551-0003](https://www.mouser.com/ProductDetail/538-70551-0003) |
| JP1 | 2x4 header | 1 | [Amphenol 10129381-908002BLF](https://www.mouser.com/ProductDetail/649-1012938190802BLF) |
| JP2 | 2x6 header | 1 | [Amphenol 10129381-912002BLF](https://www.mouser.com/ProductDetail/649-1012938191202BLF) |
| JP3 | 2x3 header | 1 | [Amphenol 10129381-906002BLF](https://www.mouser.com/ProductDetail/649-1012938190602BLF) |
| | jumpers | 4 | [Harwin M7583-46](https://www.mouser.com/ProductDetail/855-M7583-46)
| R1-R3 | 4k7Ω resistor | 3 | [Yageo CFR-25JR-52-4K7](https://www.mouser.com/ProductDetail/603-CFR-25JR-524K7) |
| R4, R5 | 220Ω resistor | 2 | [Yageo CFR-25JR-52-220R](https://www.mouser.com/ProductDetail/603-CFR-25JR-52220R) |
| U1, U2 | 74HCT138 | 2 | [TI CD74HCT138E](https://www.mouser.com/ProductDetail/595-CD74HCT138E) |
| U3 | 74HCT08 | 1 | [TI CD74HCT08E](https://www.mouser.com/ProductDetail/595-CD74HCT08E) |
| U4, U5 | 74HCT574 | 2 | [TI CD74HCT574E](https://www.mouser.com/ProductDetail/595-CD74HCT574E) |
| U6 | 74HCT240 | 1 | [TI SN74HCT240N](https://www.mouser.com/ProductDetail/595-SN74HCT240N) |
| U7, U8 | 74HCT74 | 2 | [TI CD74HCT74E](https://www.mouser.com/ProductDetail/595-CD74HCT74E) |
| U9 | ATmega1284P | 1 | [Atmel ATmega1284P](https://www.mouser.com/ProductDetail/556-ATMEGA1284P-PU) |
| | socket | 1 | [Amphenol DILB40P-223TLF](https://www.mouser.com/ProductDetail/649-DILB40P223TLF) |
| U10 | 78L05 | 1 | [ST L78L05ACZ](https://www.mouser.com/ProductDetail/511-L78L05ACZ) |
| U11 | 79L05 | 1 | [ST L79L05ACZ](https://www.mouser.com/ProductDetail/511-L79L05ACZ) |
| U12 | OPA2134 | 1 | [TI OPA2134PA](https://www.mouser.com/ProductDetail/595-OPA2134PA) |
| Y1 | 20MHz crystal | 1 | [Arbacon ABL-20.000MHz-B1U](https://www.mouser.com/ProductDetail/815-ABL-20-B1U) |
| bracket | ISA DA-15 bracket | 1 | [Keystone 9200-11](https://www.mouser.com/ProductDetail/534-9200-11) |