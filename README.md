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

### Introduction to open-source simulator Iverilog

RTL design is simulated to check for its adherence wrt to the spec. To simulate we use Iverilog. Icarus Verilog is a Verilog simulation and synthesis tool. 

We use a testbench to instantiate the values for the Verilog code variables which is given as input to check for both the verilog code simulation as well as for the netlist. 

### About GTKWave

GTKWave is used to display the waveforms, giving the vcd file as the input.

### Installing iverilog and GTKWave

**For Ubuntu**

Follow the below codes.
```
$   sudo apt-get update
$   sudo apt-get install iverilog gtkwave
```

## Functional Simulation

Follow the below commands.
```
$   iverilog seq_detector.v seq_detector_tb.v
$   ./a.out
$   gtkwave seq_set.vcd
```

### Introduction to Yosys

Yosys stands for "Yosys Open SYnthesis Suite". Yosys is a framework for Verilog RTL synthesis. 

It converts the RTL code to gate-level netlist. The verilog code along with the lib file are the inputs given to it, which then generates the gate-level netlist as the output.

For more information refer to the link attached below:
- https://yosyshq.net/yosys/

To install yosys follow the instructions in this GitHub repository
- https://github.com/YosysHQ/yosys

To just run commnds in this repository and for simpler installations in Ubuntu:
```
$   sudo apt-get install yosys 
```

## Synthesis

Synthesis transforms the simple RTL design into a gate-level netlist with all the constraints as specified by the designer. In simple language, Synthesis is a process that converts the abstract form of design to a properly implemented chip in terms of logic gates.

The two methods to proceed with the synthesis process are as follows:

- Method 1 - Using direct Commands
Invoke 'yosys' and execute the below commands to perform the synthesis of the above circuit.
```
$   yosys
$   yosys>    read_liberty -lib ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
$   yosys>    read_verilog seq_detector.v
$   yosys>    synth -top Sequence_detector_moore
$   yosys>    abc -liberty ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
$   yosys>    clean
$   yosys>    write_verilog -noattr seq_detector_synth.v
```

- Method 2 - Using script file
The above commandscan all be run in a single step by executing the script file after invoking 'yosys' terminal.
```
$   yosys
$   yosys>    script yosys_run.sh
```

### Gate Level Simulation (GLS)

GLS generates the simulation output by running test bench along with the netlist file generated from synthesis as design under test.

Netlist is logically same as RTL code, therefore, same test bench can be used for it. We perform this to verify logical correctness of the design after synthesizing it. Also ensuring the timing of the design is satisfied.

Follow the below commands to run the GLS simulation.
```
$   iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 verilog_model/primitives.v verilog_model/sky130_fd_sc_hd.v seq_detector_synth.v seq_detector_tb.v
$   ./a.out
$   gtkwave seq_set.vcd
```


### Functional Characteristics

In this simultion result we observe three waveform namely Clock(clk), Sequence Input and Detected Output. We also observe that around 160ns the Detector displays a HIGH signal indicating that the given sequence of '1111' was detected succesfully.
