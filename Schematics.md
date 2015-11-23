

# Example schematics #

<font color='#FF0000'><b>NOTE, not shown in the schematics:</b></font>
  * **Power connections** to the shift registers
  * **Decoupling capacitors** near the shift register, with say a 100nF capacitor across Vcc - GND.
  * **Contrast potmeter** for the LCD (About 5k should do? I Guess the 1k - 10k range is ok).


---


## 1-wire LCD connection ##
  * Must use a latched shiftregister
  * Shiftregister bit #0 (LSB) cannot be used (is zero because of latch signal).

### ShiftRegLCD123 ###

Also showing backlight control via a transistor.

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD123-1-wire-slow-SRLCD.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD123-1-wire-slow-SRLCD.png)

**Quicker version**, About 2 times as quick as the above:
Again with backlight circuitry.

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD123-1-wire-SRLCD.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD123-1-wire-SRLCD.png)

### LCD3Wire ###

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD123-1-wire-LCD3Wire.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD123-1-wire-LCD3Wire.png)

**Note** The "compatibility" for LCD3Wire in 1-wire mode only goes from the shiftregister to the LCD, **not** from Arduino to the shiftregister! IE it is basically a useless mode. I only included it because it is possible to combine it like this.


---


## 2-wire LCD connection ##
  * Can use either a latched or unlatched shiftregister.
  * Shiftregister bit #0 (LSB) cannot be used (is zero because of avoiding accidental LCD enable signal).
  * LCD3Wire cannot use this method.

### ShiftRegLCD123 (2) ###
  * Exactly the same as [arduinoshiftreglcd / ShiftRegLCD](http://code.google.com/p/arduinoshiftreglcd/).

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-unlatched.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-unlatched.png)

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-unlatched_B.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-unlatched_B.png)

**Note** Depending on how you use the LCD, you might not want to have a simple backlight circuit like the above when using 2-wire mode. For an improved backlight circuit have a look at this: [2-wire LCD backlight antiflicker circuit](http://code.google.com/p/shiftreglcd123/wiki/Hardware#2-wire_LCD_backlight_antiflicker_circuit).


![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-latched-v1-Nystrom.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-latched-v1-Nystrom.png)


Bill's method of simply connecting together RCK and CLK.

**NOTE** The shiftregister output bits are shifted to one lower position when connecting RCK (or Strobe) together with CLK.

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-latched-v2-Bill.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-latched-v2-Bill.png)

Same with backlight control:
![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-latched-v2-Bill_B.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-2-wire-latched-v2-Bill_B.png)



---


## 3-wire LCD connection ##
  * Can use either a latched or unlatched shiftregister
    * LCD3Wire must use a latched shiftregister.
  * All shiftregister bits can be used.

### ShiftRegLCD123 (3) ###
  * Exactly the same as [arduinoshiftreglcd / ShiftRegLCD](http://code.google.com/p/arduinoshiftreglcd/).

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-ShiftRegLCD-3-wire-unlatched_B.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-ShiftRegLCD-3-wire-unlatched_B.png)

Arduino forum user Nadir had this solution a while ago for 3-wire with a latched shiftregister:
![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-3-wirelatched-Nadir.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD-3-wirelatched-Nadir.png)


### LCD3Wire (3) ###

![http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD123-3-wirelatched-LCD3Wire_B.png](http://i557.photobucket.com/albums/ss11/rar0n/Electronics/Arduino/ShiftRegLCD123/ShiftRegLCD123-3-wirelatched-LCD3Wire_B.png)



---
