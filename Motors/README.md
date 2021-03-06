# **Raspberry Pi Servo Motor control**

In addition to stepper motors, there are also small and cheap servo motors. The control of Raspberry Pi servo motors is very simple and thanks to the small size and weight they are used in many areas.
Unlike stepper motors, servomotors can be controlled with a single GPIO.

In this tutorial, I’ll show an example of how to use Python to control one or more servo motors.

Required Hardware Parts

    Servo motor*
    Jumper cable*
    (Breadboard*)
    if necessary, a battery holder* + (rechargeable) batteries*, as an external power supply

Of course, it is possible to supply the servo motor with an external power source, but this only makes sense when using several motors. For that, rechargeable batteries*/regular batteries would still be needed.

## Setup

In most cases, the colors of the servo are as follows and are connected to the Pi:

BROWN/BLACK – comes to GND (pin 6) from the Pi
RED – comes to 3V3 (pin 1) from the Pi
YELLOW/ORANGE – to a free GPIO pin (e.g., GPIO17, pin 11)

If you want to play it safe, you can set a ~ 1kΩ resistor between the data pin (yellow/orange) and the Pi. Normally this is not necessary.

If the servo motor does not rotate correctly, this may also influence the power supply of the Raspberry Pi (just look at the datasheet, what the engine consumes). In such a case an external power source makes sense (usually it is 4 to 6V).

## Software for controlling the Raspberry Pi servo motors

Unlike stepper motors, servo motors don’t occupy many GPIO pins to command a movement. For this, the rotation is controlled by the length of the pulse.

The angle of the motor is set along the length of the pulse, so PWM is particularly useful, which sends repetitive signals at even intervals (the Raspberry Pi Python library must be installed).

We either start python (sudo python) or open a new script (sudo nano servomotor.py) with the following content:


```python
import RPi.GPIO as GPIO
from gpiozero import Servo
from time import sleep

servo = Servo(17)

while True:
  servo.mid()
  print("mid")
  servo.min()
  print("min")
  sleep(1)
  servo.mid()
  print("max")
  sleep(1)
```

**Or with This Code:**

```python
import RPi.GPIO as GPIO
import time

servoPIN = 17
GPIO.setmode(GPIO.BCM)
GPIO.setup(servoPIN, GPIO.OUT)

p = GPIO.PWM(servoPIN, 50) # GPIO 17 for PWM with 50Hz
p.start(2.5) # Initialization
try:
  while True:
    p.ChangeDutyCycle(5)
    time.sleep(0.5)
    p.ChangeDutyCycle(7.5)
    time.sleep(0.5)
    p.ChangeDutyCycle(10)
    time.sleep(0.5)
    p.ChangeDutyCycle(12.5)
    time.sleep(0.5)
    p.ChangeDutyCycle(10)
    time.sleep(0.5)
    p.ChangeDutyCycle(7.5)
    time.sleep(0.5)
    p.ChangeDutyCycle(5)
    time.sleep(0.5)
    p.ChangeDutyCycle(2.5)
    time.sleep(0.5)
except KeyboardInterrupt:
  p.stop()
  GPIO.cleanup()
```


### Note:
If the servo motor shakes a bit while it is not moving, you can pause the pulse with ChangeDutyCycle(0)
