# Using UART between a Raspberry Pi Pico and Raspberry Pi 3b (Raspbian)
![opening image](https://i.imgur.com/JYi7HVU.jpeg)

This tutorial describes how we can facilitate communication between two devices using UART, a form of serial communication. In this simple example, we will use both a Raspberry Pi Pico (Microcontroller) and a Raspberry Pi 3b (running Raspbian, a Linux image).

## Understanding UART
UART provides a simple way to facilitate serial communication between two devices. Since UART is the protocol that is adopted by the RYLR998, we must use UART in our application.

## UART on the Raspberry Pi 3b
Below is the pinout for the Pi 3b. You'll see the Pi has two GPIO pins specifically configured to serve as a transmit ("TxD") and receive ("RxD") pin, pins 8 and 10 respectively:

![3b pins](https://i.imgur.com/bupd7Vj.png)

This means that the Pi 3b will **send out** data via UART on pin 8 and **receive** data via UART on pin 10.

Before we can use serial communications on the Pi, there is some configuration we have to do first.

Firstly, serial communication has to be enabled. Enter the command `sudo raspi-config`. Navigate to **Interface Options**. Then select **Serial Port**. Say "No" to the question *Would you like a login shell to be accessible over serial?*, and then say "Yes" to the question *Would you like the serial port hardware to be enabled?"*. After seeing the confirmation message, I think it will ask you to reboot. Say yes to reboot. These steps are summarized in the screenshot below:

![enable serial](https://i.imgur.com/ES5gNMI.png)

Now with serial enabled, we are able to start using UART communication!

Like with many (or all?) Linux functionality, the UART service is accessible by incrementally writing to and reading from **a file** on the device that represents the serial interface.

After enabling serial communication via the steps above and rebooting, we can see now a file exists within the `/dev/` directly, called "ttyS0":

![ttyS0](https://i.imgur.com/uA7fzTL.png)

This is the file that the programs on the Pi can use to communicate over serial, reading from it to read incoming messages and writing to it to send out outbound messages.

## UART on the Raspberry Pi Pico
Being a microcontroller, there is a lot less set up required to begin using UART on the Raspberry Pi Pico. I will be using MicroPython in this example.

The MicroPython documentation has a great writeup on [how to use UART on the Pi Pico (RP2040)](https://docs.micropython.org/en/latest/rp2/quickref.html#uart-serial-bus), but I will break it down here as well.

There are multiple UART buses and multiple pins the Raspberry Pi Pico can use to facilitate communication via UART:
- UART0 can be mapped to pins 0/1, 12/13, and 16/17
- UART1 can be mapped to pins 4/5 and 8/9.

We can see this clearly indicated in the official Raspberry Pi Pico pinout diagram from the Raspberry Pi Organization:

![pico pinout](https://i.imgur.com/aTvTWyr.png)

Writing code to send/receive data via UART is easy:

```
from machine import UART, Pin
uart1 = UART(1, baudrate=9600, tx=Pin(4), rx=Pin(5))
uart1.write('hello')  # write 5 bytes
uart1.read(5)         # read up to 5 bytes
```

## Wiring
With serial set up and a basic understanding of the interface any program on the Pi will use to leverage the serial capabilities of the Pi and also how to use UART on the Pi Pico, we are ready to proceed with wiring so we can give it a try!

Wiring for any serial communication is rather simple. Usually there are **3 wires** shared between devices:
- A wire for device A to transmit data to device B
- A wire for device B to transmit data to device A
- A common ground wire, connecting a ground pin on device A to a ground pin on device B

By having a wire for both directions, this ensures the devices are both able to communicate with each other bidirectionally. However, if your project only requires single-direction communication (common in some of my projects), you really only need to worry about setting up the wire in a direction you care about.

My wiring in the below examples is the following:
- Pin 8 (GPIO 15) on the Raspberry Pi to Pin 7 (GP5) on the Pi Pico. *(RPi 3b --> Pico)*
- Pin 6 (GP4) on the Pi Pico to Pin 10 (GPIO 16) on the Raspberry Pi.  *(Pico --> RPi3b)*
- Pin 3 (GND) on the Pi Pico to Pin 6 (GND) on the Raspberry Pi. *(sharing common ground)*

## First Communication
With wiring set up, we'll first use the command line on the Pi 3 to send a message to the Pi Pico.

On the Pi Pico, probably through Thonny, run this simple script. This will establish a UART interface and constantly read any incoming data:

```
import machine
import time

uart1 = machine.UART(1, baudrate=9600, tx=machine.Pin(4), rx=machine.Pin(5), timeout=1000)
while True:
    print(uart1.read(5))
    time.sleep(0.1)
```

The simple script above sets up a UART interface using the pins from our wiring and continuously reads any incoming UART data. You'll see when you run it, if no data is coming in, that's okay! It'll just print `None` (the `uart1.read()` function will return `None`).

**Important to note: The `timeout` parameter above specifies how long the `read` function should wait for incoming data before giving up and just returning `None` ("we didn't get anything after _ seconds"). In MicroPython, the timeout is set in milliseconds (ms), so a value of 1,000 is 1 full second. In the Python-implementation, the `timeout` parameter is in seconds. You'll see this later.**

With this script running (now our Pico is scanning for incoming UART communication constantly), we are now ready to send it some data from the Pi 3!

As mentioned above, serial communication is handled simply by writing to/reading from a file, `/dev/ttyS0`, on the Pi 3. So, we can write to this file from the command line just like any other file!

With the script above running on the Pico (Pico is ready to receive some data in 12-byte increments), run the following command from the command line on the Pi 3b:

```
echo “hello” > /dev/ttyS0 
```

You should see data come in!

![data](https://i.imgur.com/UhB6Cse.png)

You may wonder "if I'm sending the word 'hello', which is only 5 characters and thus should be 5 bytes, why are 12 bytes being received?". This is a good question. When using the `echo` command to write to the `/dev/ttyS0` file, there are also other characters that the command will append to the end, like `\n` and others.

Congratulations! You just sent your first data packet from the Pi 3 to the Pico using UART!

## Code: Pi 3 --> Pico
To accomplish this via Python code, it is actually quite simple.

The Python implementation on its own does not have the required resources to facilitate serial communication on the Pi. So, we'll have to install a popular and simple library, [pyserial](https://pypi.org/project/pyserial/).

Run this from the command line to install (I am installing it in a virtual environment, but that is up to you): `python -m pip install pyserial`

After installing `pyserial` via pip, we are now able to use this library to facilitate UART communication. With the example "read" script from above still running on the Pi Pico, run the following code on the Pi 3b:

```
import serial
ser = serial.Serial("/dev/ttyS0", 9600)
ser.write("hi".encode())
```

Again, although it isn't 12 bytes (that were requested to be read through the MicroPython script), you'll see the script on the Pico should receive that message as two bytes:

![hi](https://i.imgur.com/2nXpD5W.png)

## Code: Pico --> Pi 3
To receive data via UART on the Pi 3b, the code is again quite simple:

```
import serial
import time
ser = serial.Serial(/dev/ttyS0", 9600, timeout=1)
while True:
    print(ser.read(5))
    time.sleep(0.1)
```

Again, the code will perform an infinite loop of trying to ready a full 5 bytes of data on the incoming UART line. If at least 5 bytes are not received after a full second (specified in the `timeout` parameter), it will proceed to print what it did get, which will oftentimes be nothing at all. 

Again, note the while the `UART` class in the MicroPython implementation returns `None` when nothing is received, the `Serial` class in the Python implementation returns a byte array with nothing in it. Just a small difference that should be taken into account in your code.

With that code now running, you should continuously see something printed like this:

```
b''
b''
b''
b''
```

Again, the Pi 3 is continuously reading (listening for) incoming data on the UART line, but nothing is being sent! Let's send it some data from the Pi Pico!

Run the following code snippet on the Pi Pico:

```
import machine
uart1 = machine.UART(1, baudrate=9600, tx=machine.Pin(4), rx=machine.Pin(5))
uart1.write("hello".encode())
```

You should see the message read on the Raspberry Pi 3b!

![read](https://i.imgur.com/lpDv1Y7.png)

To recap, the Raspberry Pi 3b is constantly trying to read a total of 5 incoming bytes. If it does not read at least 5 bytes, it will timeout after trying for 1 second and show what it did read, which would be anything below 5 bytes (could be 0 bytes, could be less than 5). The Raspberry Pi Pico uses UART to send the message "hello", which, being 5 characters, neatly encodes into 5 bytes. The Raspberry Pi 3 reads these 5 bytes and then prints them out.


## UART Buffers
During testing, you may notice that even if you don't have an active script *receiving* messages on the device the data is being sent to via UART, but then later on start one up, a lot of the data will be there, ready to be read, all at once. As if it built up.

How is this? Is it stored somewhere? The answer is yes. Devices like this commonly have UART Rx buffers that are managed at a lower level (i.e. OS-level) that are sort of like a quality of life feature. So just in case your program is running slowly or temporarily stopped reading, the data is still there to be read so no major interruption takes place.

The UART Rx buffer size differs between devices, but I've seen buffers listed as little as 32 bytes and larger than 99,999. So not that large! If the buffer exceeds this length, old bytes will be lapsed and be "overwritten" with new bytes (continuous stream with a cutoff).

This is why sometimes when you start the read program, if you haven't been reading for a while, a lot of data will come back all at once. It was in the buffer, being received, but not being read (and subsequently disposed of). For this reason, you should usually do a buffer flush by reading all bytes all at once to clear the buffer before continuing with reading as part of the normal program. In other words, start each program by rapidly reading all bytes (clearing the buffer) before continuing with the normal read cycle.