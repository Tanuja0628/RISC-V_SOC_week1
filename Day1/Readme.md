# Day 1: RTL Design & Synthesis Kickoff 🚀

Welcome to **Day 1** of the RTL Workshop!  
Today marks the beginning of your digital design journey. You’ll explore Verilog HDL, run your first simulations with **Icarus Verilog (iverilog)**, and dive into **logic synthesis** using **Yosys**. This day is all about getting your fundamentals right through practical hands-on labs.  

---

## 📌 Agenda for Day 1
1. Simulator, Design & Testbench — What are they?  
2. Setting up and running iverilog  
3. Hands-on Lab: Simulating a 2:1 Multiplexer  
4. Understanding Verilog Code  
5. Introduction to Yosys and Standard Cell Libraries  
6. Synthesis Flow with Yosys  
7. Key Takeaways  

---

## 1️⃣ Simulator, Design & Testbench

- **Simulator** → Software that mimics your digital circuit, applying test inputs and generating outputs. Essential for checking functionality before hardware.  
- **Design** → Your Verilog code describing the intended circuit behavior.  
- **Testbench** → A supportive Verilog file that feeds input signals to your design and monitors outputs.  

In short:  
👉 *Design = What you build*  
👉 *Testbench = How you test it*  
👉 *Simulator = Where you check it works*

---

## 2️⃣ Getting Started with iverilog

**iverilog** is an open-source Verilog simulator.  
The workflow is straightforward:  

Design + Testbench → iverilog → Simulation Output (.vcd) → GTKWave

- `.vcd` = Value Change Dump file (stores signal activity)  
- GTKWave = Tool to view signals as waveforms  

---

## 3️⃣ Lab: Simulating a 2-to-1 Multiplexer

Let’s roll up our sleeves and simulate a simple multiplexer.  

### Step 1: Clone the Repo
```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
Step 2: Install Tools
```bash
sudo apt install iverilog
sudo apt install gtkwave
```
Step 3: Run Simulation
Compile:
```bash
iverilog good_mux.v tb_good_mux.v
```
Execute:

```bash
./a.out
``
View waveforms:
```bash
gtkwave tb_good_mux.vcd
```

4️⃣ Verilog Code Walkthrough
Multiplexer code (good_mux.v):
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

🔎 Explanation:

Inputs: i0, i1 (data), sel (select line)

Output: y

Logic: sel=1 → y=i1, else y=i0


5️⃣ Synthesis Flow with Yosys
Run Yosys step by step:

```tcl

# Load the standard cell library
read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib

# Read design
read_verilog good_mux.v

# Synthesize
synth -top good_mux

# Technology mapping
abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib

# Visualize schematic
show
```
