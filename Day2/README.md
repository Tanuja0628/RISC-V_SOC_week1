# Day 2: Timing Libraries, Synthesis Styles & Flip-Flop Coding ⏱️🔧

Welcome back to **Day 2 of the RTL Workshop!**  
Today we’ll build on yesterday’s foundation and dive into three essential areas of RTL design:

- 📚 Understanding **timing libraries (.lib files)** in the SKY130 open-source PDK  
- 🏗️ Exploring **hierarchical vs. flat synthesis flows**  
- 🔄 Writing **efficient flip-flop RTL code** with reset/set behavior  

---

## 📌 Day 2 Roadmap
1. Timing Libraries  
   - SKY130 PDK Basics  
   - Breaking down `tt_025C_1v80`  
   - Opening and inspecting `.lib` files  
2. Synthesis Styles  
   - Hierarchical vs. Flattened synthesis  
   - Pros, cons, and when to use each  
   - Quick comparison table  
3. Flip-Flop Coding Styles  
   - Async Reset FF  
   - Async Set FF  
   - Sync Reset FF  
4. Simulation & Synthesis Flow  
   - Simulation with Icarus Verilog  
   - Synthesis with Yosys  

---

## 1️⃣ Timing Libraries

### SKY130 PDK Overview
The **SKY130 PDK** (Process Design Kit) is an open-source toolkit for SkyWater’s 130nm CMOS process.  
It contains **timing, power, and process variation** data that synthesis tools use to generate accurate hardware netlists.

---

### Opening a `.lib` File
You can explore the file contents using any text editor:

```bash
sudo apt install gedit   # if not already installed
gedit sky130_fd_sc_hd__tt_025C_1v80.lib
```
Inside, you’ll find delay, power, and functional models for different cells.

2️⃣ Hierarchical vs. Flattened Synthesis
Hierarchical Synthesis
Definition: Keeps the module structure from RTL.

How: Yosys processes each module independently (hierarchy command).

Pros:

- Faster synthesis for large projects

- Easier debugging (modules map back to RTL)

- Modular & reusable

Cons:

- Limited cross-module optimization

- Extra steps needed for detailed reporting

📷 Example: (hierarchical schematic screenshot)

Flattened Synthesis
Definition: Breaks down all modules into a single flat netlist.

How: Done using flatten in Yosys.

Pros:

- Global optimization across the entire design

- Produces a single netlist (sometimes easier for tools)

Cons:

- Slower runtime for big designs

- Debugging becomes harder

- Memory usage increases

📷 Example: (flattened schematic screenshot)

Quick Comparison
Aspect	Hierarchical	Flattened
Hierarchy	Preserved	Removed
Optimization	Module-level only	Whole design
Runtime	Faster for big designs	Slower
Debugging	Easier	Harder
Output	Modular netlist	One big netlist
Best For	Modularity, analysis	Max performance

3️⃣ Flip-Flop Coding Styles
Flip-flops are the backbone of sequential logic. Let’s see three efficient ways to code them in Verilog.

🔹 Asynchronous Reset D Flip-Flop
```verilog

module dff_asyncres (input clk, input async_reset, input d, output reg q);
  always @ (posedge clk, posedge async_reset)
    if (async_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```
Reset overrides the clock

q is immediately cleared when reset = 1

🔹 Asynchronous Set D Flip-Flop
```verilog

module dff_async_set (input clk, input async_set, input d, output reg q);
  always @ (posedge clk, posedge async_set)
    if (async_set)
      q <= 1'b1;
    else
      q <= d;
endmodule
```
Output forced to 1 immediately when set = 1

🔹 Synchronous Reset D Flip-Flop
```verilog

module dff_syncres (input clk, input sync_reset, input d, output reg q);
  always @ (posedge clk)
    if (sync_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```
Reset only applies on the clock edge

4️⃣ Simulation & Synthesis Workflow
Simulation (Icarus Verilog)
Compile:

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
```
Run:

```bash
./a.out
```
Waveform:

```bash
gtkwave tb_dff_asyncres.vcd
```


Synthesis (Yosys)
```bash

yosys

# Load library
read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib

# Load RTL
read_verilog dff_asyncres.v

# Run synthesis
synth -top dff_asyncres

# Map flip-flops
dfflibmap -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib

# Map logic
abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib

# View schematic
show
```


✅ Day 2 Summary
- Understood timing libraries and their role in synthesis

- Learned the difference between hierarchical vs. flattened synthesis approaches

- Explored three flip-flop coding styles (async reset, async set, sync reset)

Practiced simulation with iverilog and synthesis with Yosys
