# Data Logger (and using cool sensors!)

*A lab report by Alana C*

## In The Report

For this lab, we will be experimenting with a variety of sensors, sending the data to the Arduino serial monitor, writing data to the EEPROM of the Arduino, and then playing the data back.

## Part A.  Writing to the Serial Monitor
 
**a. Based on the readings from the serial monitor, what is the range of the analog values being read?**
0-1023
 
**b. How many bits of resolution does the analog to digital converter (ADC) on the Arduino have?**
10 (2^10 = 1024)

## Part B. RGB LED

**How might you use this with only the parts in your kit? Show us your solution.**
Wire the RGB LED such that three different digital arduino pins are connected to each of the three red, green, and blue pins, adding a resistor in series to each of the three R G B pins and then wiring these to ground, and the common anode is wired to power (5V in this case).

## Part C. Voltage Varying Sensors 
 
### 1. FSR, Flex Sensor, Photo cell, Softpot

**a. What voltage values do you see from your force sensor?**
(used 10k resistor)
~0-1000

**b. What kind of relationship does the voltage have as a function of the force applied? (e.g., linear?)**
It appears logarithmic such that the higher voltage values are more difficult to increment, whereas it is much easier to increase the voltage values at lower pressures

**c. Can you change the LED fading code values so that you get the full range of output voltages from the LED when using your FSR?**
Yes: int yTemp = analogRead(A0);
     **int y = map(yTemp,0,1023,0,255);**
     setColor(y,0,0);
     
**d. What resistance do you need to have in series to get a reasonable range of voltages from each sensor?**
photo cell: 10k
softspot: 10k

**e. What kind of relationship does the resistance have as a function of stimulus? (e.g., linear?)**
photo cell: linear
softspot: linear

### 2. Accelerometer
 
**a. Include your accelerometer read-out code in your write-up.**
![Accelerometer](https://github.com/AlanaCrognale/IDD-Fa19-Lab3/blob/master/accelerometer.ino)

### 3. IR Proximity Sensor

**a. Describe the voltage change over the sensing range of the sensor. A sketch of voltage vs. distance would work also. Does it match up with what you expect from the datasheet?**

**b. Upload your merged code to your lab report repository and link to it here.**

## Optional. Graphic Display

**Take a picture of your screen working insert it here!**

![Graphic_Display](https://github.com/AlanaCrognale/IDD-Fa19-Lab3/blob/master/IMG_0626.jpeg)
## Part D. Logging values to the EEPROM and reading them back
 
### 1. Reading and writing values to the Arduino EEPROM

**a. Does it matter what actions are assigned to which state? Why?**
Yes - in this case, only certain states can be reached from certain states, i.e. can only go from 0->1, 1->2, 1->0, or 2->1, so in this structure, you cannot jump from state 0 to 2 or vice versa. So, we wouldn't want to switch around which actions are going to which states because we want to be able to only go from clear->write->read or read->write->clear, not clear->read or read->clear.

**b. Why is the code here all in the setup() functions and not in the loop() functions?**
We only want the setup() action to occur once per potentiometer turn.  If this portion of the code were in loop(), we would be constantly restarting the reading/writing/clearing process without finishing, which could cause issues in storage.

**c. How many byte-sized data samples can you store on the Atmega328?**
1024

**d. How would you get analog data from the Arduino analog pins to be byte-sized? How about analog data from the I2C devices?**
Since integer analog data is larger than one byte, you could convert the number to a char and map each number to a different ASCII value.  For the I2C accelerometer, you could do the same but would first need to map the accelerometer data from 0-1023 using the map() function, or you could map the original accelerometer data to a larger set of ASCII values.

**e. Alternately, how would we store the data if it were bigger than a byte? (hint: take a look at the [EEPROMPut](https://www.arduino.cc/en/Reference/EEPROMPut) example)**
Using eeprom.put(), store data the same as if you were using eeprom.write() except make sure to increment the address every time to one index past the size of the previous data you stored, as opposed to incrementing the address one index every time as in eeprom.write() (one address stores one byte).

**Upload your modified code that takes in analog values from your sensors and prints them back out to the Arduino Serial Monitor.**
![Modified](https://github.com/AlanaCrognale/IDD-Fa19-Lab3/blob/master/SwitchState2.zip)

### 2. Design your logger
 
**a. Insert here a copy of your final state diagram.**
![State Diagram](https://github.com/AlanaCrognale/IDD-Fa19-Lab3/blob/master/attentionDiagram.jpeg)

### 3. Create your data logger!
 
**a. Record and upload a short demo video of your logger in action.**
![Attention meter_perfect](https://github.com/AlanaCrognale/IDD-Fa19-Lab3/blob/master/attention_perfect.mov)
![Attention meter_too_much](https://github.com/AlanaCrognale/IDD-Fa19-Lab3/blob/master/attention_too_much.mov)
![Attention meter_not_enough](https://github.com/AlanaCrognale/IDD-Fa19-Lab3/blob/master/attention_not_enough.mov)
![Code](https://github.com/AlanaCrognale/IDD-Fa19-Lab3/blob/master/DataLogger.zip)
