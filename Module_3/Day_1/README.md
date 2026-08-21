# Day 01 • Combinational Logic Optimizations

> *"Good hardware isn't just designed — it's optimized."*

Today, I explored how synthesis tools optimize digital circuits to reduce **area, improve performance, and eliminate unnecessary logic**.

---

## 📚 Topics Covered

- [Introduction to Optimizations](#introduction-to-optimizations)
- [Objectives of Optimization](#objectives-of-optimization)
- [Combinational Logic Optimization](#combinational-logic-optimization)
- [Constant Propagation](#constant-propagation)
- [Boolean Logic Minimization](#boolean-logic-minimization)
- [Optimization in Yosys](#optimization-in-yosys)
- [Exploring Combinational Logic Optimization](#exploring-combinational-logic-optimization)
- [Optimization 1 - opt_check](#optimization-1-opt_check)
- [Optimization 2 - opt_check2](#optimization-2-opt_check2)
- [Optimization 3 - opt_check3](#optimization-3-opt_check3)
- [Multiple Module Optimization](#multiple-module-optimization)
- [Key Learnings](#key-learnings)
- [Files](#files)
- [Tools Used](#tools-used)
- [Completion](#completion)

---

## 🔹 Introduction to Optimizations

Optimization is the process of improving a digital circuit to achieve better performance while using fewer hardware resources.

During **RTL synthesis**, optimization techniques are applied to simplify the logic and generate an efficient **gate-level implementation**.

The synthesis tool analyzes the RTL code, removes unnecessary logic, and produces an optimized netlist without changing the original functionality of the design.

### 🎯 Objectives of Optimization

- Reduce the number of logic gates
- Minimize chip area
- Lower power consumption
- Improve circuit speed
- Reduce propagation delay

---

## 🔹 Combinational Logic Optimization

Combinational logic circuits do not contain memory elements. Their outputs depend only on the current inputs.

The synthesis tool analyzes the logic and removes unnecessary hardware while preserving the same functionality.

---

## 🔹 Constant Propagation

Constant propagation is an optimization technique in which constant values are propagated through the circuit to simplify the logic.

### Example

```text
y = ((a & b) | c)'
```

If `b = 0`:

```text
y = ((a & 0) | c)'
  = (0 | c)'
  = c'
```

The entire expression is reduced to a single inverter.

---

## 🔹 Boolean Logic Minimization

Boolean algebra can simplify large expressions into much smaller circuits.

### Example

```verilog
assign y = a ? (b ? c : (c ? a : 0)) : (~c);
```

After simplification:

```text
y = a XNOR c
```

A complex expression is reduced to a much simpler logic implementation.

---

## 🔹 Optimization in Yosys

After synthesis, Yosys can remove redundant logic using:

```text
opt_clean -purge
```

This command removes unused cells and wires from the synthesized design.

Other useful optimization commands are:

```text
opt
flatten
opt_clean -purge
```

---

## 🔬 Exploring Combinational Logic Optimization

In this lab, I used **Yosys** inside **VirtualBox** to analyze how different combinational logic expressions are optimized during synthesis.

The generated circuits were visualized using the **`show`** command in Yosys, which displayed the synthesized netlists in **Dot Viewer**.

---

## ⚙️ Optimization 1 (`opt_check`)

![Optimization 1 - opt_check](screenshots/opt_check_show.png)

The synthesized circuit was optimized into a **2-input AND gate**.

### Observation

- Inputs **`a`** and **`b`** are connected to the AND gate.
- The output **`y`** is generated only when both inputs are **1**.
- The synthesis tool simplified the original RTL description and mapped it to a standard cell from the **SKY130 library**.

### Optimized Logic

```text
y = a & b
```

---

## ⚙️ Optimization 2 (`opt_check2`)

![Optimization 2 - opt_check2](screenshots/opt_check2_show.png)

The synthesized circuit was optimized into a **2-input OR gate**.

### Observation

- Inputs **`a`** and **`b`** are connected to the OR gate.
- The output **`y`** becomes **1** whenever at least one input is **1**.
- Yosys recognized the logic expression and directly mapped it to an optimized OR gate.

### Optimized Logic

```text
y = a | b
```

---

## ⚙️ Optimization 3 (`opt_check3`)

![Optimization 3 - opt_check3](screenshots/opt_check3_show.png)

The synthesized circuit was optimized into a **3-input AND gate**.

### Observation

- Inputs **`a`**, **`b`**, and **`c`** are connected to a single AND gate.
- The output **`y`** becomes **1** only when all three inputs are **1**.
- Instead of creating multiple 2-input AND gates, the synthesis tool selected a **single 3-input standard cell**, reducing hardware complexity.

### Optimized Logic

```text
y = a & b & c
```

---

## 🔄 Multiple Module Optimization

When a design contains multiple modules, synthesis can optimize them more efficiently after flattening.

The following Yosys commands can be used:

```text
flatten
opt_clean -purge
```

Flattening combines the hierarchy of multiple modules, allowing the synthesis tool to analyze and optimize the complete logic more effectively.

---

## 📌 Key Learnings

- Constant values can simplify entire logic networks.
- Boolean algebra reduces gate count.
- Yosys automatically removes unnecessary hardware.
- Different RTL descriptions can produce efficient hardware implementations.
- Logic optimization reduces unnecessary hardware while preserving functionality.
- Synthesis can map simplified logic to efficient standard cells.
- Multi-module designs can be optimized further after flattening.
- Netlist visualization helps understand the optimized hardware structure.

---

## 📁 Files

```text
Day1/
│
├── opt_check.v
├── opt_check2.v
├── opt_check3.v
│
├── screenshots/
│   ├── opt_check_show.png
│   ├── opt_check2_show.png
│   └── opt_check3_show.png
│
└── README.md
```

---

## 🛠️ Tools Used

- **Yosys**
- **VirtualBox**
- **SKY130 Standard Cell Library**
- **Dot Viewer**
- **Verilog HDL**
- **GitHub**

---

## 📸 Screenshots

### Optimization 1 - `opt_check`

![opt_check_show](screenshots/opt_check_show.png)

### Optimization 2 - `opt_check2`

![opt_check2_show](screenshots/opt_check2_show.png)

### Optimization 3 - `opt_check3`

![opt_check3_show](screenshots/opt_check3_show.png)

---

## ✅ Completion

| Topic | Status |
|------|--------|
| Introduction to Optimizations | ✅ Completed |
| Combinational Logic Optimization | ✅ Completed |
| Constant Propagation | ✅ Completed |
| Boolean Logic Minimization | ✅ Completed |
| Optimization in Yosys | ✅ Completed |
| Exploring Combinational Logic Optimization | ✅ Completed |
| Optimization 1 - `opt_check` | ✅ Completed |
| Optimization 2 - `opt_check2` | ✅ Completed |
| Optimization 3 - `opt_check3` | ✅ Completed |
| Multiple Module Optimization | ✅ Completed |
| Netlist Visualization | ✅ Completed |

---

## 📝 Day 01 Summary

During Day 01, I learned how synthesis tools optimize combinational RTL logic by removing redundant hardware, propagating constants, simplifying Boolean expressions, and mapping optimized logic to efficient standard cells.

The experiments demonstrated how different RTL descriptions can be transformed into simpler **AND, OR, and multi-input logic gates** while preserving the original functionality.

---

### ⭐ Day 01 Complete

> *"Every unnecessary gate removed is another step toward smarter hardware."*

**Day 01 Completed Successfully! ✅**
