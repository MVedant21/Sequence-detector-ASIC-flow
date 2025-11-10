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

![Alt text](ASIC_project_images/block_diagram.jpg)


### Schematic of Sequence Detector using VIVADO

![Alt text](ASIC_project_images/vivado_schematic.png)

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

### To view different types of cells after Synthesis
```
$   yosys>    stat
```

### To view the generated Schemantics
```
$   yosys>    show
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

![Alt text](ASIC_project_images/sim1.jpg)

### Netlist representation

![Alt text](ASIC_project_images/show.jpg)

### Statistics after Synthesis

![Alt text](ASIC_project_images/stat.jpg)

### Pre Synthesis Simulation Result (O1)

![Alt text](ASIC_project_images/pre_synth_sim.jpg)

### Post Synthesis Simulation Result (O2)

![Alt text](ASIC_project_images/post_synth_sim.jpg)

Here we observe that the pre-synthesis output O1 is equal to the post-synthesis output (O2).


## Layout

### Python Installation
```
$   sudo apt install -y build-essential python3 python3-venv python3-pip
```

### Docker Installation
```
$   sudo apt-get update
$   sudo apt-get install docker docker-compose
$   sudo systemctl start docker
$   sudo docker run hello-world
```

*If the docker is successfully installed u will get a success message like below*

![Alt text](ASIC_project_images/hello_docker.jpg)

### OpenLANE Installation

OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, CU-GR, Klayout and a number of custom scripts for design exploration and optimization. The flow performs full ASIC implementation steps from RTL all the way down to GDSII.

Perform the commands below to install openlane.

```
$   git clone https://github.com/The-OpenROAD-Project/OpenLane.git
$   cd OpenLane/
$   make
$   make test
```

After 43 steps, if it ends with saying - "Basic test passed", then open lane is installed succesfully.

![Alt text](ASIC_project_images/open_lane_installation.jpg)

### Magic Installation

For Magic to be installed and for it to work properly the following softwares have to be installed first.

```
$   sudo apt-get install m4 
$   sudo apt-get install tcsh
$   sudo apt-get install csh
$   sudo apt-get install libx11-dev
$   sudo apt-get install tcl-dev tk-dev
$   sudo apt-get install libcairo2-dev
$   sudo apt-get install mesa-common-dev libglu1-mesa-dev
$   sudo apt-get install libncurses-dev
```


#### Installing Magic
```
$   git clone https://github.com/RTimothyEdwards/magic
$   cd magic
$   ./configure
$   make
$   make install
```



