This is an automatic translation and may be incorrect in some places. See the source README and examples for authoritative information.

[![latest](https://img.shields.io/github/v/release/GyverLibs/GyverPID.svg?color=brightgreen)](https://github.com/GyverLibs/GyverPID/releases/latest/download/GyverPID.zip)
[![PIO](https://badges.registry.platformio.org/packages/gyverlibs/library/GyverPID.svg)](https://registry.platformio.org/libraries/gyverlibs/GyverPID)
[![Foo](https://img.shields.io/badge/Website-AlexGyver.ru-blue.svg?style=flat-square)](https://alexgyver.ru/)
[![Foo](https://img.shields.io/badge/%E2%82%BD%24%E2%82%AC%20%D0%9F%D0%BE%D0%B4%D0%B4%D0%B5%D1%80%D0%B6%D0%B0%D1%82%D1%8C-%D0%B0%D0%B2%D1%82%D0%BE%D1%80%D0%B0-orange.svg?style=flat-square)](https://alexgyver.ru/support_alex/)
[![Foo](https://img.shields.io/badge/README-ENGLISH-blueviolet.svg?style=flat-square)](https://github-com.translate.goog/GyverLibs/GyverPID?_x_tr_sl=ru&_x_tr_tl=en)  

[![Foo](https://img.shields.io/badge/ПОДПИСАТЬСЯ-НА%20ОБНОВЛЕНИЯ-brightgreen.svg?style=social&logo=telegram&color=blue)](https://t.me/GyverLibs)

| ️ ️ ️ ️ ️ ️ ️ ️ ️ ️ ️<br><br>**I have a new library.[uPID](https://github.com/GyverLibs/uPID)It’s lighter, faster and has more algorithm tweaks and modifications!**<br><br> ️ ️ ️ ️ ️ ️ ️ ️ ️ ️|
| --- |

# GyverPID
GyverPID - the PID regulator library for Arduino
- Time of one calculation about 70 μs
- Mode of operation by magnitude or by its change (for integrating processes)
- Returns the result on the built-in timer or in manual mode
- Built-in coefficient calibrators
- Error and Measurement Error mode
- Integrated sum optimizers

### Compatibility
Compatible with all Arduino platforms (Arduino features are used)

### Documentation.
There's a library[extended documentation](https://alexgyver.ru/GyverPID/)

## Contents
- [Installation](#install)
- [Initialization](#init)
- [Use of use](#usage)
- [Example](#example)
- [Versions](#versions)
- [Bugs and feedback](#feedback)

<a id="install"></a>
## Installation
- The library can be found under the name **GyverPID** and installed through the library manager in:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Download the library](https://github.com/GyverLibs/GyverPID/archive/refs/heads/main.zip).zip archive for manual installation:
    - Unpack and put in *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Unpack and put in *C:\Program Files\Arduino\libraries* (Windows x32)
    - Unpack and put in *Documents/Arduino/libraries/ *
    - (Arduino IDE) Automatic installation from .zip: *Sketch/Connect library/Add .ZIP library...* and specify downloaded archive
- Read more detailed instructions for installing libraries[here](https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)
### Update
- I recommend always updating the library: new versions fix errors and bugs, as well as optimize and add new features.
- Through the library manager IDE: find the library as when installing and click "Update"
- Manually: **Delete the folder with the old version** and then put the new one in its place. “Replacement” can not be done: sometimes new versions delete files that will remain when replaced and can lead to errors!

<a id="init"></a>
## Initialization
```cpp
// An object can be initialized in three ways:
GyverPID regulator;                 // initialize without settings (all zero, dt 100 ms)
GyverPID regulator(kp, ki, kd);     // initialize with coefficients. dt will be standard 100 ms
GyverPID regulator(kp, ki, kd, dt); // initialize with coefficients and dt (in milliseconds)
```

<a id="usage"></a>
## Use of use
Look.[documentation](https://alexgyver.ru/GyverPID/)
```cpp
datatype setpoint = 0;     // the amount that the regulator should support
datatype input = 0;        // signal from a sensor (for example, the temperature we adjust)
datatype output = 0;       // exit from the regulator to the control device (for example, the PWM value or the angle of rotation of the servo)
  
datatype getResult();      // returns a new value when you call (if you use your timer with dt!)
datatype getResultTimer(); // returns the new value no earlier than after dt milliseconds (built-in timer with period dt)
void setDirection(boolean direction);    // Regulatory direction: NORMAL (0) or REVERSE (1)
void setMode(boolean mode);              // ON ERROR (0) or ON RATE (1)
void setLimits(int min_output, int max_output);    // limit of output value (for example, for PWM set 0-255)
void setDt(int16_t new_dt);              // setting the sampling time (for getResultTimer)
float Kp = 0.0;
float Ki = 0.0;
float Kd = 0.0;
```

<a id="example"></a>
## Example
For more examples see **examples**!
```cpp
/*
   Пример работы ПИД регулятора в автоматическом режиме по встроенному таймеру
   Давайте представим, что на 3 пине у нас спираль нагрева, подключенная через мосфет,
   управляем ШИМ сигналом
   И есть какой то абстрактный датчик температуры, на который влияет спираль
*/

// Before connecting the library, you can add settings:
// It will make some of the computations integer, which will speed up the code a little (just a little!).
// #define PID_INTEGER

// mode in which the integral component is summed only within a specified number of values
// #define PID_INTEGRAL_WINDOW 50
#include "GyverPID.h"

GyverPID regulator(0.1, 0.05, 0.01, 10);  // Coef. Uh, coef. And, coef. E, sampling period dt (ms)
// or so:
// GyverPID regulator(0.1, 0.05, 0.01); // can P, I, D, without dt, dt will be silent. 100 ms.

void setup() {
  regulator.setDirection(NORMAL); // Regulatory direction (NORMAL/REVERSE) By omission, NORMAL stands.
  regulator.setLimits(0, 255);    // Limits (set for 8 bit PWM). By omission stand 0 and 255
  regulator.setpoint = 50;        // Tell the regulator the temperature it needs to maintain.

  // Coefficients can be changed in the process of work
  regulator.Kp = 5.2;
  regulator.Ki += 0.5;
  regulator.Kd = 0;
}

void loop() {
  int temp;                 // temperature reading
  regulator.input = temp;   // Notify the regulator of the current temperature

  // getResultTimer returns the value to the control device
  // (Once called, you can get this value as regulator.output)
  // Update on the built-in timer on millis()
  analogWrite(3, regulator.getResultTimer());  // send out

  // .getResultTimer() essentially returns regulator.output
}
```

<a id="versions"></a>
## Versions
- v1.1 - defiles removed
- v1.2 - defiles returned
- v1.3 - Accelerated calculations, library facilitated
- v2.0 - the logic of the work is slightly rethought, the code is improved, simplified and lightened
- v2.1 - Integral made public
- v2.2 - optimization of calculations
- v2.3 - Added PID INTEGRAL WINDOW mode
- v2.4 - implementation entered in class
- v3.0
    - Integral component optimization mode is added (see dock)
    - Automatic coefficient calibrators added (see examples and dock)
- v3.1 - corrected by ON RATE mode, added auto-limiting integer amount
- v3.2 - a little optimization, added getResultNow
- v3.3 - in tuners you can transfer another Stream class handler for debugging

<a id="feedback"></a>
## Bugs and feedback
If you find bugs, create **Issue**, or better write to the mail immediately.[alex@alexgyver.ru](mailto:alex@alexgyver.ru)  
The library is open for revision and your **Pull Requests*!

When reporting bugs or incorrect work of the library, it is necessary to specify:
- Library version
- What is used by the IC
- SDK version (for ESP)
- Arduino IDE version
- Are embedded examples that use features and designs that cause bugs in your code working correctly?
- What code was downloaded, what work was expected from it and how it works in reality
- Ideally, attach the minimum code in which the bug is observed. Not a canvas of a thousand lines, but a minimum code.
