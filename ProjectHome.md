# ShiftRegLCD123 #

## Introduction ##
A Hitachi HD4780 compatible LCD needs about 6 to 11 connections from an MCU to control it, in 4-bit or 8-bit parallel mode. Sometimes that is a bit much, especially for a small MCU and / or if you are low on I/O pins to use.

One solution is to use a simple SIPO (Serial In Parallel Out) shiftregister, and that's what this library does. So this one is just another Arduino shiftregister-to-LCD libray, and I offer this simply because I started tinkering with it.


Features:
  * 1, 2 or 3-wire modes. Generally more wires = faster.
  * Two connection types from the shiftregister to the LCD:
    * ShiftRegLCD123 (Compatible with [arduinoshiftreglcd](http://code.google.com/p/arduinoshiftreglcd/) in 2- and 3-wire mode).
    * [LCD3Wire](http://www.arduino.cc/playground/Code/LCD3wires).
  * LCD backlight control.


# Details #

This library is basically a rather late update of [arduinoshiftreglcd](http://code.google.com/p/arduinoshiftreglcd/) to cope with the 1-wire alternative, backlight control and also compatible with LCD3Wire schematics.

  * This library is a (tiny) little bit larger than the old one, so if that is really, really crucial, you might try that one instead. It is identical to this in 2- and 3-wire mode, except the old library does not identify 16x4 LCD's.
  * Proper and very safe Hitachi HD44780 initialization and timings are implemented (no reading of the Busy Flag yet), and it is therefore probably not the fastest HD44780-based LCD library out there.
  * Syntax / usage should be very similar if not directly compatible with official Arduino LiquidCrystal library.
    * Except for added backlight control.
    * It is thus a bit different than the old arduinoshiftreglcd library. Some (4) functions are renamed to be the same as the official library.

More details on the [How to use](Software.md), [How it works](Hardware.md) and [Example schematics](Schematics.md) pages.