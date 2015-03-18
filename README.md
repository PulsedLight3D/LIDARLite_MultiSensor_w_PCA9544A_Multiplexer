# LIDAR-Lite MultiSensor w/ PCA9544A I2C Multiplexer

This sketch demonstrates getting distance from multiple LIDAR-Lite sensors with the NXP PCA9544A I2C Multiplexer.
The PCA9544A (or PCA9548) easily allows 4 I2C devices to share the same I2C address.

## Dependencies
1. Arduino Uno compatible Board
2. Arduino IDE (1.0.5 or newer)
3. USB Cable
4. 1 x PCA9544A I2C Multiplexer ([NXP PCA9544A Product Page](http://www.nxp.com/products/interface_and_connectivity/i2c/i2c_multiplexers_switches/series/PCA9544A.html), [PCA9544A Datasheet](http://www.nxp.com/documents/data_sheet/PCA9544A.pdf)) or PCA9548 (8 device multiplexer) 
5. 2-4 x LIDAR-Lite Sensor
6. The 'Arduino I2C Master Library' from DSS Circuits: http://www.dsscircuits.com/index.php/articles/66-arduino-i2c-master-library

## Installation
1. Download Repository and open with Arduino IDE
2. Install DSS "Arduino I2C Master Library" - Information about installing libraries can be found:  
http://arduino.cc/en/Guide/Libraries


## Arduino Setup

![Multiplexer Fritzing Diagram](http://pl3d.us/ll-multiplexer-update.jpg)
*Note: The breakout board shown here is available from [SparkFun](https://www.sparkfun.com/products/495)

### PCA9544A to Arduino Uno Setup
PCA9544A | Arduino Uno Pins
:---|:---
20 - 5V | 5V
19 - SCL | SCL (topmost pin on the right hand side of Arduino)
18 - SDA | SDA (second pin from the top on the right hand side of Arduino)
10 - GND | GND

### LIDARLite(s) Setup

LIDARLite Cable | Arduino Uno | PCA9544A Channel 0 | PCA9544A Channel 1 | PCA9544A Channel 2 | PCA9544A Channel 3
:---|:---|:---|:---|:---|:---
5V | 5V ||||
PWR EN | _(Unused)_ | _(Unused)_ | _(Unused)_ | _(Unused)_ | _(Unused)_
MODE |  _(Unused)_ | _(Unused)_ | _(Unused)_ | _(Unused)_ | _(Unused)_
SCL | |6|8|13|16
SDA | |5|9|12|15
GND | GND||||

## Usage

1. [Connect an Arduino](#arduino-setup) and upload code. *Note: if you have fewer than 4 sensors, be sure to edit the code to reflect which channels your sensors are attached to in the code.* 
2. Open the serial monitor (Make sure baud rate is set to 115200)
3. For each sensor the distance measured in centimeters (cm) will print to the serial monitor separated by a "."

## How it works

The PCA9544A allows multiple I2C sensors with the same I2C address to be addressed individually. Each I2C device connects to 1 of 4 channels on the PCA9544A and we write a Bitwised OR of the control register (0x04) and the channel (0 through 4) to the PCA9544A at its address (default 0x70, but this can be changed). This enables us to write to the I2C address of the sensors and get only the sensor attached to that channel. 
