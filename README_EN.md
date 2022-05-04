This is an automatic translation, may be incorrect in some places. See sources and examples!

# GyverPID
GyverPID - PID controller library for Arduino
- The time of one calculation is about 70 µs
- Mode of operation by value or by its change (for integrating processes)
- Returns the result by the built-in timer or in manual mode
- Built-in ratio calibrators
- Operating mode by error and measurement error
- Built-in integral sum optimizers

### Compatibility
Compatible with all Arduino platforms (using Arduino functions)

### Documentation
The library has [extended documentation](https://alexgyver.ru/GyverPID/)

## Content
- [Install](#install)
- [Initialization](#init)
- [Usage](#usage)
- [Example](#example)
- [Versions](#versions)
- [Bugs and feedback](#feedback)

<a id="install"></a>
## Installation
- The library can be found by the name **GyverPID** and installed through the library manager in:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Download library](https://github.com/GyverLibs/GyverPID/archive/refs/heads/main.zip) .zip archive for manual installation:
    - Unzip and put in *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Unzip and put in *C:\Program Files\Arduino\libraries* (Windows x32)
    - Unpack and put in *Documents/Arduino/libraries/*
    - (Arduino IDE) automatic installation from .zip: *Sketch/Include library/Add .ZIP library…* and specify the downloaded archive
- Read Bolher detailed instructions for installing libraries [here] %B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)

<a id="init"></a>
## Initialization
```cpp
// You can initialize an object in three ways:
GyverPID regulator; // initialize without settings (all zeros, dt 100 ms)
GyverPID regulator(kp, ki, kd); // initialize with coefficients. dt will be standard 100ms
GyverPID regulator(kp, ki, kd, dt); // initialize with coefficients and dt (in milliseconds)
```

<a id="usage"></a>
## Usage
See [documentation](https://alexgyver.ru/GyverPID/)
```cpp
datatype setpoint = 0; // set value that the controller must support
datatype input = 0; // signal from the sensor (for example, the temperature that we regulate)
data type output = 0; // output from the controller to the control device (for example, PWM value or servo rotation angle)
  
datatype getResult(); // returns a new value when called (if we use our own timer with a period of dt!)
datatype getResultTimer(); // returns a new value no earlier than after dt milliseconds (built-in timer with a period of dt)
void setDirection(boolean direction); // regulation direction: NORMAL (0) or REVERSE (1)
void setMode(boolean mode); // mode: work on input error ON_ERROR (0) or change ON_RATE (1)
void setLimits(int min_output, int max_output); // output value limit (for example, for PWM we set 0-255)
void setDt(int16_t new_dt); // set sampling time (for getResultTimer)
float Kp = 0.0;
float Ki = 0.0;
float Kd = 0.0;
```

<a id="example"></a>
## Example
See **examples** for other examples!
```cpp
/*
   An example of the operation of the PID controller in automatic mode using the built-in timer
   Let's imagine that on pin 3 we have a heating coil connected via a mosfet,
   control PWM signal
   And there is someabstract temperature gauge influenced by spiral
*/

// before connecting the library, you can add settings:
// make part of the calculations integer, which will slightly (just a little!) speed up the code
// #define PID_INTEGER

// mode in which the integral component is summed only within the specified number of values
// #define PID_INTEGRAL_WINDOW 50
#include "GyverPID.h"

GyverPID regulator(0.1, 0.05, 0.01, 10); // coefficient P, coefficient And, coefficient. D, sampling period dt (ms)
// or like this:
// GyverPID regulator(0.1, 0.05, 0.01); // you can P, I, D, without dt, dt will be by default. 100ms

void setup() {
  regulator.setDirection(NORMAL); // regulation direction (NORMAL/REVERSE). DEFAULT IS NORMAL
  regulator.setLimits(0, 255); // limits (set for 8 bit PWM). DEFAULT IS 0 AND 255
  regulator.setpoint = 50; // tell the controller the temperature it should maintain

  // in the process of work, you can change the coefficients
  regulator.Kp = 5.2;
  regulator.Ki += 0.5;
  regulator.Kd = 0;
}

void loop() {
  inttemp; // read the temperature from the sensor
  regulator.input = temp; // tell the controller the current temperature

  // getResultTimer returns the value for the control device
  // (after the call, you can get this value as regulator.output)
  // update occurs on the built-in timer on millis ()
  analogWrite(3, regulator.getResultTimer()); // send to mosfet

  // .getResultTimer() essentially returns regulator.output
}
```

<a id="versions"></a>
## Versions
- v1.1 - defines removed
- v1.2 - defaults returned
- v1.3 - calculations are accelerated, the library is lightened
- v2.0 - the logic of work is slightly rethought, the code is improved, simplified and lightened
- v2.1 - integral moved to public
- v2.2 - calculation optimization
- v2.3 - added PID_INTEGRAL_WINDOW mode
- v2.4 - implementation added to the class
- v3.0
    - Added integral component optimization mode (see doc)
    - Added automatic calibratorscoefficients (see examples and doc)
- v3.1 - fixed ON_RATE mode, added auto limit int. amounts
- v3.2 - a bit of optimization, getResultNow added
- v3.3 - in tuners, you can pass another Stream class handler for debugging

<a id="feedback"></a>
## Bugs and feedback
When you find bugs, create an **Issue**, or better, immediately write to the mail [alex@alexgyver.ru](mailto:alex@alexgyver.ru)
The library is open for revision and your **Pull Request**'s!