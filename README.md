# Arduino ATmega16U2 USB chip firmware

## Sources

* `hardware/arduino/avr/firmwares/atmegaxxu2` at <https://github.com/arduino/Arduino>
* `firmware/atmega16u2` at <https://github.com/Pinoccio/hardware-pinoccio>

## How to build

    # DFU bootloader
    (cd Bootloaders/arduino-usbdfu && make clean && make)
    # USB serial firmware
    (cd Projects/arduino-usbserial && make clean && make)

## Note very well

* The LUFA version is 100807, as in the hardware-pinoccio repository. This version is required for both DFU and USB serial firmware.

## References

* LUFA source: https://github.com/abcminiuser/lufa

## Original notice in the Arduino firmware distribution follows

See <https://github.com/arduino/Arduino/blob/master/hardware/arduino/avr/firmwares/atmegaxxu2/README.txt>

## Arduino Uno and Mega 2560 Firmwares for the ATmega16U2

This directory contains the firmwares used on the ATmega16U2 on the Arduino
Uno and Arduino Mega 2560.  The arduino-usbdfu directory contains the DFU
bootloader on the 16U2; the arduino-usbserial directory contains the actual
usb to serial firmware.  Both should be compiled against LUFA 100807.  The
two .hex files in this directory combine the dfu and serial firmwares into
a single file to burn onto the 16U2.  

To burn (Uno):

```
avrdude -p atmega16u2 -F -P usb -c avrispmkii \
  -U flash:w:UNO-dfu_and_usbserial_combined.hex \
  -U lfuse:w:0xFF:m -U hfuse:w:0xD9:m -U efuse:w:0xF4:m \
  -U lock:w:0x0F:m
```

To burn (Mega 2560):

```
avrdude -p atmega16u2 -F -P usb -c avrispmkii \
  -U flash:w:MEGA-dfu_and_usbserial_combined.hex \
  -U lfuse:w:0xFF:m -U hfuse:w:0xD9:m -U efuse:w:0xF4:m \
  -U lock:w:0x0F:m
```

Note on USB Vendor IDs (VID) and Product IDs (PID): The arduino-usbdfu
project uses Atmel's VID and MCU-specific PIDs to maintain compatibility
with their FLIP software.  The source code to the arduino-usbserial
project includes Atmel's VID and a PID donated by them to LUFA.  This
PID is used in LUFA's USBtoSerial project, which forms the basis for
arduino-usbserial.  According to the LUFA documentation, this VID/PID
combination is:

> "For use in testing of LUFA powered devices during development only,
>  by non-commercial entities. All devices must accept collisions on this
>  VID/PID range (from other in-development LUFA devices) to be resolved
>  by using a unique release number in the Device Descriptor. No devices
> using this VID/PID combination may be released to the general public."

The production version of the arduino-usbserial firmware uses the
Arduino VID.  This is only for use with official Arduino hardware and
should not be used on other products.
