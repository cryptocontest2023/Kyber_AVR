# Optimized Kyber on AVR Sensor Nodes

##Introduction
This code is a Crystals-Kyber for 8-bit AVR environment, which submitted to cryptocontest 2023(Title : “Signed LUT-based NTT: Faster Kyber on 8-bit AVR-based Sensor Nodes”). In the future, the goal of this project is to implement Post-Quantum Cryptography in constrained devices such as 8-bit AVR and 16-bit MSP430. Ultimately, I would like to design a PQC library for an AVR environment with minimal stack usage. 

## What This code is trying to achieve
* It aims to properly port the implementation of [PQM4](https://github.com/mupq/pqm4) to the AVR environment. This code is written in a way that is easy to read and understand.

* It aims to apply as much as possible all the latest optimization implementation techniques for Crystals-Kyber. Additionally, assembly code for NTT for AVR environment is provided. Unfortunately, the merge technique for NTT implementation could not be applied due to the limitation of general-purpose registers. Perhaps this will become a new research topic in the future.

## Setup/Build
The description of [Build]] and [Code] is as follows:

[Build]
* We develop and benchmark the code using [Microchip studio](https://www.microchip.com/). Please select "New Project" in “Microchip studio” or “Atmel studio” and paste our code. Our target device is selected in ATmega1280 with 8Kbytes RAM, 128Kbytes flash memory, and 4Kbytes EEPROM. The compile tool is “avr-gcc 5.4.0”, and we compile the code with the “-O3” option. 
We use an 8-bit AVR version of keccak implementation provided by XKCP library. Our Kyber implementation was designed based on optimized [PQM4](https://github.com/mupq/pqm4) code. “Code execution takes a long time! Please wait long enough that it's not a bug.”

[Code]
One can select security level of Kyber in line 5 of "params.h". 
The available options are 2, 3, and 4, for security level 1, 3, and 5, respectively. 
The executable file starts from the "main.c", and can be executed for each API of Kyber KEM. 
Performance measurement can be checked through the register status information of "Microchip studio". 
If set the breaking point to the KEM api on "main.c" and check the register status information, then one can measure the same performance as the summited paper. 
Our “Signed LUT reduction” is defined in "reduce.S", and CT and GS butterfly are defined in "LUT_ntt.S" and "LUT_invntt.S", respectively. 
Except for the dependent files (LUT_butterfly.i, LUT_xN_butterfly.i, and so on) separated for readability, our code organization is basically the same as the stack version of PQM4.
