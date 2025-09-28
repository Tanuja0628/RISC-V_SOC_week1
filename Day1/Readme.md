Day 1 of Week 1 involves intro toverilog RTL design and synthesis, labs in iverilog, gtkwave, yosys and sky130 PDKs
"All required documents are uploaded as images"
# Day 1: Introduction to Verilog RTL Design & Synthesis  

Welcome to **Day 1** of the RTL Workshop!  
In this session, you’ll begin your digital design journey by exploring **Verilog coding**, **simulation with Icarus Verilog (iverilog)**, and the fundamentals of **logic synthesis using Yosys**. Through practical labs and guided explanations, you’ll gain a solid foundation in RTL design.  

---

## 📑 Table of Contents
1. [Simulator, Design, and Testbench Explained](#1-simulator-design-and-testbench)  
2. [Getting Started with iverilog](#2-getting-started-with-iverilog)  
3. [Lab: Simulating a 2-to-1 Multiplexer](#3-lab-simulating-a-2-to-1-multiplexer)  
4. [Verilog Code Walkthrough](#4-verilog-code-walkthrough)  
5. [Introduction to Yosys & Gate Libraries](#5-introduction-to-yosys--gate-libraries)  
6. [Synthesis Lab with Yosys](#6-synthesis-lab-with-yosys)  
7. [Summary](#7-summary)  

---

## 1. Simulator, Design, and Testbench  

**Simulator**  
A simulator is software that checks whether your digital circuit behaves as expected by applying inputs and observing outputs. It helps you verify designs before moving to hardware.  

**Design**  
This is the Verilog code that describes the logic or functionality of your circuit.  

**Testbench**  
A testbench provides the environment to apply inputs to your design and verify outputs. It ensures the design works as intended.  

---

## 2. Getting Started with iverilog  

**iverilog** is an open-source Verilog simulator. The typical simulation process looks like this:  

1. Provide both design and testbench files to iverilog.  
2. The simulator generates a `.vcd` file.  
3. Use **GTKWave** to visualize waveforms.  

---

## 3. Lab: Simulating a 2-to-1 Multiplexer  

Let’s simulate a 2:1 multiplexer using iverilog.  

**Step 1: Clone the repository**  
```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
Step 2: Install tools

bash
Copy code
sudo apt install iverilog
sudo apt install gtkwave
Step 3: Run the simulation
Compile design and testbench:

bash
Copy code
iverilog good_mux.v tb_good_mux.v
Execute the output:

bash
Copy code
./a.out
Open the waveform viewer:

bash
Copy code
gtkwave tb_good_mux.vcd
4. Verilog Code Walkthrough
Here’s the code for the multiplexer (good_mux.v):

verilog
Copy code
module good_mux (input i0, input i1, input sel, output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
How it works:

Inputs: i0, i1 (data), sel (select line)

Output: y

Logic: If sel = 1, output = i1. If sel = 0, output = i0.

5. Introduction to Yosys & Gate Libraries
What is Yosys?
Yosys is an open-source synthesis tool that converts Verilog RTL code into a gate-level netlist. It acts as a bridge between design and hardware implementation.

Key Features:

RTL to logic circuit synthesis

Optimization for area and performance

Technology mapping to real gate libraries

Verification support

Flexible, extensible flows

Why multiple gate variants in libraries?
Standard cell libraries (.lib files) include multiple versions of gates such as AND, OR, NOT. These differ by:

Speed (fast vs. slow)

Power consumption

Area (compact vs. larger)

Drive strength (ability to drive load)

Signal integrity (noise/performance handling)

During synthesis, Yosys picks the best variant to balance performance, power, and area.

6. Synthesis Lab with Yosys
Now let’s synthesize the good_mux design:

Start Yosys

bash
Copy code
yosys
Load the standard cell library

bash
Copy code
read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
Load the Verilog code

bash
Copy code
read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v
Run synthesis

bash
Copy code
synth -top good_mux
Technology mapping

bash
Copy code
abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
Visualize netlist

bash
Copy code
show
This produces a gate-level schematic of your design.

7. Summary
On Day 1, I covered:

The role of simulators, designs, and testbenches

Running your first Verilog simulation with iverilog

Waveform visualization using GTKWave

Analyzing multiplexer code structure

Basics of synthesis in Yosys and how libraries provide different gate options
