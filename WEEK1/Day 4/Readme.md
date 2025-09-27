# Day 4: Gate-Level Simulation (GLS), Blocking vs. Non-Blocking in Verilog, and Synthesis-Simulation Mismatch

Focus is on three fundamental topics in digital design:

* **Gate-Level Simulation (GLS)**
* **Blocking vs. Non-Blocking Assignments in Verilog**
* **Synthesis-Simulation Mismatch**

---

## Table of Contents

* [1. Gate-Level Simulation (GLS)](#1-gate-level-simulation-gls)
* [2. Synthesis-Simulation Mismatch](#2-synthesis-simulation-mismatch)
* [3. Blocking vs. Non-Blocking Assignments](#3-blocking-vs-non-blocking-assignments)

  * [3.1 Blocking Assignments (`=`)](#31-blocking-assignments-)
  * [3.2 Non-Blocking Assignments (`<=`)](#32-non-blocking-assignments-)
  * [3.3 Comparison Table](#33-comparison-table)
* [4. Labs](#4-labs)
* [5. Summary](#5-summary)

---

## 1. Gate-Level Simulation (GLS)

**Definition:**
GLS is the simulation of a **synthesized gate-level netlist**, instead of RTL code. It helps validate that the synthesized circuit (made of standard cells) is functionally correct and meets timing.

**Why GLS?**

* Ensures **functional correctness** after synthesis.
* Validates **timing** using SDF (Standard Delay Format) annotation.
* Confirms **test structures** (like scan chains for DFT).
* Detects issues missed in RTL simulation.

**When to perform GLS?**

* After synthesis (before physical design).
* After place-and-route (with full timing annotation).

**Types of GLS:**

* **Zero/Unit Delay GLS:** Checks functionality only.
* **Timing GLS:** Includes actual gate and interconnect delays.

---

## 2. Synthesis-Simulation Mismatch

This occurs when **RTL simulation results ≠ Netlist simulation results**.

**Common causes:**

1. **Incomplete sensitivity lists**

   ```verilog
   always @(sel)   // Wrong: i0 and i1 missing
   ```

   Correct:

   ```verilog
   always @(*)     // All inputs automatically included
   ```

2. **Blocking vs Non-blocking misuse**

   * Using `<=` in combinational logic (causes unintended latches).
   * Using `=` inside clocked blocks (causes incorrect sequential behavior).

3. **Non-synthesizable constructs**

   * `#delays`, `initial` blocks, or system tasks.

4. **Tool interpretation differences**

   * Simulation interprets ambiguity differently than synthesis.

**Best Practice:**
Always use synthesizable, deterministic RTL.

---

## 3. Blocking vs. Non-Blocking Assignments

Verilog supports two procedural assignment types:

### 3.1 Blocking Assignments (`=`)

* **Executes sequentially, in order.**
* RHS evaluated and assigned immediately.
* Use in **combinational logic**.

Example:

```verilog
always @(*) begin
  y = a & b;
end
```

---

### 3.2 Non-Blocking Assignments (`<=`)

* **Executes concurrently.**
* RHS values are evaluated first, assignments happen together at the end of the time step.
* Use in **sequential logic (flip-flops)**.

Example:

```verilog
always @(posedge clk) begin
  q <= d;
end
```

---

### 3.3 Comparison Table

| **Blocking (`=`)**                     | **Non-Blocking (`<=`)**              |
| -------------------------------------- | ------------------------------------ |
| Sequential execution (line by line)    | Parallel execution (end of timestep) |
| Updates immediately                    | Updates scheduled                    |
| Used in **combinational** always block | Used in **sequential** always block  |
| Example: mux logic                     | Example: D flip-flop logic           |

---

## 4. Labs

### Lab 1: Ternary Operator MUX

```verilog
module mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```

✔ Correct combinational design.

---

### Lab 2: Synthesis Using Yosys

Run the MUX through Yosys to generate a gate-level netlist.

---

### Lab 3: GLS of MUX

```shell
iverilog primitives.v sky130_fd_sc_hd.v mux.v tb.v
```

Simulates synthesized design.

---

### Lab 4: Bad MUX (Common Mistakes)

```verilog
module bad_mux (input i0, input i1, input sel, output reg y);
  always @(sel) begin
    if (sel)
      y <= i1;   // Wrong: non-blocking in comb logic
    else
      y <= i0;
  end
endmodule
```

**Problems:**

* Missing inputs in sensitivity list.
* Wrong assignment type.

**Corrected:**

```verilog
always @(*) begin
  if (sel) y = i1;
  else     y = i0;
end
```

---

### Lab 5: GLS of Bad MUX

* Expect mismatches due to coding mistakes.

---

### Lab 6: Blocking Assignment Caveat

```verilog
always @(*) begin
  d = x & c;
  x = a | b;
end
```

Here, `d` uses the old value of `x`.

**Correct fix:**

```verilog
always @(*) begin
  x = a | b;
  d = x & c;
end
```

---

## 5. Conclusion

* **GLS** ensures correctness of synthesized netlists with or without timing.
* **Synthesis-Simulation mismatch** occurs due to poor coding practices (fix with proper sensitivity lists, correct assignment usage).
* **Blocking (`=`)** → for combinational; **Non-blocking (`<=`)** → for sequential.
* **Labs** highlighted best practices and common pitfalls.

