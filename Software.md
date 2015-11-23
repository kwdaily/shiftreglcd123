


---


# Installing the library #

  * Download the most current version of the library.
  * Create a subfolder in your arduino sketch folder named "libraries", if it doesn't exist. [As described here](http://arduino.cc/blog/?p=313=1)
  * Extract the library. It should be in a subfolder of its own (something like your-sketch-folder/libraries/ShiftRegLCD123/).
  * Then, when you start the Arduino IDE, it should contain a new menu "ShiftRegLCD123" with examples. (**File > Examples > ShiftRegLCD123**...). It should also appear in the menu **Sketch > import library**.



---


# Using #

## 1: Instantiate / make an LCD object, set arduino output pins and LCD wiring scheme: ##

`ShiftRegLCD123 LCDobject( DataPin [, scheme] )`
> or
`ShiftRegLCD123 LCDobject( DataPin , ClockPin, scheme )`
> or
`ShiftRegLCD123 LCDobject( DataPin , ClockPin, LatchPin, scheme )`

> where:
    * DataPin : Arduino pin to shiftregister serial data input.
    * ClockPin: Arduino pin to shiftregister clock input.
    * LatchPin: Arduino pin to shiftregister latch/strobe/register clock input. **NOTE** Latchpin goes directly to LCD Enable if using SRLCD123 in 3-wire.
    * scheme  : Indicates shiftregister to LCD wiring type / variant.
      * `SRLCD123` for ShiftRegLCD123 (Also compatible with old ShiftRegLCD in 2 and 3-wire mode)
      * `LCD3WIRE` for LCD3Wire compatible wiring.

## 2: Initialize the LCD by calling begin() function with LCD size and font: ##

`LCDobject.begin( cols, lines [, font] )`

> where:
    * cols  : Nr. of columns in the LCD
    * lines : Nr. of "logical lines" in the LCD (not neccesarily physical lines)
    * font  : 0 = small (default), 1 = large font for some 1-line LCD's.

Each time you call the begin() method, the LCD resets to the parameters given.


## Example ##

The compulsory "Hello, World" example, using a two-wire connection:

```
#include <ShiftRegLCD123.h>

const byte dataPin  = 10;    // SR Data from Arduino pin 10
const byte clockPin = 11;    // SR Clock from Arduino pin 11

// Instantiate a LCD object
ShiftRegLCD123 srlcd(dataPin, clockPin, SRLCD123);

void setup()
{
  // Initialize the LCD and set display size
  // LCD size 20 columns x 2 lines, defaults to small (normal) font
  srlcd.begin(20,2);
  
  // Turn on backlight (if used)
  srlcd.backlightOn();
  
  // Print a message to the LCD.
  srlcd.print("HELLO, WORLD!");
}

void loop()
{
}
```



---


# Functions #

The examples is for an object named `srlcd`. Of course you can name the object whatever you want. Or even have more than one.


## Backlight ##
```
srlcd.backlightOn(); // Turn backlight on
```
```
srlcd.backlightOff(); // Turn backlight off
```


## Clear display ##
```
srlcd.clear(); // Clear display, set cursor position to zero
```


## Cursor positioning ##
```
srlcd.home();  // Set cursor position to zero
```
```
srlcd.setCursor(column, row); // Sets cursor position
```
Remember row begins at row 0 (zero), for the first line.
Also column begins at column 0.


## Turn the display on/off ##
```
srlcd.noDisplay(); // Turn the display off
```
```
srlcd.display();   // Turn the display on
```


## Cursor types ##
```
srlcd.noCursor();//Turns the underline cursor off
```
```
srlcd.cursor();  // Turns the underline cursor on
```

```
srlcd.noBlink(); // Turn the blinking cursor off
```
```
srlcd.blink();   // Turn the blinking cursor on
```


## Scrolling ##
These commands scroll the display without changing the display RAM
```
srlcd.scrollDisplayLeft();
```
```
srlcd.scrollDisplayRight();
```


## Text flow direction ##
```
srlcd.leftToRight();  //  This is for text that flows Left to Right
```
(Formerly shiftLeft() in arduinoshiftreglcd)

```
srlcd.rightToLeft(); // This is for text that flows Right to Left
```
(Formerly shiftRight() in arduinoshiftreglcd


## Text justification ##
```
srlcd.autoscroll();    // This will 'right justify' text from the cursor
```
(Formerly shiftIncrement in arduinoshiftreglcd)

```
srlcd.noAutoscroll(); // This will 'left justify' text from the cursor
```
(Formerly shiftDecrement() in arduinoshiftreglcd)



## Custom character generation ##
There can be up to 8 custom generated characters.
```
srlcd.createChar(uint8_t location, uint8_t charmap[])
```
Where `location` is a number from 0 to 7, and `charmap[]` is an 8 element byte array of a character pattern. Beware the characters are only 5 pixels wide, so the 3 upper bits must always be zero.

For example, to make a bell character as custom character nr. 3:

```
uint8_t bell[8]  = { B00000100,
                     B00001110,
                     B00001110,
                     B00001110,
                     B00011111,
                     B00000000,
                     B00000100 };

srlcd.createChar(3, bell);
```