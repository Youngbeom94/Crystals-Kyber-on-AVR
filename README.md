# Crystals-Kyber on 8-bit AVR environment

This project is accompanied by my master degree's paper "Efficient Implementation of Post-Quantum Cryptography Crystals-Kyber on 8-bit AVR microcontroller".

Please email <darania@kookmin.ac.kr> if you have any suggestions for additions or improvements of my code. In the future, the goal of this project is to implement Post-Quantum Cryptography in constrained devices such as 8-bit AVR and 16-bit MSP430. Ultimately, I would like to design a PQC library for an AVR environment with minimal stack usage. If you have input about what should be covered, I'd be grateful for any input.


## Abstract

Post-Quantum Cryptography generally requires lower performance and higher memory compared to elliptic curve-based cryptographic schemes. Therefore, optimization research has been actively conducted in a 32-bit and 64-bit ARM-based devices, starting with PQM4. The 8-bit AVR environment has the most constrained performance and memory among embedded devices, and research on the implementation of PQC has not been conducted yet. In order to achieve quantum security in all devices in the future, it is necessary to install PQC even in the 8-bit AVR environment. Therefore, in this paper, we present the first optimization implementation of Crystals-Kyber and contribute to the performance evaluation of NIST PQC project.

## What This code is trying to achieve
- It aims to properly port the implementation of [PQM4](https://github.com/mupq/pqm4) to the AVR environment. This code is written in a way that is easy to read and understand.

- It aims to apply as much as possible all the latest optimization implementation techniques for Crystals-Kyber. Additionally, assembly code for NTT for AVR environment is provided. Unfortunately, the merge technique for NTT implementation could not be applied due to the limitation of general-purpose registers. Perhaps this will become a new research topic in the future.


## Setup/Build
We simulate using [Microchip Studio](https://www.microchip.com/en-us/tools-resources/develop/microchip-studio). Please generate a new project, move the source code and run it. As for the compilation option, `avr-gcc -O3` for speed version and `avr-gcc -Os` for stack version was used. The choice of board depends on the security level of Crystals-Kyber. However, it is recommended to select the `ATmega1284p` with 16,384 bytes of SRAM for smooth execution at all security levels. `randombytes` api, which generates entropy with the help of the operating system, we uses numbers the same as PQC-KAT testvectors of NIST.

## Code
The code for this project is provided in two versions. The `stack` version is the code implemented using a minimal stack, and the `speed` version achieves higher speed instead of using additional stacks. The optimization methods applied to each version are as follows: 
- `stack`
  + Streaming public matrix A and noise e [[PQM4](https://github.com/mupq/pqm4)]
  + Using [XKCP](https://github.com/XKCP/XKCP) library for sha3 and SHAKE
  + Signed Barrett/Montgomery reduction[[Seiler2018](https://eprint.iacr.org/2018/039.pdf)]
  + Hand-written assembly NTT and Inverse NTT
- `speed`
  + Streaming public matrix A and noise e [[PQM4](https://github.com/mupq/pqm4)]
  + Asymmetric multiplication [[Becker2021](https://eprint.iacr.org/2021/986)]
  + Better Accumulation [[Amin2022](https://eprint.iacr.org/2022/112)]
  + Pre-hased public key for Kyber.CCAKEM Encapsulation
  + Using [XKCP](https://github.com/XKCP/XKCP) library for sha3 and SHAKE
  + Signed Barrett/Montgomery reduction[[Seiler2018](https://eprint.iacr.org/2018/039.pdf)]
  + Hand-written assembly NTT and Inverse NTT
