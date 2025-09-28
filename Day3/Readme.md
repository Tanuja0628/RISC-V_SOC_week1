# Day 3 – Combinational & Sequential Circuit Optimization

### On Day 3 of the RTL Workshop, the focus shifts to making designs smarter and faster by applying optimization strategies to both combinational and sequential circuits.
The session introduced techniques that not only improve efficiency but also reduce resource usage and power consumption, followed by hands-on labs to reinforce the concepts.

## 📌 Agenda

🔹 Constant Propagation

🔹 State Optimization

🔹 Cloning

🔹 Retiming

🔹 Practical Labs

### 1. Constant Propagation

Constant propagation is a technique where variables assigned to fixed values are directly replaced by those constants during synthesis.
This simplification leads to smaller, faster, and more power-efficient circuits.

How it works:

- Identify signals with constant values.

- Replace them directly in logic.

- Allow synthesis tools to simplify the circuit further.

#### Benefits:

🟢 Reduced logic depth

🟢 Improved speed

🟢 Lower resource usage

### 2. State Optimization

State optimization focuses on finite state machines (FSMs), reducing redundancy to achieve compact and efficient logic.

Key methods:

- State Reduction → Merge equivalent or unused states

- State Encoding → Choose codes that simplify logic

- Logic Minimization → Boolean simplification or CAD tools

- Power Reduction → Techniques like clock gating

### 3. Cloning

Cloning is the duplication of critical logic elements to reduce delay and improve load balancing.

Process:

- Detect timing bottlenecks using analysis tools.

- Duplicate the critical cell or logic block.

- Redistribute loads to the cloned blocks.

- Verify performance improvements through timing analysis.

### 4. Retiming

Retiming is the art of moving registers (flip-flops) across logic to balance path delays while keeping functionality intact.

Steps:

- Represent design as a graph.

- Relocate registers to balance delays.

- Check constraints to ensure equivalence.

- Optimize for speed and power.

### 🧪 Hands-On Labs
Lab 1 – Cleaning Redundant Logic
```verilog
module opt_check (input a , input b , output y);
  assign y = a ? b : 0;
endmodule
```

➡ Optimization replaces conditional logic with simpler equivalent logic.
Command used:
```bash
opt_clean -purge
```
Lab 2 – Simple MUX Optimization
```verilog
module opt_check2 (input a , input b , output y);
  assign y = a ? 1 : b;
endmodule
```

Acts like a 2:1 multiplexer, simplified during synthesis.

Lab 3 – Alternate MUX Design
```verilog
module opt_check2 (input a , input b , output y);
  assign y = a ? 1 : b;
endmodule
```

Outputs 1 when a=1, otherwise passes b.

Lab 4 – Nested Conditional Simplification
```verilog
module opt_check4 (input a , input b , input c , output y);
  assign y = a ? (b ? (a & c) : c) : (!c);
endmodule
```

Logic reduces to:

y = a ? c : !c;

Lab 5 – DFF with Constant Assignment
```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

Flip-flop resets to 0 but always loads 1 afterwards.

Lab 6 – Constant Flip-Flop Output
```verilog
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

Flip-flop always outputs 1, regardless of input.

🔑 Takeaways

Constant Propagation → Replace constants early to reduce logic.

State Optimization → Compact FSMs for efficient design.

Cloning → Duplicate logic to balance critical paths.

Retiming → Move registers to optimize timing.

Labs → Practical Verilog experiments demonstrated how synthesis tools apply these optimizations.
