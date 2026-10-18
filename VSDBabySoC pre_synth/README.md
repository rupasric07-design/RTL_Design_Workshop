# RTL Design, Synthesis and Simulation Experiments

This repository documents the RTL design, simulation, synthesis, and waveform analysis experiments performed using open-source EDA tools such as **Icarus Verilog, GTKWave, and Yosys**.

The experiments covered in this session include:

1. Good MUX RTL Simulation and Synthesis
2. Multiple Modules Synthesis and Visualization
3. BabySoC Pre-Synthesis Simulation and Waveform Analysis

---

# 1. Good MUX – RTL Simulation and Synthesis

## Objective

The objective of this experiment is to understand the complete RTL design flow of a multiplexer, including:

- Verilog design
- Testbench creation
- RTL simulation
- Waveform verification
- Synthesis
- Technology mapping
- Netlist visualization

---

## 1.1 Creating the Testbench

The testbench is used to verify the functionality of the `good_mux` design.

The testbench instantiates the Unit Under Test (UUT) and applies different input combinations to verify the multiplexer operation.

The testbench generates a VCD file using:

```verilog
$dumpfile("tb_good_mux.vcd");
$dumpvars(0, tb_good_mux);
```

The important signals used in the simulation are:

- `i0`
- `i1`
- `sel`
- `y`

The inputs are toggled at different time intervals to verify the output behavior.

### Screenshot

<img width="1001" height="1600" alt="WhatsApp Image 2026-08-30 at 10 25 05 PM" src="https://github.com/user-attachments/assets/0b43588a-9301-4d53-b7bd-2f684ac3df32" />

---

## 1.2 Running RTL Simulation

The Verilog design and testbench are compiled using Icarus Verilog.

```bash
iverilog -o tb_good_mux.out good_mux.v tb_good_mux.v
```

The compiled simulation is executed using:

```bash
vvp tb_good_mux.out
```

The simulation generates the VCD waveform file:

```text
tb_good_mux.vcd
```

---

## 1.3 Viewing the RTL Simulation Waveform

The generated waveform is opened using GTKWave.

```bash
gtkwave tb_good_mux.vcd
```

The waveform is used to observe the relationship between:

- Input `i0`
- Input `i1`
- Select signal `sel`
- Output `y`

The output changes according to the selected input, confirming the correct operation of the multiplexer.

### Screenshot

<img width="1136" height="524" alt="WhatsApp Image 2026-08-30 at 10 25 06 PM" src="https://github.com/user-attachments/assets/c3891025-f081-420f-ab91-4bb77497758d" />

---

## 1.4 Synthesizing the Good MUX

The RTL design is synthesized using Yosys.

First, the Verilog source file can be opened using:

```bash
vi good_mux.v
```

The synthesized design is mapped into an optimized implementation.

During synthesis, Yosys performs operations such as:

- RTL elaboration
- Logic optimization
- Technology mapping
- Cell mapping
- Netlist generation

The resulting synthesized netlist represents the multiplexer using standard cells.

---

## 1.5 Visualizing the Synthesized Netlist

After synthesis, the design can be visualized using:

```tcl
show
```

The schematic shows the input connections `i0`, `i1`, and `sel` connected to the synthesized multiplexer cell.

The output signal `y` represents the selected input.


## Observation

The simulation confirms the correct functionality of the multiplexer. The output signal follows the appropriate input depending on the value of the select signal.

The synthesis visualization also confirms that the RTL multiplexer is successfully mapped into a technology cell.

---

# 2. Multiple Modules – Synthesis and Visualization

## Objective

The objective of this experiment is to understand how multiple Verilog modules are instantiated, synthesized, connected, and visualized using Yosys.

---

## 2.1 Synthesis Analysis

The design is synthesized using Yosys, where the RTL modules are processed and optimized.

During synthesis, Yosys performs:

- Sequential cell processing
- Logic optimization
- Technology mapping
- ABC optimization
- Cell mapping

The synthesis output provides information about the optimized design.



## 2.2 Visualizing the Synthesized Module

The synthesized design can be visualized using the Yosys command:

```tcl
show
```

This command generates a Graphviz representation of the synthesized circuit.

The visualization helps in understanding:

- Input connections
- Logic cells
- Signal flow
- Output connectivity


## 2.3 Visualizing Multiple Modules

The complete design hierarchy containing multiple modules can be visualized using:

```tcl
show multiple_modules
```

The generated schematic displays the connectivity between individual submodules.

The design contains:

- Input `a`
- Input `b`
- Input `c`
- `submodule1`
- Intermediate signal `net1`
- `submodule2`
- Output `y`

The output of the first submodule is connected to the second submodule through an intermediate net.


# 3. BabySoC – Pre-Synthesis Simulation

## Objective

The objective of this experiment is to analyze the BabySoC RTL design and perform pre-synthesis simulation.

The BabySoC design integrates multiple modules including:

- RVMYTH processor core
- PLL
- DAC
- Clock generation logic
- Supporting modules

---

## 3.1 Exploring the Design Hierarchy

The design hierarchy was explored using Yosys.

The synthesized Verilog representation can be generated using:

```tcl
write_verilog -noattr multiple_modules_netlist.v
```

The design hierarchy can be flattened using:

```tcl
flatten
```

After flattening, the design structure can be visualized using:

```tcl
show multiple_modules
```

This generates a graphical representation of the connectivity between the modules.

### Screenshot

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/34b6b0b0-04dd-4268-a148-37bfbf604f5c" />

---

## 3.2 Writing the Flattened Netlist

After flattening the design, the flattened Verilog netlist can be generated using:

```tcl
write_verilog -noattr multiple_modules_netlist2.v
```

The resulting file contains the flattened representation of the design.

---

## 3.3 Inspecting the BabySoC RTL Files

The BabySoC design directory contains multiple RTL source files.

Important files include:

- `avsdac.v`
- `avspll.v`
- `clk_gate.v`
- `pseudo_rand.v`
- `pseudo_rand_gen.v`
- `rvmyth.v`
- `rvmyth_gen.v`
- `testbench.v`
- `vsdbabysoc.v`

These files together implement the complete BabySoC design.

### Screenshot

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/2e0aca5c-4208-44ce-aec9-b8f0d9fedc6f" />

---

## 3.4 RVMYTH Processor Core

The `rvmyth.v` file contains the RVMYTH processor core implementation.

The module includes:

- Clock input
- Reset input
- Output register
- Instruction execution logic
- Program counter logic
- Fetch stage
- Decode stage

The processor executes different RISC-V instructions and generates output signals for the integrated BabySoC system.

### Screenshot

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/b10b537f-a770-4601-a017-74575d843095" />

---

# 4. Testbench Verification

Before running the simulation, the testbench was checked to verify the top-level module.

The following command can be used:

```bash
grep -n "module vsdbabysoc" testbench.v
```

The included Verilog source files can be checked using:

```bash
grep -n "include" testbench.v
```

The output confirms that the required BabySoC modules are included in the testbench.

---

# 5. Pre-Synthesis Simulation

The BabySoC design is compiled using Icarus Verilog.

The following command is used:

```bash
iverilog -o pre_synth_sim.out -DPRE_SYNTH_SIM -I ./include testbench.v
```

### Command Explanation

| Command Option | Description |
|---|---|
| `iverilog` | Verilog compiler |
| `-o pre_synth_sim.out` | Output simulation executable |
| `-DPRE_SYNTH_SIM` | Defines pre-synthesis simulation |
| `-I ./include` | Specifies include directory |
| `testbench.v` | Top-level testbench |

After compilation, the simulation is executed using:

```bash
./pre_synth_sim.out
```

The simulation generates the following waveform file:

```text
pre_synth_sim.vcd
```

### Screenshot

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/3a7f07f8-7bf7-4246-8833-0f3a1cdea093" />

---

# 6. Viewing the Simulation Waveforms

The generated VCD file is opened using GTKWave.

```bash
gtkwave pre_synth_sim.vcd
```

The important signals observed include:

- `CLK`
- `ENb_CP`
- `ENb_VCO`
- `OUT`
- `REF`
- `RV_TO_DAC`
- `VCO_IN`
- `VREFH`
- `reset`

Initially, the signals are loaded into GTKWave for waveform analysis.

### Screenshot

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/f5b189dc-4cd7-4567-bb82-29cf62871092" />

---

# 7. Detailed Waveform Analysis

The internal signals of the BabySoC design were expanded for detailed analysis.

The `RV_TO_DAC` output bus was expanded to observe individual bits:

- `RV_TO_DAC[9]`
- `RV_TO_DAC[8]`
- `RV_TO_DAC[7]`
- `RV_TO_DAC[6]`
- `RV_TO_DAC[5]`
- `RV_TO_DAC[4]`
- `RV_TO_DAC[3]`
- `RV_TO_DAC[2]`
- `RV_TO_DAC[1]`
- `RV_TO_DAC[0]`

The waveform demonstrates the interaction between the processor output and the DAC interface.

### Screenshot

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/7041a770-d09d-4d46-b4d6-6d9d7e7cbf05" />

---

# Commands Used

## Good MUX

```bash
vi good_mux.v
```

```bash
iverilog -o tb_good_mux.out good_mux.v tb_good_mux.v
```

```bash
vvp tb_good_mux.out
```

```bash
gtkwave tb_good_mux.vcd
```

```tcl
show
```

---

## Multiple Modules

```tcl
show
```

```tcl
show multiple_modules
```

---

## BabySoC

```tcl
write_verilog -noattr multiple_modules_netlist.v
```

```tcl
flatten
```

```tcl
show multiple_modules
```

```tcl
write_verilog -noattr multiple_modules_netlist2.v
```

```bash
grep -n "module vsdbabysoc" testbench.v
```

```bash
grep -n "include" testbench.v
```

```bash
iverilog -o pre_synth_sim.out -DPRE_SYNTH_SIM -I ./include testbench.v
```

```bash
./pre_synth_sim.out
```

```bash
gtkwave pre_synth_sim.vcd
```

---

# Overall Conclusion

In this session, multiple RTL design and verification flows were performed successfully.

The Good MUX design was verified through RTL simulation and waveform analysis. The synthesized design was visualized to understand the technology-mapped implementation.

Multiple Verilog modules were synthesized and visualized to understand hierarchical design and module connectivity.

Finally, the BabySoC RTL design was analyzed and simulated in pre-synthesis mode. The generated VCD waveform was examined using GTKWave to observe the interaction between the processor, DAC, PLL, and other system signals.

These experiments provided practical understanding of the complete RTL design flow:

**RTL Design → Testbench → Simulation → Waveform Analysis → Synthesis → Netlist Visualization**
