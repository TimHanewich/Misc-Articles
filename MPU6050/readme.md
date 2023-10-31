# How to use the MPU-6050 in MicroPython with the Raspberry Pi Pico
During Summer 2023 [I developed my own quadcopter flight controller from scratch](https://medium.com/@timhanewich/taking-flight-with-the-raspberry-pi-pico-micropython-diy-quadcopter-drone-61ed4f7ee746) in MicroPython that runs on a Raspberry Pi Pico. As part of this project, my flight controller needed to continuously collect a stream of telemetry data from an onboard IMU (interial measurment unit). 

I wrote an **MPU-6050 driver in MicroPython** that allows you to interface with an MPU-6050, an inexpensive three-axis accelerometer and gyroscope, using the I2C protocol. I wrote this for my quadcopter flight controller project mentioned above but this can be used in any application, so I am open-sourcing the code and sharing it broadly here.

Unlike [other drivers I found online](https://www.hackster.io/shilleh/connect-mpu-6050-to-raspberry-pi-pico-w-7f3345), my driver is only a **single** `.py` file (one module) and is designed to be very intuitive to read, modify, and use.

I'll explain the basic steps to begin collecting telemetry from an MPU-6050 from a Raspberry Pi Pico with MicroPython below:

## Hardware Needed
For my basic example you'll need:
- Raspberry Pi Pico (Pico W works as well)
- MPU-6050
- Breadboard
- 4 male-to-male jumper wires
- USB to plug into your computer

## Step 1: Wiring
![wiring](https://i.imgur.com/iC8JQr5.jpg)
1. Plug your Raspberry Pi Pico into the breadboard
2. Plug your MPU-6050 into the breadboard directly below the Raspberry Pi Pico
3. Connect the MPU-6050's **VCC** pin to the Raspberry Pi Pico's **VBUS** pin (pin 40)
4. Connect the MPU-6050's **GND** pin to one of the Raspberry Pi Pico's **GND** pins (I am using pin 38 here)
5. Connect the MPU-6050's **SCL** pin to **GP15** (pin 20) on the Raspberry Pi Pico
6. Connect the MPU-6050's **SDA** pin to **GP14** (pin 19) on the Raspberry Pi Pico


## Step 2: Upload `MPU6050.py` Module to Pico
Below is the MicroPython driver for the MPU-6050:
INCLUDE GIST: https://gist.github.com/TimHanewich/60e66eae5bddc48cbffeeaef7b195b8f

You can use the code in the snippet above, but any updates to this code will be available in my **MicroPython-Collection** repo on GitHub [here](https://github.com/TimHanewich/MicroPython-Collection/blob/master/MPU6050/MPU6050.py).

To upload this code snippet to your Raspberry Pi Pico, follow the steps below:
1. Download the file from the snippet above and save to your computer as `MPU6050.py`.
2. Plug your Raspberry Pi Pico (with MicroPython already installed on it) into your computer with a USB cable. Open [**Thonny**](https://thonny.org/)
3. Find the file in the **Files** pane in the top right, right click on it, and select *Upload to /*:  
![Upload to](https://i.imgur.com/y3gtEAj.png)
4. After uploading the `MPU6050.py` file to your Raspberry Pi Pico, you should see it now in the Pico's directory:  
![uploaded](https://i.imgur.com/4XV9Jkx.png)

## Step 3: Capture Telemetry!
With the `MPU6050.py` module now loaded onto the Pico, you are now ready to begin collecting telemtry from the MPU-6050! Open a new code file in Thonny by clicking on the *new* button at the top left of the window. Paste in the following test code:
INCLUDE GIST: https://gist.github.com/TimHanewich/d8f0b4ef67df6238976b6372998ac823

Next, click the green *Run current script* button at the top left and watch your Raspberry Pi Pico begin to read telemetry from the MPU-6050 using I2C!

EMBED YOUTUBE VIDEO: https://www.youtube.com/watch?v=SHaTKe32XyA
