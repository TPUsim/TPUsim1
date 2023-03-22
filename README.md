# SMA Simulator
SMAsim is a TPU-style [1] Systolic Array functional/detailed simulator. This tool has been developed and maintained by [**Bridge LAB**](https://engineering.usu.edu/ece/faculty-sites/bridge-lab/) at **Utah State University** with the goal of promoting extensive research in the AI training domain.

SMAsim has been implemented using C++. The RTLs of the Multiply-Accummate Unit is used along with a Statistical Timing Analysis tool to obtain the computational (path sensitized) delays for various input combinations. The usage of path sensitization delays aids in creating a real-time simulation environment.

![!alt text](https://github.com/bridgelabUSU/SMAsim/blob/master/docs/images/SystolicArray.png)
<!--
Currently, only one layer of operation is supported.
Source files: pstream.h, SystolicArray.hpp, SystolicArray.cpp, main.cpp
To know about pstream.h, read the second answer in:
https://stackoverflow.com/questions/478898/how-to-execute-a-command-and-get-output-of-command-within-c-using-posix
Makefile: makefile
How to compile: make
How to generate debug build: make -f makefile_debug
-->

### Extract delay file
```sh
$ tar -xzvf delays.tar.gz
$ python sequence_for_tpu_high.py <delay in ns> > high_delay_seqs.txt
```

### How to run: 
```sh
$ ./runTPU -h
```


#### Example: 
```sh
$ ./runTPU -a activation.txt -b bias.txt -w weight.txt -l lut.txt -o output.txt -g green_tpu_stats.txt -d 8
```
>-d -> dimension of the weight, activation and bias matrices. d = 8 means 8x8 matrix. Also, the systolic array dimension is same as d.

>-a -> text file containing the activation matrix.

> -b -> text file containing the bias matrix.

> -w -> text file containing the weight matrix.

> -l -> input/output text file that will be read and/or written with the last simulation's Error LUT for GreenTPU

> -g -> text file that will contain the GreenTPU statistics.

> -o -> text file that will contain the output matrix obtained by multiplying the activation and weight matrices through the systolic array.

<!--The simulator also outputs the GreenTPU stats in the file green_tpu_stats.txt-->


## Detailed Simulation
For each MAC operation in any cycle, the sta_tool can be invoked to get a realistic circuit-level delay from the MAC netlist and from inputs in two consecutive cycles. This detailed simulation
can be guided from a prior knowledge of input data-driven delay pattern.


## Tackling Timing Errors
By default, timing errors are tackled using TE-Drop [2]. The simulator also has implementations of our GreenTPU [3] and EFFORT [4] techniques that offers a better resiliency against PV-induced timing errors at NTC.

[1] Jouppi, N. P. and others In-datacenter performance analysis of a tensor processing unit. In Computer Architecture (ISCA), 2017 ACM/IEEE 44th Annual International Symposium on (2017), IEEE, pp. 1–12.

[2] Zhang, J. and others ThUnderVolt: Enabling Aggressive Voltage Underscaling and Timing Error Resilience for Energy Efficient Deep Neural Network Accelerators. arXiv preprint arXiv:1802.03806 (2018).

[3] P. Pandey and others Greentpu: Improving timing error resilience of a near-threshold tensor processing unit,” in IEEE/ACM Design Automation Conference (DAC), pp. 173:1–173:6, 2019.

[4]  N. D. Gundi and others Effort: Enhancing energy efficiency and error resilience of a near-threshold tensor processing unit,” in 2020 25th Asia and South Pacific Design Automation Conference (ASP-DAC), pp. 241–246, IEEE, 2020.


## Core Developer
- Prabal Basu (prabalb@aggiemail.usu.edu)


## Additional Contributors
- Pramesh Pandey (pandey.pramesh1@aggiemail.usu.edu)
- Noel Daniel Gundi (noeldaniel.gundi@usu.edu)
