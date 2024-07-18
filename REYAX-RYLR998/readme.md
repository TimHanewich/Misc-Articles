# Using the REYAX RYLR998 To Facilitate Communication between two Raspberry Pi Devices
This tutorial describes how we can use [the REYAX RYLR998](https://a.co/d/8JPBouS) (and likely other REYAX LoRa modules) to facilitate communication between two Raspberry Pi's, sending packets of data to each other using UART.

In this simple example, we will use both a Raspberry Pi Pico (Microcontroller) and a Raspberry Pi 3b (running Raspbian, a Linux image).

## UART Protocol
The RYLR998 uses UART as the communication protocol. To understand how to use the RYLR998, you first must understand how to effectively use UART. Read more about the UART protocol and how to use it with a Raspberry Pi here: [Using UART between a Raspberry Pi Pico and Raspberry Pi 3b (Raspbian)](https://timhanewich.medium.com/using-uart-between-a-raspberry-pi-pico-and-raspberry-pi-3b-raspbian-71095d1b259f).