This is an automatic translation, may be incorrect in some places. See sources and examples!

# GRGB
Library for controlling RGB LEDs and strips for Arduino. Lightweight version of the GyverRGB library
- Support for common anode and common cathode drivers
- Brightness setting
- Brightness gamma correction (square CRT)
- The library can not be tied to pins and simply generate 8 bit values
- Fast optimized integer calculations (not everywhere)
- Smooth transition between any colors (does not block code execution)
- Set color in different ways:
    - RGB
    - HSV
    - Fast HSV
    - Color wheel (1530 values)
    - Color wheel (255 values)
    - Heat (1000-40000K)
    - HEX colors 24 bit
    - HEX colors 16 bit
    - 17 preset colors

### Compatibility
Compatible with all Arduino platforms (using Arduino functions)

## Content
- [Install](#install)
- [Initialization](#init)
- [Usage](#usage)
- [Example](#example)
- [Versions](#versions)
- [Bugs and feedback](#feedback)

<a id="install"></a>
## Installation
- The library can be found by the name **GRGB** and installed through the library manager in:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Download library](https://github.com/GyverLibs/GRGB/archive/refs/heads/main.zip) .zip archive for manual installation:
    - Unzip and put in *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Unzip and put in *C:\Program Files\Arduino\libraries* (Windows x32)
    - Unpack and put in *Documents/Arduino/libraries/*
    - (Arduino IDE) automatic installation from .zip: *Sketch/Include library/Add .ZIP library…* and specify the downloaded archive
- Read more detailed instructions for installing libraries [here] (https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE% D0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)

<a id="init"></a>
## Initialization
```cpp
// specify driver type COMMON_CATHODE/COMMON_ANODE
// and PWM pins (on which analogWrite works) in order R,G,B
GRGB led(COMMON_CATHODE, pinR, pinG, pinB);

// 10 bit PWM mode (for esp8266)
GRGB led(COMMON_CATHODE, pinR, pinG, pinB, GRGB_10BIT);

// virtual driver - pins are not specified
GRGBled(COMMON_CATHODE);
```

<a id="usage"></a>
## Usage
```cpp
void enable(); // on
void disable(); // off
void setPower(bool power); // on off
void setRGB(uint8_t r, uint8_t g, uint8_t b, uint8_t br = 255); // set colors r, g, b: 0-255 + optional brightness
void setHSVfast(uint8_t h, uint8_t s, uint8_t v); // fast HSV
void setHSV(uint8_t h, uint8_t s, uint8_t v); // normal HSV
void setWheel(int color, uint8_t br = 255); // color wheel 0-1530 + optional brightness
void setWheel8(uint8_t color, uint8_t br = 255); // color wheel 0-255 + optional brightness
void setKelvin(int kelvin, uint8_t br = 255); // color temperature 1000-40000K + optional brightness
void setKelvinFast(int kelvin, uint8_t br = 255); // fast calculation. Color temperature 1000-10000K + optional brightness
void setHEX(uint32_t color, uint8_t br = 255); // HEX color 24 bits + optional brightness
void setHEX16(uint32_t color, uint8_t br = 255);// HEX color 16 bit + optional brightness
void setColor(uint32_t color, uint8_t br = 255); // color from preset RGBCOLORS + optional brightness
void setBrightness(uint8_tbri); // total brightness 0-255
void setCRT(bool crt); // enable/disable brightness gamma correction
bool tick(); // smooth color change ticker, call as often as possible if enabled
void fadeMode(bool fade); // enable/disable the smooth color change mode (off by default)
void setFadePeriod(uint32_t fadeTime); // set color change duration
void attach(void (*handler)()); // connect callback
void detach(); // disable callback
uint8_t R, G, B; // 8 bit signals for "virtual" driver, updated after color is set
```

<a id="example"></a>
## Example
See **examples** for other examples!
```cpp
#include <GRGB.h>
// specify driver type COMMON_CATHODE/COMMON_ANODE
// and pins in order R,G,B
GRGB led(COMMON_CATHODE, 9, 10, 11);

void setup() {
  // set TOTAL brightness
  led.setBrightness(255);

  // enable gamma correction (off by default)
  led.setCRT(true);
  //led.setCRT(false); // turn off

  // set RGB color [0-255]
  led.setRGB(255, 0, 0);
  //led.setRGB(255, 0, 0, 120); // + "local" brightness [0-255]
  delay(1000);

  // set HSV color (hue, saturation, lightness) [0-255]
  led.setHSV(120, 200, 250);
  delay(1000);

  // set color HSV fast algorithm (color, saturation, lightness) [0-255]
  led.setHSVfast(120, 200, 250);
  delay(1000);

  // set color from color wheel [0-1530]
  ledsetwheel(800);
  //led.setWheel(800, 120); // + "local" brightness [0-255]
  delay(1000);

  // set color from colorWheel [0-255]
  led.setWheel8(120);
  //led.setWheel8(120, 255); // + "local" brightness [0-255]
  delay(1000);

  // set color temperature [1000-40000] Kelvin
  led.setKelvin(5500);
  //led.setKelvin(5500, 255); // + "local" brightness [0-255]
  delay(1000);

  // set HEX color (24 bits)
  led.setHEX(0x00FF00);
  //led.setHEX(0x00FF00, 120); // + "local" brightness [0-255]
  delay(1000);

  // set HEX color (16 bits)
  led.setHEX16(0xF81F);
  //led.setHEX16(0xF81F, 120); // + "local" brightness [0-255]
  delay(1000);

  // set preset color (16 bit)
  led.setColor(GAqua); // list of colors in GRGB.h
  //led.setColor(GAqua, 120); // + "local" brightness [0-255]
  delay(1000);
}

void loop() {
  // smoothly blink brightness
  static uint32_t tmr;
  if (millis() - tmr >= 5) {
    tmr = millis();
    static int8_t dir = 1;
    static int val = 0;
    val += dir;
    if (val == 255 || val == 0) dir = -dir; // expand
    led.setBrightness(val);
  }
}
```

<a id="versions"></a>
## Versions
- v1.0
- v1.1 - added setKelvinFast
- v1.2 - added pseudo 10 bits
- v1.2.1 - bug fixed
- v1.3 - fixed local brightness
- v1.3.1 - fixed inversion for 10 bits
- v1.4 - added enable(), disable() and setPower(bool)
- v1.4.1 - Digispark compatibility

<a id="feedback"></a>
## Bugs and feedback
When you find bugs, create an **Issue**, or better, immediately write to the mail [alex@alexgyver.ru](mailto:alex@alexgyver.ru)
The library is open for revision and your **Pull Request**'s!