reprap_modular_electronics
==========================


Licence
-------
reprap modular electronics by Peter Theobald is licensed under a
Creative Commons Attribution-NonCommercial-ShareAlike 3.0 Unported
License.
Based on a work at
https://github.com/pete-theobald/reprap_modular_electronics.


Overview
--------
This project is intended to address a common issue with reprap
electronics, boards are monolithic, high cost and support a
predetermined set of components. Adding more motors, extruders, fans etc
than supported by your board needs a full upgrade.

This project intends to build a series of modules that can be
connected in parrallel with each unit able to control exactly one
stepper motor, 2x endstops, 1x heated device (extruder or bed) and one
fan. The modules will be as low cost as possible and be designed to
offload as much processing onto a host computer as possible. They will
still require a small controller module which will be tasked with
ensuring that stepper motors are controlled to a specific time base.

Details of modules
------------------

MCU
---
This project uses a low cost microchip pic MCU as its main processor.
The chip is available for around £1 and programmers are easily
available. This chip could easily be swapped for an arduino compatible 
chip. The chip is responsible for running PWM control loops (x2) and
listening for stepper commands on the bus and passing them to the
stepper driver.

Addressing
----------
The modules address can be set using 8 jumpers. The values are read by
the pic using the internal weak pullups meaning a closed jumper is read
as zero. The value on the port should be inverted so a closed jumper 
reads as a one. The jumpers are intended to be closed with solder to
keep costs low.
The MCU will use its UART to listen for messages. The RX pin is
connected to a bus. No bus drivers are included as the distance between
modules is intended to be small. The MCU will expect its address to be
sent in 9 bit format, i.e. 9th bit indicates an address byte.
The host controller software is responsible for mapping board addresses
to individual motors etc.

Stepper driver
--------------
The stepper module uses an Allegro A4982 stepper driver. All digital
pins on the chip are accessible to the MCU. The stepper driver will be
controlled by passing an absolute intended position for the stepper.
The stepper driver is intended to be fixed to a 1inch square heatsink.

Heater
------
The board contains a power mosfet (n channel) and a thermistor
PIDconnection. The MCU will run a pid control loop to regulate the
temperature. The PID constants and target will be setup remotely,
however there is currently no provision for monitoring the temperature
remotely.

End stops
---------
Connectors are included for end stops containing gnd, 5v and signal
lines. The end stop needs to be able to provide a digital output.

Fan
---
A fan header is included that can support 2, 3 and 4 pin pc fans.
The MCU can control the fan using a PWM signal. A jumper allows the pwm
signal to control either a mosfet on the 12v connector or directly to
the 4th pin for 4 pin fan connectors.
The possible states of the jumper are as follows
* No jumper - Continuous voltage supplied
* 2/3pin mode - Voltage controlled by PWM signal
* 4pin mode - PWM connected to pwm pin. Continuous voltage supplied


Components
----------
Below list shows prices for individual components in small quantitites

Part     Value          Device         Package           Library                 Sheet

ADDRESS  8WAYJUMPER     8WAYJUMPER     8WAYJUMPER        ics                     1		n/a
BUS                     PINHD-1X3/90   1X03/90           pinhead                 1		M22-2035005 50way right angle header £1.30 @10
C1                      C-EUC0603K     C0603K            rcl                     1		
C2                      C-EUC0603K     C0603K            rcl                     1
C3                      C-EUC0603K     C0603K            rcl                     1
C4                      C-EUC0603K     C0603K            rcl                     1
C5                      C-EUC0603K     C0603K            rcl                     1
C6                      CPOL-EUE2-5    E2-5              resistor                1
C7                      C-EUC0603K     C0603K            rcl                     1
EXTRUDE                 PINHD-1X4/90   1X04/90           pinhead                 1		M22-2035005 50way right angle header £1.30 @10
FAN                     PINHD-1X4/90   1X04/90           pinhead                 1		M22-2035005 50way right angle header £1.30 @10
IC1      AD820R         AD8541R        SO8               analog-devices          1		£0.60 @10
JP3                     PINHD-1X6/90   1X06/90           pinhead                 1
JP4                     JP2E           JP2               jumper                  1
MOTOR                   PINHD-1X4/90   1X04/90           pinhead                 1		M22-2035005 50way right angle header £1.30 @10
POWER                   PINHD-1X6/90   1X06/90           pinhead                 1		M22-2035005 50way right angle header £1.30 @10
Q3       FDN360P        FDN360P        SUPER-SOT3        transistor-power        1
Q4       BSS123         BSS123         SOT23             transistor-small-signal 1
R1                      R-EU_R0603     R0603             rcl                     1
R2                      R-EU_R0805     R0805             resistor                1
R3                      R-EU_R0805     R0805             resistor                1
R4                      R-EU_R0603     R0603             resistor                1
R5                      R-EU_R0603     R0603             rcl                     1
R6                      R-EU_R0603     R0603             rcl                     1
R7                      R-EU_R0603     R0603             rcl                     1
R8                      R-EU_R0603     R0603             rcl                     1
R9                      R-EU_R0603     R0603             rcl                     1
R10                     R-EU_R0603     R0603             rcl                     1
STOP+                   PINHD-1X3/90   1X03/90           pinhead                 1		M22-2035005 50way right angle header £1.30 @10
STOP-                   PINHD-1X3/90   1X03/90           pinhead                 1		M22-2035005 50way right angle header £1.30 @10
TX                      PINHD-1X1      1X01              pinhead                 1		M22-2035005 50way right angle header £1.30 @10
U$1      A4982          A4982          TSSOP24           ics                     1		£2.86 @10
U$2      PIC16F722-I/SS PIC16F722-I/SS SOP65P780X200-28N ics                     1		£0.96 @10
U$3      NMOSFET_DPACK  NMOSFET_DPACK  D-PAK_TO252AA     ics                     1		IPD170N04N G £0.29 @1



















