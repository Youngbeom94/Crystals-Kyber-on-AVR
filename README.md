# Crystals-Kyber-on-AVR

This code accompanies the paper "Efficient Implementation of Crystals-Kyber on 8-bit AVR environment" published in CICS-W22(maybe...).

Please email <darania@kookmin.ac.kr> if you have any suggestions for additions or improvements of my code. In the future, the goal of this project is to implement Post-Quantum Cryptography in constrained devices such as 8-bit AVR and 16-bit MSP430. Ultimately, I would like to design a PQC library for an AVR environment with minimal stack usage. If you have input about what should be covered, I'd be grateful for any input.


## Abstract

Post-Quantum Cryptography generally requires lower performance and higher memory compared to elliptic curve-based cryptographic schemes. Therefore, optimization research has been actively conducted in a 32-bit and 64-bit ARM-based devices, starting with [PQM4](https://github.com/mupq/pqm4). The 8-bit AVR environment has the most constrained performance and memory among embedded devices, and research on the implementation of PQC has not been conducted yet. In order to achieve quantum security in all devices in the future, it is necessary to install PQC even in the 8-bit AVR environment. Therefore, in this project, we present the results of the first optimization implementation of Crystals-Kyber of PKE/KEM, and contribute to the performance evaluation of NIST PQC project.

## What This code is trying to achieve
- It aims to properly port the implementation of [PQM4](https://github.com/mupq/pqm4) to the AVR environment. This code is written in a way that is easy to read and understand.

- It aims to apply as much as possible all the latest optimization implementation techniques for Crystals-Kyber. Additionally, assembly code for NTT for AVR environment is provided. Unfortunately, the merge technique for NTT implementation could not be applied due to the limitation of general-purpose registers. Perhaps this will become a new research topic in the future.


## Setup/Build

## Code
