# Day 1: Verilog RTL Design and Simulation

## Overview

Welcome to Day 1 of my RTL Design learning journey. In this session, I explored the fundamentals of Verilog HDL, digital circuit simulation, and RTL synthesis. The main objective was to understand how hardware is described using Verilog, verified through simulation, and synthesized into a gate-level representation.

---

## Table of Contents

1. Simulator, Design and Testbench
2. Getting Started with Icarus Verilog
3. Lab: Simulating a 2-to-1 Multiplexer
4. Verilog Code Analysis
5. Introduction to Yosys
6. Simulation Results
7. Summary

---

# 1. What is a Simulator, Design, and Testbench?

## Simulator

A simulator is a software application that executes Verilog code and verifies whether a digital circuit behaves as expected before it is implemented on hardware. It helps identify and correct design errors at an early stage.

## Design

The design is the Verilog module that represents the required digital circuit. It defines the inputs, outputs, and the logic that determines how the circuit functions.

## Testbench

A testbench is a separate Verilog program used to apply different input values to the design and observe the output. It is mainly used to verify that the design works correctly under different test conditions.

---

# 2. Getting Started with Icarus Verilog

## What is Icarus Verilog?

Icarus Verilog is a free and open-source Verilog compiler and simulator. It allows users to compile Verilog source files, execute simulations, and generate waveform files for analyzing the behavior of digital circuits.

### Basic Simulation Flow

```text
Verilog Design
      ↓
   Testbench
      ↓
Icarus Verilog
      ↓
   Simulation
      ↓
  GTKWave
      ↓
Waveform Analysis
```

---

# 3. Lab: Simulating a 2-to-1 Multiplexer

## Installing the Required Tools

Install Icarus Verilog:

```bash
sudo apt install iverilog
```

Install GTKWave:

```bash
sudo apt install gtkwave
```

## Design Files

The following Verilog files were used for this experiment:

- `good_mux.v` – 2-to-1 Multiplexer design
- `tb_good_mux.v` – Testbench for the Multiplexer

## Compiling the Design

```bash
iverilog good_mux.v tb_good_mux.v
```

## Running the Simulation

```bash
./a.out
```

## Viewing the Waveform

```bash
gtkwave tb_good_mux.vcd
```

## Simulation Result

The generated waveform verifies that the 2-to-1 multiplexer functions correctly. The output changes according to the select signal and follows the selected input.

---

# 4. Verilog Code Analysis

## 2-to-1 Multiplexer

A 2-to-1 multiplexer consists of two input lines, one select line, and one output line.

### Working

- The multiplexer has two inputs: `i0` and `i1`.
- It has one select line: `sel`.
- When `sel = 0`, input `i0` is selected.
- When `sel = 1`, input `i1` is selected.
- The selected input is passed to the output `y`.

### Truth Table

| sel | Output |
|-----|--------|
| 0   | i0     |
| 1   | i1     |

---

# 5. Introduction to Yosys

## What is Yosys?

Yosys is an open-source framework for Verilog RTL synthesis. It converts RTL descriptions into a representation of digital logic that can be further optimized and mapped to hardware technologies.

### Basic Synthesis Flow

```text
Verilog RTL
     ↓
   Yosys
     ↓
RTL Synthesis
     ↓
Logic Representation
     ↓
Gate-Level Design
```

---

# 6. Simulation Results

## 6.1 Terminal Compilation

The Verilog design and testbench were compiled successfully using Icarus Verilog in Ubuntu.

<img src="./Screenshot%202026-02-26%20065804.png" alt="Terminal Compilation">

---

## 6.2 Testbench Code

The testbench was used to apply different input and select values to verify the multiplexer operation.

<img src="./Screenshot%202026-02-26%20103727.png" alt="Testbench Code">

---

## 6.3 GTKWave Simulation

The generated VCD waveform was opened and analyzed using GTKWave.

<img src="./Screenshot%202026-02-26%20103753.png" alt="GTKWave Waveform">

---

# 7. Summary

- Learned the fundamentals of Verilog HDL.
- Understood the roles of a simulator, design module, and testbench.
- Learned the working principle of a 2-to-1 multiplexer.
- Created a 2-to-1 multiplexer using Verilog HDL.
- Created a separate testbench for verification.
- Compiled the Verilog design using Icarus Verilog.
- Simulated the design in Ubuntu.
- Generated a VCD waveform file.
- Analyzed the waveform using GTKWave.
- Learned the basic concept of RTL synthesis.
- Got an introduction to Yosys and its role in RTL synthesis.

---

# Day 1 Completed ✅

**Topic:** 2-to-1 Multiplexer

**Language:** Verilog HDL

**Simulation Tool:** Icarus Verilog

**Waveform Tool:** GTKWave

**Synthesis Tool:** Yosys

**Environment:** Ubuntu on VirtualBox

---

# Repository Structure

```text
RTL_Workshop/
│
└── Day_1/
    ├── README.md
    ├── good_mux.v
    ├── tb_good_mux.v
    ├── Screenshot 2026-02-26 065804.png
    ├── Screenshot 2026-02-26 103727.png
    └── Screenshot 2026-02-26 103753.png
```

---

# Tags

#Verilog #RTLDesign #VLSI #DigitalDesign #HDL #ECE #IcarusVerilog #GTKWave #Yosys
