This is an automatic translation, may be incorrect in some places. See sources and examples!

# Gyverpid
Gyverpid - PID Library for Arduino
- The time of one calculation is about 70 μs
- mode of operation by value or by its change (for integrating processes)
- returns the result by built -in timer or manually
- Built -in calibrators of coefficients
- mode of operation by mistake and by error of measurement
- built -in optimizers of integral sum

## compatibility
Compatible with all arduino platforms (used arduino functions)

### Documentation
There is [extended documentation] to the library (https://alexgyver.ru/gyverpid/)

## Content
- [installation] (# Install)
- [initialization] (#init)
- [use] (#usage)
- [Example] (# Example)
- [versions] (#varsions)
- [bugs and feedback] (#fedback)

<a id="install"> </a>
## Installation
- The library can be found by the name ** gyverpid ** and installed through the library manager in:
    - Arduino ide
    - Arduino ide v2
    - Platformio
- [download the library] (https://github.com/gyverlibs/gyverpid/archive/refs/heads/main.zip) .Zip archive for manual installation:
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
// You can initialize the object in three ways:
Gyverpid Regulator;// initialize without settings (all by zero, DT 100 ms)
Gyverpid Regulator (KP, Ki, KD);// initialize with coefficients.DT will be standard 100 ms
Gyverpid Regulator (KP, Ki, KD, DT);// initialize with coefficients and DT (in milliseconds)
`` `

<a id="usage"> </a>
## Usage
See [documentation] (https://alexgyver.ru/gyverpid/)
`` `CPP
Datatype setpoint = 0;// a given value that the regulator should support
Datatype Input = 0;// signal from the sensor (for example, the temperature that we adjust)
DATATYPE OUTPUT = 0;// Exit from the regulator to the control device (for example, the size of the PWM or the angle of rotation servo)
  
Datatype Getressult ();// returns a new value when calling (if we use our timer with a period DT!)
Datatype Getressulttimer ();// returns a new value no earlier than through DT milliseconds (built -in timer with a DT period)
VOID Setdirection (Boolean Direction);// Direction of Regulation: Normal (0) or Reverse (1)
vCranberries OID setmode (Boolean Mode);// mode: Work on the input error on_error (0) or on change on_rate (1)
VOID setlimits (int min_outPut, int max_outPut);// Limit output (for example, for Shim, put 0-255)
VOID setdt (int16_t new_dt);// Installation of sampling time (for Getressulttimer)
Float kp = 0.0;
Float Ki = 0.0;
Float KD = 0.0;
`` `

<a id="EXAMPLE"> </a>
## Example
The rest of the examples look at ** Examples **!
`` `CPP
/*
   An example of the work of the controller PID in automatic mode according to the built -in timer
   Let's imagine that on 3 pin we have a heating spiral connected via Mosfet,
   We control the PWM signal
   And there is some kind of abstract temperature sensor on which the spiral affects
*/

// Before connecting the library, you can add settings:
// will make part of the calculations integer, which a little (just a little!) will accelerate the code
// #define pid_integer

// mode in which the integral component is summarized only within the specified number of values
// #define pid_integral_window 50
#include "gyverpid.h"

Gyverpid Regulator (0.1, 0.05, 0.01, 10);// coeff.P, coeff.And, coeff.D, DT sampling period (MS)
// or so:
// gyverpid regulator (0.1, 0.05, 0.01);// You can, and, d, without dt, dt will be a silence.100 ms

VOID setup () {
  regulator.setdirection (Normal);// Direction of regulation (Normal/Reverse).The default is Normal
  regulator.setlimits (0, 255);// limits (set for 8 bit PWM).The default cost 0 and 255
  regulator.setpoint = 50;// inform the regulator of the temperature that he must support

  // In the process of work, you can change the coefficients
  regulator.kp = 5.2;
  regulator.ki += 0.5;
  regulator.kd = 0;
}

VOID loop () {
  Int TEMP;// read the temperature from the sensor
  regulator.input = TEMP;// inform the regulator the current temperature

  // Getressulttimer returns the value for the control device
  // (after a call, you can get this value as regulator.Output)
  // update takes place according to the built -in timer on millis ()
  analogwrite (3, regulator.getresulttimer ());// Send to Mosfet

  // .Getressulttimer () essentially returns regulator.Output
}
`` `

<a id="versions"> </a>
## versions
- V1.1 - Defaine removed
- v1.2 - Defaines returned
- v1.3 - calculations are accelerated, the library is facilitated
- v2.0 - the logic of work is slightly rethought, the code is improved, simplified and facilitated
- V2.1 - Integral is taken to Public
- V2.2 - Optimization of calculations
- V2.3 - Added PID_INTEGRAL_Window mode
- v2.4 - implementation is included in the class
- V3.0
    - added the optimization mode of the integral component (see Dok)
    - Added automatic calibers of coefficients (see examples and documents)
- V3.1 - Fixed by the ON_Rate mode, added by int.sums
- V3.2 - A little optimization, added Getressultnow
- V3.3 - In tuners, you can transfer another STREAM class processor for debugging

<a id="feedback"> </a>
## bugs and feedback
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