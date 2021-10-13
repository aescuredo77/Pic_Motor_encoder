# Pic_Motor_encoder
I2C controlled servo
![](./Electronics/pictures/detail_servo_00.png)
 
 **ServoMotor detail of the interior circuit** 

## Description ##
The servomotor consists of a controller board with a PIC16f1503 and a LB1938FA driver, which drive a Pololu &quot;Micro-metal-gearmotors&quot; type reduction motor. The reduction of the motors is based on the need for example:

- 100: 1, 130 rpm
- 298:1, 45 rpm

The microcontroller communicates through I2C, which allows us to connect 127 nodes in series. There will not be so many in our applications, we have asked for LP motors, Low power because they have a low consumption that allows us to connect them in series.

The I2c address will depend on the application that we are going to make, it should not be a problem to change it depending on the needs, but we are going to give a few premises. The I2C addresses are defined by 7 bits, the least significant bit tells us if it is writing or reading, so we will give a couple of addresses, the first is the one recorded in the pic, and the other is the one we will use in the Arduino / teensy to communicate. Always in Hexadecinal.


| Name            | Mode     | address | address Arduino |
| --------------- | -------- | --------| ----------------|
|Servo_Motor_Left |   speed  |   0x22  |       0x11      |
|Servo_Motor_Rigt |   speed  |   0x20  |       0x10      |
|Gripper          | position |   0x10  |       0x08      |
|Servo_base       | position |   0x30  |       0x18      |
|Servo_left       | position |   0x32  |       0x19      |
|Servo_right      | position |   0x34  |       0x1A      |


The 12 pulses per turn encoder must be multiplied by the reduction of the motor to obtain the pulses per turn that the motor will give.For example in the case of reduction 100:1 the servoMotor needs 1200 pulses to complete a lap.

## Boards and Schematics ##

![](./Electronics/pictures/board_0.png)
**ServoMotor board in Eagle**

![](./Electronics/pictures/sch_0.png)
**ServoMotor schematic in Eagle** 

## Electronics  

[PIC16F1503](http://ww1.microchip.com/downloads/en/DeviceDoc/40001607D.pdf) 14 pin Flash, 8 bit microcontroller, 16Mhz internal Oscillator, 2Kwords linear program, 128 data SDRAM, 8 Analog to Digital converter at 10 bit resolution, 4 PWM, 3 Timers, I2C/SPI.

![](./Electronics/pictures/pic16f1503.png)



![](./Electronics/pictures/pic16f1503_b.png)



The driver is [LB1938FA](https://www.dropbox.com/s/l5har1ai8nknbxs/LB1938FA.pdf?dl=0), It is a driver configured in H-bridge that allows us to control the motor in speed and direction with only two inputs. Analyzing the driver that the DFRobot servomotors carry, the L9110S looks for alternatives because the aforementioned driver is a little out of print or is only sold on pages of doubtful reliability.

 ![](./roseco/tree/main/Electronics/pictures/detail_driver_00.png)

 **LB1938FA is the cousin of the L9110S carried by Dfrobotics servos.** 

#### The inputs IN1 and IN2 of the driver are the RC5 and RC3 outputs of the PIC16f1503 respectively that can operate as PWM1 and PWM2.

####

### Software ###

 ![](./Electronics/pictures/detail_servo_01.png)
 
 **Detail of the motor control board** 

The same board can serve different purposes, for example, in the robotic arm we need positioning and hold, while the Eco\_robot we are interested in speed. For this reason and everything that the program is going to be very similar we will have to make different versions. Or group them all in one, although I am afraid that the memory of the PI16f1503 (2Kwords) is not going to give that much.



![](./Electronics/pictures/boards.jpg)

If you need more powerful motors or those that require a greater workload, there is a version of these servos with an independent power supply from the motors.
