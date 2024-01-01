## Advisory

It was discovered that in rare circumstances the MegaCMD could corrupt its own bootloader, if operating under low power.

These instructions do not apply to the DIY MegaCommand!!

## Symptoms:

1) MegaCMD suddenly does not boot.
2) Unable to upgrade the MCL OS.

## Solution:

To fix this issue we need to re-flash the bootloader with the corrected fuse settings.

The tools required will only cost a few dollars. The links provided are just typical examples,
please source them locally via ebay/aliexpress or an electronics store.

## Tools required:
  1) USBasp USBISP 3.3 V/5V AVR Programmer
     https://core-electronics.com.au/usbasp-usbisp-3-3v-5v-avr-programmer.html?gclid=Cj0KCQiAv8SsBhC7ARIsALIkVT0Ud-RIRIi8ZF0OTpDQ5HzNdAqZZreSxNwTLxYEdv5KJ_r_GVjRA7gaAmF3EALw_wcB

  2) AVR Programming Cable that incorporates 10pin and 6pin ISP headers.
     https://www.sparkfun.com/products/9215

## Procedure:
  1) Install avrdude on your system.

  2) Connect the cable to the 6 pin header closest to the Atmega2560 microcontroller. (See reference image below for correct cable placement and orientation)

  4) Connect the USB programmer to your computer. The LEDs should light up, and if the MegaCMD is still operational it will power on.

  5) Run the avdude command below to flash the bootloader, ensure the optiboot_atmega2560.hex file is in your current working directory.

     ```avrdude -c usbasp -p m2560 -U flash:w:optiboot_atmega2560.hex -U lfuse:w:0xFF:m -U hfuse:w:0xDE:m -U efuse:w:0xFF:m -U lock:w:0x0F:m -B10```

     (See sample output below).

  6) Now the bootloader is reflashed, disconnect the programming cables.

  7) Connect the MegaCMD to the computer via USB.

  8) We must put the MegaCMD in to serial mode for the OS upgrade.

     Use SysexLibrarian or equivalent to transmit USB_SERIAL.hex to the MegaCMD via USB-MIDI.

     The MegaCMD will now function as a USB serial device.

  9) Perform standard MCL OS upgrade using HexUploader or XLoader following instructions from https://github.com/jmamma/MCL/releases

### Reference Image

![alt text](https://github.com/jmamma/MegaCMD/blob/main/Bootloader/programmer.jpg?raw=true)

### Sample Output

```
avrdude -c usbasp -p m2560 -U flash:w:optiboot_atmega2560.hex -U lfuse:w:0xFF:m -U hfuse:w:0xDE:m -U efuse:w:0xFF:m -U lock:w:0x0F:m 

avrdude: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.00s

avrdude: Device signature = 0x1e9801 (probably m2560)
avrdude: NOTE: "flash" memory has been specified, an erase cycle will be performed
         To disable this feature, specify the -D option.
avrdude: erasing chip
avrdude: reading input file "optiboot_atmega2560.hex"
avrdude: input file optiboot_atmega2560.hex auto detected as Intel Hex
avrdude: writing flash (262144 bytes):

Writing | ################################################## | 100% 0.00s

avrdude: 262144 bytes of flash written
avrdude: verifying flash memory against optiboot_atmega2560.hex:
avrdude: load data flash data from input file optiboot_atmega2560.hex:
avrdude: input file optiboot_atmega2560.hex auto detected as Intel Hex
avrdude: input file optiboot_atmega2560.hex contains 262144 bytes
avrdude: reading on-chip flash data:

Reading | ################################################## | 100% 0.00s

avrdude: verifying ...
avrdude: 262144 bytes of flash verified
avrdude: reading input file "0xFF"
avrdude: writing lfuse (1 bytes):

Writing | ################################################## | 100% 0.00s

avrdude: 1 bytes of lfuse written
avrdude: verifying lfuse memory against 0xFF:
avrdude: load data lfuse data from input file 0xFF:
avrdude: input file 0xFF contains 1 bytes
avrdude: reading on-chip lfuse data:

Reading | ################################################## | 100% 0.00s

avrdude: verifying ...
avrdude: 1 bytes of lfuse verified
avrdude: reading input file "0xDE"
avrdude: writing hfuse (1 bytes):

Writing | ################################################## | 100% 0.00s

avrdude: 1 bytes of hfuse written
avrdude: verifying hfuse memory against 0xDE:
avrdude: load data hfuse data from input file 0xDE:
avrdude: input file 0xDE contains 1 bytes
avrdude: reading on-chip hfuse data:

Reading | ################################################## | 100% 0.00s

avrdude: verifying ...
avrdude: 1 bytes of hfuse verified
avrdude: reading input file "0xFF"
avrdude: writing efuse (1 bytes):

Writing | ################################################## | 100% 0.00s

avrdude: 1 bytes of efuse written
avrdude: verifying efuse memory against 0xFF:
avrdude: load data efuse data from input file 0xFF:
avrdude: input file 0xFF contains 1 bytes
avrdude: reading on-chip efuse data:

Reading | ################################################## | 100% 0.00s

avrdude: verifying ...
avrdude: 1 bytes of efuse verified
avrdude: reading input file "0x0F"
avrdude: writing lock (1 bytes):

Writing | ################################################## | 100% 0.01s

avrdude: 1 bytes of lock written
avrdude: verifying lock memory against 0x0F:
avrdude: load data lock data from input file 0x0F:
avrdude: input file 0x0F contains 1 bytes
avrdude: reading on-chip lock data:

Reading | ################################################## | 100% 0.00s

avrdude: verifying ...
avrdude: 1 bytes of lock verified

avrdude: safemode: Fuses OK (E:FF, H:DE, L:FF)

avrdude done.  Thank you.
```
