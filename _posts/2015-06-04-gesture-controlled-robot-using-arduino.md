---
layout: post
title:  "Gesture Controlled Robot Using Arduino"
date:   2015-06-04 22:22:32 +0530
permalink: /gesture-controlled-robot-using-arduino/
---
This gesture controlled robot uses Arduino,ADXL335 accelerometer and RF transmitter-receiver pair.

We will divide the entire robot into 3 parts the transmitter,the receiver and the robot.

The different gestures that have been mapped to the direction of the bot are-

Hand parallel to the ground-stationary

Hand tilted forward-forward

Hand tilted backward-backward

Hand tilted right-right

Hand tilted left-left

I've made the transmitter on thermocol though it can also be made on a glove.

All the code, schematics and other pictures in one place are available [here][1].

### Step 1: Materials Required

For transmitter-

1.  Arduino Uno
2.  ADXL335 accelerometer
3.  433 MHz RF transmitter
4.  Breadboard

For receiver and robot-

1.  Arduino Uno
2.  433 MHz RF receiver
3.  L293D motor driver IC
4.  Chassis and wheels
5.  2 DC motors
6.  Breadboard

Of course you will also need jumper wires and 9V batteries

Instead of using the Arduino and breadboard in transmitter like I did ,you may instead use an ATMega328p, which can be programmed from the Arduino board and solder it along with RF transmitter and ADXL335 on a perfboard.

The perfboard can then be attached to a glove.However here I've used a remote controller like setup with the gestures the same

### Step 2: Assembling the Robot

![](/assets/uploads/2018/12/gesture-robot-7-300x166.jpg)

Fix the wheels on the chassis.

Mount the DC motors on the back wheels and use dummy wheels for the front.

Mount the L293D IC on the breadboard and place it on the chassis

Place the Arduino on the chassis and make the connections of L293D as follows

4,5,12,13 to GND

1,9,16 to VCC(5V)

3,6 to left motor(output)

11,14 to right motor(output)

2,7,10,15 to pins 8,9,10,7 of Arduino(inputs)

8 to 9V battery

### Step 3: Determining the Direction of Robot

You can learn more about L293D from internet.

Basically ,the motor rotates when the inputs supplied are opposite.

For example high,low may rotate the motor in clockwise while low,high in anti clockwise.

If both inputs are same then motor does not rotate.

The sketch in test.ino will help to determine for what inputs for the 2 motors will the robot move forward. Copy and paste the code in test.ino it in Arduino IDE

In my case it was observed that the bot will move forward pin 9 of Arduino is high,pin 8 is low(for left motor),pin 10 is high,pin 7 is low(for right motor).Try different combinations till you get desired direction. Similarly for moving back the combination is high,low,low,high.The bot will go right if left motor is moving and right is stopped by giving same inputs.Similarly for left.

### Step 4: Interfacing ADXL335 With Arduino

![](/assets/uploads/2018/12/gesture-robot-8-300x211.jpg)

Mount the ADXL335 and on the breadboard.

The connections to Arduino should be as follows.The Arduino should be different from the one used in step 2

ADXL335 ARDUINO

VCC 3.3 V

GND GND

X A0

Y A1

Z open

ST open

Now copy and paste the code in adxl335interface.ino and determine the threshold values for different gestures.

The code gives 2 values xval and yval which will have unique values for different gestures.

Determine the range of values of xval and yval when the hand is tilted forward,backward etc.

### Step 5: Interfacing RF Transmitter With Arduino

![](/assets/uploads/2018/12/gesture-robot-9-300x219.jpg)

![](/assets/uploads/2018/12/gesture-robot-10-300x300.jpg)

Mount the RF transmitter on the breadboard in previous step and make connections as follows.

RF transmitter Arduino

GND GND

DATA D12

VCC 5V

Now download the VirtualWire library from the following link

[Link][2]

Extract the VirtualWire folder from the downloaded folder and paste it in arduino-1.6.1>libraries

Now program the arduino of the transmitter with the code given in transmitter.ino.

Basically what the code does is to map the different threshold values (for gestures) obtained in step 4 to different letters (stationary-'s' forward -'f' etc) which are then transmitted through the RF transmitter.

This step completes the construction of the transmitter

### Step 6: The Receiver

![](/assets/uploads/2018/12/gesture-robot-11-300x196.jpg)

![](/assets/uploads/2018/12/gesture-robot-12-300x139.jpg)

![](/assets/uploads/2018/12/gesture-robot-13-300x300.jpg)

Mount the RF receiver on the breadboard of step 2.The connections to the Arduino used in step 2 are

RF receiver Arduino

VCC 5V

DATA D11

GND GND

Now program the Arduino with the code given in receiver.ino.

The code maps the different letters obtained from the receiver to the inputs for directions.

For instance if the receiver receives the letter "f" corresponding to the bot moving forward,it maps the letter "f" with the inputs low,high,high,low which are the required inputs for the bot to move forward.

### Step 7: Run the Robot

Go to [this repository][1] to download the code and schematics.Click on the "Clone or Download" button (green in color on the right side) and select "Download ZIP" to download the zip file.Now extract the contents on your computer to get the code and schematics(in the schematics folder).

The gesture controlled robot is now complete.

Connect 9V batteries to both the Arduinos and the L293D motor supply and run the robot.

I've also added functionality by which the on board led on pin 13 of both the Arduinos is on when the bot is moving and off when it's stationary.You may add more such functionality.

Any doubts and connections be clarified by seeing the comments in the given code.

I've posted a video of the robot [here][3].

 [1]: https://github.com/khansubhan95/instructables/tree/master/gesture_robot
 [2]: http://www.pjrc.com/teensy/td_libs_VirtualWire.html
 [3]: https://www.youtube.com/watch?v=2Rc2l9QYKBM

