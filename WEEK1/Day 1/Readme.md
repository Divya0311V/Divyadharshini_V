
# Day 1: Introduction to Verilog RTL Design & Synthesis

In this session, we will begin with the fundamentals of Verilog design and simulation using **Icarus Verilog (iverilog)**, then move on to logic synthesis with **Yosys**. By the end of this module, you will have completed your first design simulation and synthesis experiment.

---

## Table of Contents

1. [Simulator, Design and Testbench](#1-simulator-design-and-testbench)
2. [Simulation with Icarus Verilog](#2-simulation-with-icarus-verilog)
3. [Lab Experiment: 2-to-1 Multiplexer](#3-lab-experiment-2-to-1-multiplexer)
4. [Verilog Code Walkthrough](#4-verilog-code-walkthrough)
5. [Introduction to Yosys and Libraries](#5-introduction-to-yosys-and-libraries)
6. [Synthesis Flow with Yosys](#6-synthesis-flow-with-yosys)
7. [Summary](#7-summary)

---

## 1. Simulator, Design and Testbench

**Simulator**
A simulator is a software tool that models the behavior of a digital circuit. It allows you to test the design with different inputs and check the outputs before implementing it in hardware.

**Design**
The design is the Verilog code that describes the digital logic function, such as a multiplexer or adder.

**Testbench**
The testbench provides the stimulus (input signals) to the design and verifies the correctness of the outputs.
 

---

## 2. Simulation with Icarus Verilog

**Icarus Verilog (iverilog)** is an open-source Verilog simulator. The simulation process generally follows these steps:

<div align="center">
  <img src="https://github.com/user-attachments/assets/3ca190fb-cfa4-4abb-b9e1-0151b3c4bdba" width="70%">
</div>  

* The design and testbench files are compiled using iverilog.
* A simulation executable is produced.
* Running this executable generates a `.vcd` file.
* The waveform file can be viewed in **GTKWave**.

---

## 3. Lab Experiment: 2-to-1 Multiplexer

We will now simulate a 2-to-1 multiplexer design.

**Step 1: Clone the Repository**

```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

**Step 2: Install the Tools**

```bash
sudo apt install iverilog
sudo apt install gtkwave
```

**Step 3: Compile and Simulate**

```bash
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
``` 

---

## 4. Verilog Code Walkthrough

The Verilog code for the multiplexer is shown below:

```verilog
module good_mux (input i0, input i1, input sel, output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

**Explanation**

* `i0` and `i1` are data inputs.
* `sel` is the select line.
* `y` is the output.
* The logic is: if `sel = 1`, `y` takes `i1`. If `sel = 0`, `y` takes `i0`.

---

## 5. Introduction to Yosys and Libraries

**Yosys** is an open-source tool for logic synthesis. It translates the Verilog RTL description into a gate-level representation.

**Key features of Yosys**

* Performs synthesis and optimization
* Maps RTL logic to available hardware cells
* Supports verification of results

**Why libraries provide multiple gate versions**
A standard cell library (`.lib`) contains different implementations of gates optimized for:

* Performance (high-speed gates)
* Power (low-power cells)
* Area (compact cells)
* Drive strength (ability to handle higher loads)

---

## 6. Synthesis Flow with Yosys

To synthesize the multiplexer design using Yosys:

1. Launch yosys

```bash
yosys
```

2. Read the standard cell library

```bash
read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
```

3. Read the Verilog design

```bash
read_verilog good_mux.v
```

4. Run synthesis

```bash
synth -top good_mux
```

5. Perform technology mapping

```bash
abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
```

6. Visualize the netlist

```bash
show
```
 

---

## 7. Conclusion

* Understood the purpose of simulator, design, and testbench.
* Ran the first Verilog simulation using iverilog and GTKWave.
* Analyzed the Verilog code for a 2-to-1 multiplexer.
* Learned the basics of Yosys and standard cell libraries.
* Completed synthesis of the multiplexer design.

