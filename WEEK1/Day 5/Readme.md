

# Day 5: Optimization in Synthesis

Focus on **optimization in Verilog synthesis**, covering `if-else` statements, avoiding **inferred latches**, and writing efficient, scalable RTL using **for loops** and **generate blocks**.
Hands-on **labs** are included to reinforce these concepts.

---

## Contents

* [1. If-Else Statements in Verilog](#1-if-else-statements-in-verilog)
* [2. Inferred Latches in Verilog](#2-inferred-latches-in-verilog)
* [3. Labs for If-Else and Case Statements](#3-labs-for-if-else-and-case-statements)

  * [Lab 1: Incomplete If Statement](#lab-1-incomplete-if-statement)
  * [Lab 2: Synthesis Result of Lab 1](#lab-2-synthesis-result-of-lab-1)
  * [Lab 3: Nested If-Else](#lab-3-nested-if-else)
  * [Lab 4: Synthesis Result of Lab 3](#lab-4-synthesis-result-of-lab-3)
  * [Lab 5: Complete Case Statement](#lab-5-complete-case-statement)
  * [Lab 6: Synthesis Result of Lab 5](#lab-6-synthesis-result-of-lab-5)
  * [Lab 7: Incomplete Case Handling](#lab-7-incomplete-case-handling)
  * [Lab 8: Partial Assignments in Case](#lab-8-partial-assignments-in-case)
* [4. For Loops in Verilog](#4-for-loops-in-verilog)
* [5. Generate Blocks in Verilog](#5-generate-blocks-in-verilog)
* [6. Ripple Carry Adder (RCA)](#6-ripple-carry-adder-rca)
* [7. Labs on Loops and Generate Blocks](#7-labs-on-loops-and-generate-blocks)

  * [Lab 9: 4-to-1 MUX Using For Loop](#lab-9-4-to-1-mux-using-for-loop)
  * [Lab 10: 8-to-1 Demux Using Case](#lab-10-8-to-1-demux-using-case)
  * [Lab 11: 8-to-1 Demux Using For Loop](#lab-11-8-to-1-demux-using-for-loop)
  * [Lab 12: 8-bit Ripple Carry Adder with Generate Block](#lab-12-8-bit-ripple-carry-adder-with-generate-block)
* [Summary](#summary)

---

## 1. If-Else Statements in Verilog

**If-else statements** are the foundation of conditional execution in RTL.
They are used inside procedural blocks such as `always`, `initial`, tasks, or functions.

### Syntax

```verilog
if (condition) begin
    // Executed when condition is true
end else begin
    // Executed when condition is false
end
```

* **condition** → Boolean expression (non-zero = true, zero = false)
* **begin … end** → Groups multiple statements
* **else** is optional

### Nested If-Else

```verilog
if (cond1) begin
    // Code for cond1 true
end else if (cond2) begin
    // Code for cond2 true
end else begin
    // Code if none of the conditions are true
end
```

---

## 2. Inferred Latches in Verilog

A **latch** is unintentionally inferred when not all execution paths in a combinational block assign values to outputs.
This can create **unwanted sequential behavior**.

### Example (Latch Inference)

```verilog
module ex (input wire a, b, sel, output reg y);
    always @(a, b, sel) begin
        if (sel)
            y = a;  // No else → latch inferred
    end
endmodule
```

* When `sel = 0`, `y` holds its old value → latch inferred.

### Fix (Add Else/Default)

```verilog
module ex (input wire a, b, sel, output reg y);
    always @(a, b, sel) begin
        case (sel)
            1'b1: y = a;
            default: y = b;  // Complete assignment
        endcase
    end
endmodule
```

---

## 3. Labs for If-Else and Case Statements

### Lab 1: Incomplete If Statement

```verilog
module incomp_if (input i0, input i1, input i2, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
end
endmodule
```

---

### Lab 3: Nested If-Else

```verilog
module incomp_if2 (input i0, input i1, input i2, input i3, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
    else if (i2)
        y <= i3;
end
endmodule
```

---

### Lab 5: Complete Case Statement

```verilog
module comp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
always @(*) begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
        default : y = i2;
    endcase
end
endmodule

---

### Lab 7: Incomplete Case Handling

```verilog
module bad_case (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
always @(*) begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        2'b1?: y = i3; // Wildcard used
    endcase
end
endmodule
```


---

### Lab 8: Partial Assignments in Case

```verilog
module partial_case_assign (
    input i0, input i1, input i2,
    input [1:0] sel,
    output reg y, output reg x
);
always @(*) begin
    case(sel)
        2'b00: begin
            y = i0;
            x = i2;
        end
        2'b01: y = i1;
        default: begin
            x = i1;
            y = i2;
        end
    endcase
end
endmodule
```


---

## 4. For Loops in Verilog

* Used inside **procedural blocks** (`always`, `initial`, tasks, functions).
* Replicates hardware only when **iteration count is fixed**.

### Example: 4-to-1 MUX

```verilog
module mux_4to1_for_loop (input wire [3:0] data, input wire [1:0] sel, output reg y);
    integer i;
    always @(data, sel) begin
        y = 1'b0;
        for (i = 0; i < 4; i = i + 1)
            if (i == sel) y = data[i];
    end
endmodule
```

---

## 5. Generate Blocks in Verilog

* Used for **compile-time hardware generation**.
* Needs `genvar` and works with loops.

### Example

```verilog
genvar i;
generate
    for (i = 0; i < 4; i = i + 1) begin : gen_loop
        and_gate and_inst (.a(in[i]), .b(in[i+1]), .y(out[i]));
    end
endgenerate
```

---

## 6. Ripple Carry Adder (RCA)

* Consists of **n full adders** cascaded.
* Carry propagates through each stage → slow but simple.


---

## 7. Labs on Loops and Generate Blocks

* **Lab 9**: 4-to-1 MUX (for loop)
* **Lab 10**: 8-to-1 Demux (case)
* **Lab 11**: 8-to-1 Demux (for loop)
* **Lab 12**: RCA with generate block

---

## Summary

* **If-Else/Case** → must cover all paths to avoid latches.
* **For Loops** → replicate hardware compactly.
* **Generate Blocks** → parameterized, scalable designs.
* **RCA** → example of repetitive structural design.
* **Labs** → reinforce coding practices with synthesis results.
