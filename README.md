# Sequence Detection using Moore FSM

This project simulates the design of a Sequence Detector built using the MOORE FSM logic. We can detect a pre-decided 4 bit sequence and provide an output high when the sequence is detected. The following circuit design can detect overlapping sequences.


## Introduction

### Sequence Detectors

A **sequence detector** is a type of sequential circuit designed to recognize a specific pattern of bits in its input stream. Its output is **HIGH** (or logic 1) only when the target pattern is successfully detected; otherwise, the output remains **LOW** (or logic 0).

### Moore FSM 

**Moore Finite State Machines** (FSMs) are a specific model of sequential circuit characterized by how their output is generated. In a Moore machine, the output value is determined solely by the circuit's **current state**. Consequently, the output is independent of the immediate or previous input values leading up to the current state.

### Applications

A Sequence Detector can be used in the following areas:
- Binary sequences are inserted at the beginning (or end) of a data frame or subframe emanating from a digital data processor of a spacecraft. Sequence detectors are used in the decoding equipment on the ground to provide **flags** which indicate the beginning (or end) of a data block
- Detecting programming sequences to perform a sequence of tasks.

### Block Diagram of Sequence Detector

![Alt text](e.jpg)

