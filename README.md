# Optimized Kyber on AVR Sensor Nodes

## Introduction
This code is a Crystals-Kyber for 8-bit AVR environment (Title : "**Optimized Kyber on AVR Sensor Nodes**”). Please email <darania@kookmin.ac.kr> if you have any suggestions for additions or improvements of my code. In the future, the goal of this project is to implement Post-Quantum Cryptography in constrained devices such as 8-bit AVR and 16-bit MSP430. Ultimately, I would like to design a PQC library for an AVR environment with minimal stack usage. If you have input about what should be covered, I'd be grateful for any input.

## What This code is trying to achieve
* It aims to properly port the implementation of [PQM4](https://github.com/mupq/pqm4) to the AVR environment. This code is written in a way that is easy to read and understand.

* We design the optimal modular arithmetic to implement Kyber in an 8-bit AVR environment. In our paper, we present  Signed LUT-based arithmetic for Kyber suitable for 8-bit AVR environment. In addition, we aims to apply as much as possible all the latest optimization implementation techniques for Crystals-Kyber. Moreover, assembly code for NTT for AVR environment is provided. Unfortunately, the merge technique for NTT implementation could not be applied due to the limitation of general-purpose registers. Perhaps this will become a new research topic in the future.

## SetUp/Code
The description of **SetUp** and **Code** is as follows:

* **SetUp** : We develop and benchmark the code using [Microchip studio](https://www.microchip.com/). Please select "New Project" in “Microchip studio” or “Atmel studio” and paste our code. Our target device is selected in `ATmega1280` with `8 Kbytes SRAM`, `128 Kbytes flash memory`, and `4 Kbytes EEPROM`. The compile tool is `avr-gcc 5.4.0`, and we compile the code with the `-O3` option. 
We use an 8-bit AVR version of keccak implementation provided by [XKCP](https://github.com/XKCP/XKCP) library. Our Kyber implementation was designed based on optimized [PQM4](https://github.com/mupq/pqm4) code. “Code execution takes a long time! Please wait long enough that it's not a bug.”

* **Code** : One can select security level of Kyber in line 5 of `params.h`. The available options are 2, 3, and 4, for security level 1, 3, and 5, respectively. The executable file starts from the `main.c`, and can be executed for each API of Kyber KEM. 
Performance measurement can be checked through the register status information of [Microchip studio](https://www.microchip.com/). If set the breaking point to the KEM api on `main.c` and check the register status information, then one can measure the same performance as the summited paper. Our **Signed LUT reduction** is defined in `reduce.S`, and butterfies are defined in `LUT_ntt.S` and `LUT_invntt.S`, respectively. Except for the dependent files (`LUT_butterfly.i`, `LUT_xN_butterfly.i`, and so on) separated for readability, our code organization is basically the same as the stack version of [PQM4](https://github.com/mupq/pqm4).

## Optimization strategy
The code for this project is provided in two versions. The `LUT-based Kyber(stack)` version is the code implemented placing LUTs to stack, and the `LUT-based Kyber(Flash_memory)` is the code implemented placing LUTs to flash memory (program memory). The optimization methods of our code are as follows: 

- Signed LUT arithmetic (placing LUTs in stack or flash memory)
  + Our main contributions include alternatives to Montgomery and Barrett arithmetic. Specifically, Montgomery and Barrett reduction are replaced by Signed LUT reduction and small Signed LUT reduction, respectively.
- Polynomial Alignment and Cross Access
- Using Karatsuba multiplication
- Hand-written assembly NTT and Inverse NTT
- Streaming public matrix A and noise e [[PQM4](https://github.com/mupq/pqm4)]
- Using [XKCP](https://github.com/XKCP/XKCP) library for sha3 and SHAKE
- Pre-hased public key for Kyber.CCAKEM Encapsulation

## Abstract of our paper
  Crystals-Kyber (Kyber) is a PQC standardized KEM algorithm selected by NIST and has been optimized on several devices including 32-bit Cortex-M4, 64-bit ARMv8, x86-64-bit CPUs, and so on. However, until now, it has not been implemented on resource-constrained IoT devices such as 8-bit AVR MCUs. In this paper, we present the first Kyber implementation on an 8-bit AVR MCU in order to show the feasibility of using Kyber. Since 8-bit AVR MCU is resource constrained regarding computation power and memory, it is required to consider efficiency for both computation and memory. Regarding computation efficiency, we propose signed LookUp-Table (LUT) reduction methods over $q = 3329$ for efficient number-theoretic transform (NTT) based polynomial multiplication (PM). With careful range calculation, our reduction methods completely replace the usage of Montgomery and Barrett methods, which are the most widely used algorithms for NTT-based PM. In addition, we propose to use Karatsuba-based point-wise multiplication, which can save a costly multiplication with a cheap addition. Regarding memory efficiency, we take full advantage of the state-of-the-art streaming approaches which are merged into PQM4 projects. For a fair comparison, we implemented NTT-based PM using Montgomery and Barrett methods with AVR assembly codes and also implemented Kyber for all security levels by using known optimization approaches used in Kyber PQM4 implementation. With the proposed optimization methods, we can achieve 12.98%, 16.49%, and 19.74% of performance improvements for NTT, single point-wise multiplication, and InvNTT compared with the counterparts using signed Montgomery and signed Barrett assembly implementations, respectively. Furthermore, our optimized Kyber implementations on security level 1, (resp. 3, and 5) outperform the counterparts by regarding Keygen, Encaps, and Decaps, respectively. 
