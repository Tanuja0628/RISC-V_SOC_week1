# Day 5 – Optimization in Verilog Synthesis

Day 5 of the RTL Workshop dived into coding styles that affect synthesis quality. We explored how if-else constructs, case statements, loops, and generate blocks can make or break your design. The focus was on avoiding unintended latch inference and writing clean, synthesizable, and scalable RTL.

## 📚 Topics Covered

- If-Else Statements in Verilog

- Inferred Latches – Causes & Fixes

- Labs on If-Else and Case Handling

- For Loops in Verilog

- Generate Blocks in Verilog

- Ripple Carry Adder (RCA) Concept

- Labs on Loops & Generate Blocks

### 🔹 1. If-Else Statements in Verilog

If-else constructs are widely used in behavioral modeling.

Syntax:

if (condition) 
    statement1;
else 
    statement2;


Key points:

else is optional

Use begin…end for multiple statements

Nesting is allowed

### 🔹 2. Inferred Latches in Verilog

A latch is inferred when a variable isn’t assigned in all paths of a combinational block.

❌ Example (Latch inferred):

always @(a, b, sel) begin
    if (sel)
        y = a;   // missing else
end


✔ Fixed version (complete assignment):

always @(a, b, sel) begin
    case(sel)
        1'b1 : y = a;
        default : y = 1'b0;  
    endcase
end


⚡ Rule of Thumb: Always ensure every signal is assigned in all possible paths.

### 🔹 3. Labs – If-Else & Case Statements

Lab 1: Incomplete If → causes latch inference

Lab 2: Synth result of Lab 1

Lab 3: Nested If-Else example

Lab 4: Synth result of Lab 3

Lab 5: Complete Case Statement ✅

Lab 6: Synth result of Lab 5

Lab 7: Incomplete Case → pitfalls with wildcards

Lab 8: Partial Assignments in Case → multiple outputs not handled

👉 Observation: Always provide default in case and else in if to avoid latches.

### 🔹 4. For Loops in Verilog

Used inside procedural blocks (always, initial, tasks, functions).

Synthesizable only when loop bounds are fixed at compile time.

✔ Example – 4-to-1 MUX using For Loop:
```verilog
always @(data, sel) begin
    y = 0;
    for (i = 0; i < 4; i = i + 1) begin
        if (i == sel) y = data[i];
    end
end
```
🔹 5. Generate Blocks in Verilog

Generate blocks let you create repeated structures at compile time.

✔ Example:
```verilog
genvar i;
generate
    for (i = 0; i < 4; i = i + 1) begin : gen_loop
        and_gate inst (.a(in[i]), .b(in[i+1]), .y(out[i]));
    end
endgenerate
```
### 🔹 6. Ripple Carry Adder (RCA)

Built using a chain of full adders.

Each carry-out feeds the next stage’s carry-in.

Simple but suffers from propagation delay with increasing bit-width.

### 🔹 7. Labs – Loops & Generate Blocks

Lab 9: 4-to-1 MUX using For Loop

Lab 10: 8-to-1 Demux using Case

Lab 11: 8-to-1 Demux using For Loop

Lab 12: 8-bit Ripple Carry Adder with Generate Block

✔ Example – RCA with Generate:
```verilog
genvar i;
generate
    for (i = 1; i < 8; i = i + 1) begin
        fa u_fa (.a(num1[i]), .b(num2[i]), .c(carry[i-1]), 
                 .co(carry[i]), .sum(sum[i]));
    end
endgenerate
```
### ✅ Key Takeaways

Always close every path in if-else/case → avoid latches.

For loops & generate blocks → write scalable, reusable RTL.

RCA shows how generate blocks simplify repetitive logic.

Hands-on labs reinforced concepts by comparing good vs. bad coding practices.

⚡ Pro Tip:

“In synthesis, incomplete assignments = hidden latches. Be explicit, not implicit.”
