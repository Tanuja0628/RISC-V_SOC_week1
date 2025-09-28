# Day 4 – Gate-Level Simulation & Verilog Coding Practices

Day 4 of the RTL Workshop was all about bridging the gap between theory and real-world hardware. We explored how designs behave after synthesis, learned the right way to use blocking vs. non-blocking assignments, and identified the common traps that cause simulation-synthesis mismatches.

## 📌 Topics of the Day

- Gate-Level Simulation (GLS)

- Synthesis–Simulation Mismatch

- Blocking (=) vs. Non-Blocking (<=) Assignments

- Blocking Statements

- Non-Blocking Statements

## Practical Labs

### 🔹 1. Gate-Level Simulation (GLS)

GLS = simulation of a synthesized gate-level netlist.
It helps validate:

✅ Functional correctness

✅ Timing checks (setup/hold)

✅ Power estimates

✅ Scan chains & test structures

When is GLS used?

After synthesis → to confirm netlist correctness

Before physical design → to catch early functional/timing bugs

Types of GLS:

Functional GLS → zero/unit delay simulation for logic correctness

Timing GLS → includes SDF timing data for real-world checks

### 🔹 2. Synthesis–Simulation Mismatch

Mismatch = RTL simulation results ≠ Gate-level/hardware results.
Causes:

Using non-synthesizable code (initial, #delay, etc.)

Ambiguities (missing else, wrong sensitivity lists)

Different interpretations by tools

Best Practice:
Always write clean, synthesizable, and deterministic RTL.

### 🔹 3. Blocking vs. Non-Blocking in Verilog

Two flavors of procedural assignments:

3.1 Blocking (=)

Executes sequentially, step by step.

Best for combinational logic.

always @(*) y = a & b;

3.2 Non-Blocking (<=)

Executes concurrently at the end of timestep.

Best for sequential logic.

always @(posedge clk) q <= d;

#### 🧪 Hands-On Labs
Lab 1 – Ternary MUX
```verilog
module ternary_operator_mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```

➡ Simple 2:1 MUX using ternary operator.

Lab 2 – Yosys Synthesis

Run synthesis for the above MUX using Yosys flow.

Lab 3 – GLS of MUX

Command example:
```bash
iverilog /path/primitives.v /path/sky130_fd_sc_hd.v ternary_operator_mux.v testbench.v
```
Lab 4 – Bad MUX Example
```verilog
module bad_mux (input i0, input i1, input sel, output reg y);
  always @ (sel) begin
    if (sel)
      y <= i1;   // wrong: non-blocking in combo logic
    else 
      y <= i0;
  end
endmodule
```

❌ Problems:

Incomplete sensitivity list (missing i0, i1).

Wrong operator (<= instead of =).

✔ Corrected version:
```verilog
always @(*) begin
  if (sel) y = i1;
  else     y = i0;
end
```
Lab 5 – GLS of Bad MUX

Run GLS → expect mismatches/warnings.

Lab 6 – Blocking Caveat
```verilog
module blocking_caveat (input a, input b, input c, output reg d);
  reg x;
  always @ (*) begin
    d = x & c;   // uses old value of x
    x = a | b;
  end
endmodule
```

❌ Wrong ordering → d uses stale x.
✔ Fix:
```verilog
always @(*) begin
  x = a | b;
  d = x & c;
end
```
Lab 7 – Synthesizing Corrected Module

Run Yosys synthesis → observe how corrected ordering produces the intended logic.

✅ Key Learnings

GLS is mandatory for checking both functionality & timing after synthesis.

Avoid synthesis–simulation mismatches by using synthesizable coding style.

Blocking (=) → for combinational logic; Non-blocking (<=) → for sequential logic.

Labs showed real-world pitfalls (bad sensitivity list, wrong assignment type, incorrect order) and how to fix them.

⚡ Tip of the Day:

“Always simulate both RTL and synthesized netlists. Trust your synthesis tool, but verify its output!”
