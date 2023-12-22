This is an automatic translation, may be incorrect in some places. See sources and examples!

# GRGB
Library for managing RGB LEDs and ribbons for Arduino.Lighted version of the library Gyverrgb
- support for drivers with a common anode and a common cathode
- Setting brightness
- Gamma-correction of brightness (square CRT)
- the library may not be attached to the pins and simply generate values of 8 bits
- Fast optimized integer calculations (not everywhere)
- a smooth transition between any colors (does not block the performance of the code)
- Installation of color in different ways:
    - RGB
    - HSV
    - Quick HSV
    - color wheel (1530 values)
    - color wheel (255 values)
    - warmth (1000-40000k)
    - Hex color 24 bits
    - Hex color 16 bits
    - 17 pre -installed colors

## compatibility
Compatible with all arduino platforms (used arduino functions)

## Content
- [installation] (# Install)
- [initialization] (#init)
- [use] (#usage)
- [Example] (# Example)
- [versions] (#varsions)
- [bugs and feedback] (#fedback)

<a id="install"> </a>
## Installation
- The library can be found by the name ** GRGB ** and installed through the library manager in:
    - Arduino ide
    - Arduino ide v2
    - Platformio
- [download the library] (https://github.com/gyverlibs/grgb/archive/refs/heads/main.zip) .Zip archive for manual installation:
    - unpack and put in * C: \ Program Files (X86) \ Arduino \ Libraries * (Windows X64)
    - unpack and put in * C: \ Program Files \ Arduino \ Libraries * (Windows X32)
    - unpack and put in *documents/arduino/libraries/ *
    - (Arduino id) Automatic installation from. Zip: * sketch/connect the library/add .Zip library ... * and specify downloaded archive
- Read more detailed instructions for installing libraries [here] (https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%BD%D0%BE%BE%BE%BED0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)
### Update
- I recommend always updating the library: errors and bugs are corrected in the new versions, as well as optimization and new features are added
- through the IDE library manager: find the library how to install and click "update"
- Manually: ** remove the folder with the old version **, and then put a new one in its place.“Replacement” cannot be done: sometimes in new versions, files that remain when replacing are deleted and can lead to errors!


<a id="init"> </a>
## initialization
`` `CPP
// Indicate the type of driver Common_cathode/comon_anode
// and shim pins (on which Analogwrite works in order R, G, B
GRGB LED (Common_catHode, Pinr, Ping, Pinb);

// mode 10 bits shim (for ESP8266)
GRGB LED (Common_catHode, Pinr, Ping, Pinb, Grgb_10bit);

// Virtual driver - Pins are not indicated
GRGB LED (Common_cathode);
`` `

<a id="usage"> </a>
## Usage
`` `CPP
VOID Enable ();// VCL
Void Disable ();// Off
Void setpower (Bool Power);// on off
VOID setrgb (uint8_t r, uint8_t g, uint8_t b, uint8_t br = 255);// install colors r, g, b: 0-255 + optionally brightness
VOID sethsvfast (uint8_t h, uint8_t s, uint8_t v);// Fast hsv
VOID Sethsv (uint8_t h, uint8_t s, uint8_t v);// ordinary hsv
vCranberries OID setWheel (int color, uint8_t br = 255);// Color wheel 0-1530 + optionally brightness
VOID setWheel8 (Uint8_t Color, Uint8_t Br = 255);// Color wheel 0-255 + optionally brightness
VOID setkelvin (int Kelvin, Uint8_t Br = 255);// Color temperature 1000-40000k + optionally brightness
VOID setkelvinfast (Int Kelvin, Uint8_t Br = 255);// Fast calculation.Color temperature 1000-10000k + optionally brightness
VOID Sethex (Uint32_T Color, Uint8_t Br = 255);// hex color 24 bit + optionally brightness
VOID SetHex16 (Uint32_t Color, Uint8_t Br = 255);// hex color 16 bits + optionally brightness
VOID setcolor (Uint32_t Color, Uint8_t Br = 255);// Color from pre -installed rgbcolors + optionally brightness
VOID Setbrightness (Uint8_t Bri);// General brightness 0-255
VOID setcrt (bool CRT);// ON/OBEL GAMMA-correction of brightness
Bool Tick ();// ticker of smooth color change, call as often as possible if turned on
VOID Fademode (Bool Fade);// Turning on/off the smooth color change mode (default)
VOID Setfadeperiod (Uint32_T Fadetime);// set the duration of color change
VOID attach (VOID (*handler) ());// Connect collback
VOID Detach ();// Disconnect the collback
uint8_t r, g, b;// 8 bit signals for a "virtual" driver, are updated after installing color
`` `

<a id="EXAMPLE"> </a>
## Example
The rest of the examples look at ** Examples **!
`` `CPP
#include <grogb.h>
// Indicate the type of driver Common_cathode/comon_anode
// and pins in order r, g, b
GRGB LED (Common_catHode, 9, 10, 11);

VOID setup () {
  // Set the overall brightness
  led.setbrightness (255);

  // Turn on the gamma correction (disabled by default)
  led.setcrt (true);
  //LD.SetCrt(False);// switch off

  // Install the color RGB [0-255]
  led.setrgb (255, 0, 0);
  //LD.SetrgB (255, 0, 0, 120);// + "local" brightness [0-255]
  DELAY (1000);

  // Install the color hsv (color, saturation, brightness) [0-255]
  led.sethsv (120, 200, 250);
  DELAY (1000);

  // Install the color hsv fast algorithm (color, saturation, brightness) [0-255]
  led.sethsvfast (120, 200, 250);
  DELAY (1000);

  // Install color from the color wheel [0-1530]
  led.setwheel (800);
  //led.Setwheel(800, 120);// + "local" brightness [0-255]
  DELAY (1000);

  // Install color from the color wheel [0-255]
  led.setwheel8 (120);
  //led.Setwheel8(120, 255);// + "local" brightness [0-255]
  DELAY (1000);

  // set the color temperature [1000-40000] Kelvinov
  led.setkelvin (5500);
  //led.Setkelvin(5500, 255);// + "local" brightness [0-255]
  DELAY (1000);

  // Install HEX Color (24 Bits)
  led.sethex (0x00FF00);
  //LD.SetHEX ,0X00FF00, 120);// + "local" brightness [0-255]
  DELAY (1000);

  // install HEX color (16 bits)
  led.sethex16 (0xf81f);
  //LD.SetHEX16(0XF81F, 120);// + "local" brightness [0-255]
  DELAY (1000);

  // Install a pre -installed color (16 bits)
  led.setcolor (Gaqua);// List of flowers in grgb.h
  //led.setcolor(Gaqua, 120);// + "local" brightness [0-255]
  DELAY (1000);
}

VOID loop () {
  // Smoothly flashed with brightness
  Static uint32_t tmr;
  if (millis () - tmr> = 5) {
    TMR = Millis ();
    static int8_t dir = 1;
    static int vel = 0;
    val += dir;
    if (val == 255 || val == 0) die = -Dir;// unfold
    led.setbrightness (val);
  }
}
`` `

<a id="versions"> </a>
## versions
- V1.0
- V1.1 - added Setkelvinfast
- v1.2 - added pseudo 10 bits
- V1.2.1 - Bag is fixed
- v1.3 - fixed local brightness
- v1.3.1 - Fixed inversion for 10 bits
- V1.4 - added Enable (), Disable () and setPower (Bool)
- V1.4.1 - Digispark compatibility

<a id="feedback"> </a>
## baCranberries and feedback
Create ** Issue ** when you find the bugs, and better immediately write to the mail [alex@alexgyver.ru] (mailto: alex@alexgyver.ru)
The library is open for refinement and your ** pull Request ** 'ow!


When reporting about bugs or incorrect work of the library, it is necessary to indicate:
- The version of the library
- What is MK used
- SDK version (for ESP)
- version of Arduino ide
- whether the built -in examples work correctly, in which the functions and designs are used, leading to a bug in your code
- what code has been loaded, what work was expected from it and how it works in reality
- Ideally, attach the minimum code in which the bug is observed.Not a canvas of a thousand lines, but a minimum code