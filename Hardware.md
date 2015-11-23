# Contents #




---


## Generally ##

In order to save Arduino output pins, one can use a shift register. The principle is pretty straight forward:

Arduino --> shift register --> LCD

That's it!


---


This library supports some different ways to connect to the shift register, as well as a couple of different ways to connect to the LCD. In addition to LCD backlight (except where noted).

So this wall of text tries to explain a bit about that. You might want to skip to the [Schematics examples](http://code.google.com/p/shiftreglcd123/wiki/Schematics) page for (more) complete examples instead.

Ok, not completely complete schematics, some things are lacking (or, implied-ish):
  * Power connections for the shift register.
  * Decoupling capacitor for the shift register
  * Potmeter for the LCD contrast pin (About 5k should do?)


---


### Arduino to shiftregister ###
The nr. of wires denotes the nr. of output pins that are required from the Arduino to control the LCD (included shiftregister).
  * 1-wire (slow)
  * 2-wire (not so slow)
  * 3-wire (a little less slow)


### Shiftregister to LCD ###
Then there is a choice of two connection variants (schemes) for how to hook up a shiftregister to an LCD:
  * ShiftRegLCD123, also compatible with previous [ShiftRegLCD](http://code.google.com/p/arduinoshiftreglcd/).
  * LCD3Wire, included for compatibility so this library can be used with that way of wiring it up as well.


### Backlight ###
There is also support for LCD backlight (simple, slow on/off, no PWM), that can be combined with most modes.

**Notes**
  * LCD3Wire cannot use 2-wire mode.
  * LCD3Wire can only have backlight in 3-wire mode.



---


# Details #

<font color='#FF0000'><b>NOTE, not shown in the schematics:</b></font>
  * **Power connections** to the shift registers
  * **Decoupling capacitors** near the shift register, with say a 100nF capacitor across Vcc - GND.
  * **Contrast potmeter** for the LCD.

## Wiring from Arduino to a shiftregister ##

### 1-wire ###

  * Must use a latched shiftregister
  * Shiftregister bit #0 (LSB) cannot be used (is zero because of latch signal).
  * 2 versions shown, a slow with minimum components, and a somewhat faster one.

Very inspired by Roman Black's _excellent_ explanation of his [Shift1 system](http://www.romanblack.com/shift1.htm) for the PIC.

I could not explain it better, so I recommend having a look at that. In short, the LCD signal line must be kept high when not writing to the LCD. 0, 1 and latch signals are transmitted by differently timed LOW signals.

This library is slower than "Shift1 system" though, since DigitalWrite() function is slow. I _could_ use Shift1's timing, but that would require using direct port access on the Arduino. And as I'm a bit unsure how compatible such an approach would be to different Arduino's, I opted for a slower RC timing.

Even if my chosen RC time constant are 5 and 10 times Shift1's, I guesstimate it's only about 3 times slower than Shift1 system on average, using a "diode quickcharge" method, and a somewhat tighter timing. But it should be safe to use, with plenty of headroom / difference beween 1-bits, 0-bits and latch commmands.

**1-wire is mostly a kind of last resort anyway, but it does save on pins if you are in a pinch.**


#### Slowest version ####
With the least amount of external components.

This require that you edit the libray and comment out this line in ShiftRegLCD123.cpp:

`#define LCD_SLIGHTLY_FASTER_1_WIRE)`

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-1-wire-slow.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-1-wire-slow.png)


#### Slightly faster ####
With the "diode quickcharge" method. No editing of the library needed.

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-1-wire-diode-quickcharge.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-1-wire-diode-quickcharge.png)


### 2-wire ###
  * Can use either a latched or unlatched shiftregister.
  * Shiftregister bit #0 (LSB) cannot be used (is zero because of avoiding accidental LCD enable signal).
  * ShiftRegLCD123 only. LCD3Wire cannot use this method. Exactly the same as [arduinoshiftreglcd / ShiftRegLCD](http://code.google.com/p/arduinoshiftreglcd/).
  * Backlight will flicker if writing continously to the LCD (like doing animations), unless using the "antiflicker backlight" circuit below. Note it may still flicker a bit, depending on LCD write frequency.


#### Diode-resistor AND gate ####
**This method relies on a "quick'n'dirty" diode-resistor AND "gate"**
(you can also use a real AND gate of course).

The diode characteristics are very important, not every diode works the same. Also important are the series resistor, and probably also what type of LCD you got. You need a small signal diode with the least amount of junction capacitance possible, although I have found some ways to get other diodes to work too. No guarantee that it will work, though.

**Diodes that should work:**
  * 1N4148
  * 1N914
  * Even a 2N3904 NPN transistor's B-E diode works, in a pinch.
A very short list, and I'm sure lots of other diodes fit the bill. Generally, I think diodes with less than about 5 pF junction capacitance should work (not confirmed). But this is dependent on a lot of factors.


Diodes that probably don't work:
  * 1N4001
  * BYV95
Generally, (rectifier) diodes with more than 10-15 pF junction capacitance will probably not work.

Experimentation might be called for to get this to work, if it doesn't work "out of the box".

#### 2-wire partial schematics ####

**The diode must come from the shiftregister's MSB bit (unless using Bill's method with a latched shiftregister, then it is the next-last bit).**

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-diode-resistor-AND.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-diode-resistor-AND.png)


Below is another variant I came up with, that actually worked with BYV95 (10pF junction capacitance), and an LCD that otherwise would not work with it. Take care not to overload the outputs of the Arduino (ca 30 mA) or the shiftregister when experimenting.

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-diodecapdischargequicker.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-diodecapdischargequicker.png)


### 3-wire ###
  * Can use either a latched or unlatched shiftregister.
    * LCD3Wire must use a latched shiftregister.
  * All shiftregister bits can be used.

#### ShiftRegLCD123 partial schematics ####

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR-SRLCD123-3-wire.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR-SRLCD123-3-wire.png)

Arduino forum user Nadir's solution with a latched shiftregister and old ShiftRegLCD:

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-3-wirelatched-Nadir.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-3-wirelatched-Nadir.png)


#### LCD3Wire partial schematics ####

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-3-wire-latched.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-3-wire-latched.png)



---


## Wiring from a shiftregister to a LCD ##

### 2 schemes ###

#### ShiftRegLCD123 ####
Specified by **`SRLCD123`** as scheme (or defaults to this if unspecified, in 1-wire mode).
  * Is slightly faster in 3-wire mode, as it do not need another write to the shiftregister for the LCD enable pulse.
  * connects LCD Enable directly from the Arduino in 3-wire mode.
  * Is the same as [arduinoshiftreglcd (ShiftRegLCD)](http://code.google.com/p/arduinoshiftreglcd/) for 2 and 3 wires.
  * Can have backlight control in any mode.

##### 1-wire SRLCD123 #####

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR-SRLCD123.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR-SRLCD123.png)

##### 2-wire SRLCD123 #####

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR-SRLCD123-2-wire.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR-SRLCD123-2-wire.png)

##### 3-wire SRLCD123 #####

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR-SRLCD123-3-wire.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR-SRLCD123-3-wire.png)


#### LCD3Wire ####
Specified by giving **LCD3WIRE** as scheme.
  * Is HW compatible with the wiring from [LCD3Wire library](http://www.arduino.cc/playground/Code/LCD3wires) in 3-wire mode.
    * Except for the added backlight control, but that was an unused bit on the shiftregister anyway.
  * Cannot have backlight if used in a 1-wire configuration.
  * Cannot be used with 2-wire, since it's Enable-bit is not at the last position. (Still possible to configure SRLCD123 library to do that, though, but it won't work).
  * Can only have backlight control in 3-wire mode.

##### 1- and 3-wire for LCD3Wire #####

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR-LCD3Wire.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR-LCD3Wire.png)



---


## LCD backlight ##
  * Can be combined with most wiring schemes above, except as noted.
  * formerly unused shiftregister bit #1 or bit #0 controls the backlight
    * In SRLCDOLD it was unused, and should therefore not cause any trouble. I guess the same goes for LCD3Wire.
  * Your LCD _might_ require another driver circuit, the one shown are just as an example, and **not to be taken as an absolute**. I used a small-signal transistor and a resistor suitable for a LED. You have to check your LCD documentation about what type of backlight it has.
  * In 2-wire mode it will flicker, if you write continously to the LCD (like doing animations), unless perhaps if using the "antiflicker" backlight circuit below.

Simply put, if you need LCD backlight, add it to the respective shiftregister output bit. If you don't need it, just ignore it.


#### Partial backlight schematics ####

##### For ShiftRegLCD and ShiftRegLCD123 #####

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR_Backlight-1.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR_Backlight-1.png)

##### For LCD3Wire #####
(Or ShiftRegLCD when using Bill's method with a latched shiftregister)

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR_Backlight-2.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-SR_Backlight-2.png)


##### 2-wire LCD backlight antiflicker circuit #####

Bill pointed out in [Issue #1](http://code.google.com/p/shiftreglcd123/issues/detail?id=1) that the backlight will flicker when using the 2-wire mode, and if writing to the LCD a lot (like animations). I did some tests and came up with the following circuit, that seems to work pretty well, for me at least.

Without this modification, the LCD backlight will never be really off in said circumstance, just dimmer. And flickering.

In fact even with this circuit it isn't completely off, but it is almost unnoticeable. To me it is completely unnoticeable in a normal or even a dimly lit room, but in complete darkness one can se a faint light that may flicker a bit at lower FPS values (10-20 FPS) It works best for about 20-40 FPS.

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/SR_Backlight-2-wire-antiflicker-1.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/SR_Backlight-2-wire-antiflicker-1.png)


---


For more complete example schematics, go to the [Schematics](Schematics.md) page.