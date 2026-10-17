# OpenROAD Physical Design and Static Timing Analysis (STA)
## VSDBabySoC on SKY130HD — Multi-Corner Timing Analysis Across 16 PVT Corners

![Platform](https://img.shields.io/badge/Platform-SKY130HD-blue)
![Flow](https://img.shields.io/badge/Flow-OpenROAD--flow--scripts-orange)
![Analysis](https://img.shields.io/badge/Analysis-16%20PVT%20Corners-green)
![Status](https://img.shields.io/badge/Setup%20Timing-Requires%20Optimization-red)

## 1. Project Overview

This project covers physical design implementation and Static Timing Analysis (STA) of the **VSDBabySoC design using the SkyWater SKY130HD standard-cell platform**.

The implementation flow uses OpenROAD-flow-scripts to perform synthesis, floorplanning, tapcell and power distribution network (PDN) preparation, placement, clock-tree synthesis (CTS), routing-related analysis, and timing evaluation.

The project also examines timing behavior across 16 Process, Voltage, and Temperature (PVT) corners. Separate hold and setup slack analyses are used to identify timing margins and operating conditions that require further optimization.

### Project Details

| Parameter | Description |
|---|---|
| Design | VSDBabySoC |
| Platform / PDK | SkyWater SKY130HD |
| Flow framework | OpenROAD-flow-scripts |
| Workspace | `~/Desktop/OpenROAD-flow-scripts/flow` |
| Design configuration | `designs/sky130hd/vsdbabysoc/config.mk` |
| Timing analysis | Static Timing Analysis (STA) |
| Corner analysis | 16 PVT corners |
| Physical-design stages | Synthesis, floorplanning, placement, CTS, routing-related analysis |
| Main objective | Evaluate physical implementation progress and timing behavior across operating corners |

## 2. Objectives

The objectives of this project are:

1. To understand the OpenROAD physical-design flow for VSDBabySoC.
2. To configure the SKY130HD technology and design environment.
3. To perform RTL synthesis and generate the required design databases and timing constraints.
4. To study floorplanning, macro placement, tapcells, and power distribution network generation.
5. To analyze global placement and detailed placement results.
6. To understand clock-tree synthesis and its effect on setup and hold timing.
7. To inspect routing geometry, signal connectivity, and estimated parasitic effects.
8. To perform Static Timing Analysis across 16 PVT corners.
9. To compare setup slack and hold slack across operating conditions.
10. To identify timing-critical corners and determine the need for further optimization.

## 3. Design Architecture and Macro Integration

The VSDBabySoC source directory contains the following design files:

- `vsdbabysoc.v`
- `rvmyth.v`
- `avsdpll.v`
- `avsddac.v`

These modules form the design hierarchy used by the physical-design flow.

### 3.1 VSDBabySoC

VSDBabySoC is the top-level design used for the implementation flow. Its source files and dependent modules must be correctly specified in the design configuration before synthesis.

### 3.2 Hard Macro Integration

The DAC and PLL are treated as hard macros in the physical-design flow.

Correct macro integration requires:

- LEF abstracts describing physical geometry and pins.
- Timing models for timing analysis.
- Correct pin connectivity.
- Appropriate placement constraints.
- Valid power and ground connections.

The OpenROAD GUI shows the DAC macro with locked placement. Macro position, orientation, pin accessibility, and surrounding blockages can affect routing resources and congestion.

The implementation logs also contain warnings that some PLL supply pins are not connected to any net. These warnings must be reviewed against the intended macro power-pin mapping.

## 4. Technology and Design Files

The project uses the SKY130HD standard-cell platform.

The Liberty file visibly identified in the report is:

`sky130_fd_sc_hd__tt_025C_1v80.lib`

This file represents the typical process corner at 25°C and 1.80 V.

Important file types used by the flow include:

| File type | Purpose |
|---|---|
| Verilog/SystemVerilog RTL | Describes the design functionality |
| Liberty (`.lib`) | Contains standard-cell timing and electrical models |
| LEF | Describes physical cell and macro geometry |
| SDC | Defines timing constraints, including clocks and I/O delays |
| ODB | Stores physical design geometry and connectivity |
| Parasitic data | Represents estimated or extracted interconnect effects |
| Reports and logs | Record implementation results, warnings, errors, and timing metrics |

The correct library, macro models, technology files, design constraints, and database must be available for the relevant implementation and timing stages.

## 5. Theory of Physical Design

### 5.1 RTL Synthesis

RTL synthesis converts the design description into a technology-mapped gate-level netlist using the target standard-cell library.

During this stage:

- The RTL source files are read.
- The design hierarchy is elaborated and linked.
- Logic is mapped to available standard cells.
- Timing constraints and technology libraries are used by the flow.
- Design database and constraint files are generated for later stages.

The report shows synthesis-related output, including the reading of the SKY130HD typical Liberty file and the writing of ODB and SDC artifacts.

These outputs provide the basis for subsequent physical-design stages.

### 5.2 Floorplanning

Floorplanning defines the physical dimensions and organization of the design.

It includes:

- Die and core geometry.
- Macro placement.
- Standard-cell placement regions.
- Routing space and access around macros.
- Physical constraints needed for later implementation stages.

The DAC macro is visible in the floorplan GUI. Its placement and orientation influence routing channels, pin access, and congestion.

### 5.3 Tapcell Insertion

Tapcells support well and substrate-tie requirements in the standard-cell layout.

The tapcell stage prepares the design for the technology-specific physical requirements of the SKY130HD platform.

### 5.4 Power Distribution Network (PDN)

PDN generation creates power and ground structures to distribute supply connections across the design.

The OpenROAD GUI displays power and ground layers, including the `VSS` ground net.

The Inspector identifies the displayed VSS net as routed in the loaded database. However, the presence of power/ground geometry alone does not prove that every supply pin is correctly connected or that power-integrity verification has passed.

The PLL supply-pin warnings require additional investigation.

### 5.5 Global Placement

Global placement estimates standard-cell locations while optimizing objectives such as wirelength and timing.

The reported placement statistics include instance counts, movable and fixed cells, core dimensions, and utilization.

An initial global-placement attempt returned a Make error. Therefore, that particular run must be treated as failed until the first actual error is identified.

A later implementation progressed through detailed placement and CTS.

### 5.6 Detailed Placement

Detailed placement legalizes standard-cell locations after placement optimization.

It checks whether cells satisfy physical placement requirements, including:

- Placement-region restrictions.
- Row alignment.
- Site alignment.
- Adjacent-cell overlap.

The supplied detailed-placement log reports zero issues for the displayed wrong-region, row-alignment, site-alignment, and overlap checks.

These results demonstrate successful checks at that stage, but they do not establish completed routing or final DRC sign-off.

### 5.7 Clock-Tree Synthesis (CTS)

Clock-tree synthesis builds a clock distribution network connecting the clock source to sequential elements.

The CTS process considers the clock sinks and constructs the clock-tree topology.

The supplied log reports:

- Clock net: `CLK`
- Clock sinks: 11,444
- No setup violations found at the reported CTS final stage.
- No hold violations found at the reported CTS final stage.

These are positive, stage-specific results. They do not establish post-route timing closure across all PVT corners.

Clock skew, transition behavior, routing parasitics, timing constraints, and subsequent routing effects must also be considered.

### 5.8 Global Routing

Global routing assigns approximate routing paths to signal nets while considering available routing resources and congestion.

It helps identify routing-resource limitations and potential congestion regions.

The report includes a GUI view from the routing stage in which a displayed net, `net258`, is marked as `ROUTED`.

The scripting console shows commands including:

```tcl
grt::have_routes
estimate_parasitics -global_routing
sta::find_timing
sta::find_requireds
```

These commands are used in the shown session to inspect routing information, estimate parasitic effects, and update timing-analysis information.

A single net marked `ROUTED` does not prove that all nets are routed successfully.

### 5.9 Detailed Routing

Detailed routing creates actual wire and via geometry for signal connections.

After routing, the design should be evaluated using the appropriate routing, parasitic, timing, and physical-verification reports.

The earlier screenshots do not establish a complete, successful detailed-routing run or DRC/LVS sign-off. The later routing-stage GUI provides evidence of routing-related geometry and analysis, but full design-level completion must be confirmed using actual reports.

### 5.10 Parasitic-Aware STA

Interconnect resistance and capacitance affect signal delays.

After routing, the appropriate parasitic data must be extracted or estimated, and STA must be rerun using the corresponding database and constraints.

Timing results depend on:

- The Liberty process corner.
- Operating voltage.
- Operating temperature.
- RC or interconnect corner.
- SDC constraints.
- Available parasitic data.
- The physical-design stage being analyzed.

Timing results from a floorplan or CTS database must not be presented as final post-route timing results.

## 6. Project Workspace and Setup

The workspace used in the report is:

```bash
cd ~/Desktop/OpenROAD-flow-scripts/flow
```

Inspect the current directory:

```bash
pwd
ls
```

Inspect the VSDBabySoC design files:

```bash
ls designs/sky130hd/vsdbabysoc/
```

Inspect the design configuration:

```bash
sed -n '1,240p' designs/sky130hd/vsdbabysoc/config.mk
```

Check the available Make targets:

```bash
make help
```

The design configuration is:

```bash
./designs/sky130hd/vsdbabysoc/config.mk
```

Set the configuration variable:

```bash
export DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk
```

The exact targets supported depend on the installed OpenROAD-flow-scripts checkout.

## 7. Physical-Design Commands

Run the stages one at a time and inspect their logs before proceeding.

### 7.1 Synthesis

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth
```

Purpose:

- Read the RTL and required libraries.
- Perform synthesis and technology mapping.
- Generate design database and constraint outputs.

### 7.2 Floorplanning

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```

The correct target is `floorplan`, not `floorplane`.

The report shows an earlier failure caused by the misspelled target.

### 7.3 Placement

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```

This stage performs placement-related optimization and legalization.

### 7.4 Clock-Tree Synthesis

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```

This stage constructs the clock distribution tree.

### 7.5 Global Routing

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk global_route
```

If the target is unsupported by the installed checkout, inspect `make help` and use the available target name.

The report indicates that routing completion must be confirmed from the actual stage results rather than inferred from a GUI screenshot.

### 7.6 Routing Target

The report also provides this routing command:

```bash
make DESIGN_CONFIG=$DESIGN_CONFIG route
```

Use it only if `route` is a valid target in the installed version. Otherwise, inspect the available targets using `make help`.

### 7.7 Inspect Result Databases

```bash
find results/sky130hd/vsdbabysoc -maxdepth 3 -type f | sort
```

Inspect the base result database:

```bash
find results/sky130hd/vsdbabysoc/base -maxdepth 1 -type f | sort
```

### 7.8 Inspect Generated Reports

```bash
find reports/sky130hd/vsdbabysoc/base -type f | sort
```

### 7.9 Inspect Logs

```bash
find logs/sky130hd/vsdbabysoc -maxdepth 3 -type f | sort
```

Search logs for errors, warnings, timing problems, and routing information:

```bash
grep -RniE 'error|warning|slack|violation|global route|detailed route' \
logs/sky130hd/vsdbabysoc
```

A more focused search for common failure indicators is:

```bash
grep -RniE 'error|failed|violation|unconnected' \
logs/sky130hd/vsdbabysoc/base
```

If a stage fails, identify the first meaningful error in the full log rather than relying only on the final `make Error 2` message.

## 8. Static Timing Analysis (STA)

Static Timing Analysis evaluates whether signals arrive within their required timing limits without requiring functional simulation of every possible input sequence.

STA analyzes timing paths using the design connectivity, cell timing models, clock definitions, input/output constraints, and available interconnect parasitics.

### 8.1 Setup Timing

Setup timing checks whether data arrives early enough before the relevant active clock edge.

Negative setup slack indicates that the analyzed path misses its required timing limit.

### 8.2 Hold Timing

Hold timing checks whether data remains stable for the required interval after the relevant active clock edge.

Positive hold slack indicates that the analyzed hold requirement is satisfied for the reported path or path group.

### 8.3 Timing Slack

Slack represents the difference between required timing and actual timing.

For setup analysis, negative slack indicates a late-arriving path.

For hold analysis, negative slack indicates that the data arrives too early relative to the hold requirement.

The path type, units, constraints, operating corner, and design stage must be recorded whenever a slack value is reported.

### 8.4 STA Reporting Commands

The PDF includes the following illustrative OpenSTA/OpenROAD timing commands:

```tcl
report_checks -path_delay max \
  -format full_clock_expanded \
  -fields {slew cap input_pin net} \
  -group_path_count 10

report_checks -path_delay min \
  -format full_clock_expanded \
  -fields {slew cap input_pin net} \
  -group_path_count 10

report_worst_slack -max
report_worst_slack -min
report_tns
report_clock_skew
report_check_types -max_slew -max_capacitance -max_fanout
```

These commands are intended for a correctly initialized OpenSTA/OpenROAD timing session. They are not a standalone script, and exact command availability can vary with tool version and session setup.

### 8.5 Information to Record for Every Timing Run

For reproducible STA, record:

- Liberty files used.
- Process, voltage, and temperature corner.
- RC or interconnect corner.
- SDC constraints.
- Database and implementation stage.
- Parasitic source.
- Worst setup slack.
- Worst hold slack.
- Total negative slack (TNS).
- Clock skew.
- Unconstrained endpoints.
- Warnings and errors.

The earlier screenshots visibly show the typical `tt_025C_1v80` Liberty file. The later report sections provide the 16-corner setup and hold analysis.

## 9. Multi-Corner STA Across 16 PVT Corners

### 9.1 What Is a PVT Corner?

PVT stands for:

- **Process:** Manufacturing variation affecting transistor and standard-cell behavior.
- **Voltage:** Operating supply voltage.
- **Temperature:** Operating temperature.

Different PVT conditions can change cell delay, signal transition behavior, and the timing margins of a design.

A timing corner may also be paired with an interconnect RC corner to account for wiring resistance and capacitance.

### 9.2 Why Multi-Corner Analysis Is Required

A design that meets timing at one operating corner may fail at another.

Multi-corner STA helps identify:

- Setup-critical operating conditions.
- Hold-critical operating conditions.
- Timing sensitivity to process variation.
- Timing sensitivity to voltage and temperature.
- Operating corners requiring additional optimization.

### 9.3 Recommended Multi-Corner Workflow

1. Define the exact 16-corner matrix.
2. Verify that the required standard-cell Liberty files and macro timing models exist.
3. Validate clock definitions and input/output timing constraints.
4. Load the appropriate design database and parasitic data.
5. Run setup analysis for each required corner.
6. Run hold analysis separately for each required corner.
7. Save the raw timing reports with identifiable corner names.
8. Create a summary table or CSV containing the actual reported values.
9. Plot the reported setup and hold slack values with explicit units.
10. Recheck timing after routing using the appropriate routed parasitics.

Do not duplicate a typical-corner report and rename it as multiple corners. Each corner must be genuinely analyzed with its matching models and constraints.

## 10. Hold Slack Analysis Across 16 PVT Corners

The report includes a graph titled **Hold Slack vs PVT Corners**.

Hold timing verifies that data does not change too soon after the active clock edge.

### Reported Results

| Metric | Reported value |
|---|---:|
| Number of analyzed corners | 16 |
| Minimum hold slack | 0.19 ns |
| Maximum hold slack | 1.68 ns |
| Corner with maximum hold slack | `ss_n40C_1v28` |
| Corner with minimum hold slack | `ff_n40C_1v95` |

<img width="853" height="429" alt="image" src="https://github.com/user-attachments/assets/c936dff7-e101-499d-802d-3517e1025e9f" />

The graph shows positive reported hold slack across all 16 corners.

### Interpretation

The reported hold slack values range from 0.19 ns to 1.68 ns.

Positive hold slack indicates that the reported hold timing requirement is satisfied for the analyzed paths.

The smallest reported hold margin is 0.19 ns at `ff_n40C_1v95`. This is the minimum value shown in the supplied analysis and should be considered when reviewing the margin available at that corner.

However, complete hold sign-off also requires verification that all relevant paths and timing constraints have been checked.

## 11. Setup Slack Analysis Across 16 PVT Corners

The report includes a graph titled **Worst Slack (max) vs PVT Corners**.

Setup timing verifies that data arrives early enough before the active clock edge.

### Reported Results

| Metric | Reported value |
|---|---:|
| Number of analyzed corners | 16 |
| Worst setup slack | -29.05 ns |
| Corner with worst setup slack | `ss_n40C_1v28` |
| Highest setup slack | 9.03 ns |
| Corner with highest setup slack | `ff_100C_1v95` |

<img width="851" height="419" alt="image" src="https://github.com/user-attachments/assets/dc1847cf-17f1-4b9f-b9e2-6d27077bc1d5" />

The graph shows positive setup slack at several corners and negative setup slack at others.

### Interpretation

The worst reported setup slack is -29.05 ns at `ss_n40C_1v28`.

Negative setup slack indicates that the analyzed setup path does not meet its timing requirement. The large negative slack reported at some SS corners suggests a need for further timing investigation and optimization.

The highest reported setup slack is 9.03 ns at `ff_100C_1v95`.

The significant variation across corners demonstrates why checking only a typical operating condition is insufficient for evaluating timing behavior.

## 12. Overall Multi-Corner Observations

The report establishes the following observations:

1. All 16 PVT corners are included in the reported multi-corner analysis.
2. Hold slack is positive across all reported corners.
3. Setup slack varies considerably with process, voltage, and temperature.
4. The `ss_n40C_1v28` corner exhibits the worst setup slack of -29.05 ns.
5. The `ff_100C_1v95` corner exhibits the highest setup slack of 9.03 ns.
6. Multi-corner STA identifies operating conditions that require further timing investigation.

### Main Timing Conclusion

The supplied graphs indicate positive hold margins across the analyzed corners, while negative setup slack at several corners means that setup timing requires further investigation and optimization before timing sign-off.

The reported results must be interpreted together with the actual timing paths, constraints, and implementation stage used for each run.

## 13. Screenshot Evidence and Technical Explanation

The PDF contains implementation screenshots and timing-analysis evidence. The following list preserves their technical topics and the purpose of each view.

### Figures 1–3: Workspace and Source Files

**Figure 1 — Repository and OpenROAD workspace**

Shows the remote Linux workspace used for the physical-design flow. The workspace contains the design configuration, scripts, logs, and result databases.

<img width="1917" height="1198" alt="Screenshot 2026-09-26 103741" src="https://github.com/user-attachments/assets/a8400e53-d0a2-4681-9913-ced9df563698" />

**Figure 2 — VSDBabySoC source directory**

Shows the Verilog/SystemVerilog design files. Before synthesis, the top module and dependent source files must be specified correctly.

<img width="1901" height="1131" alt="Screenshot 2026-09-26 123920" src="https://github.com/user-attachments/assets/ffe6488b-c6b6-4c7f-9d8e-8a51d87fa0f5" />

**Figure 3 — VSDBabySoC design folder in VS Code**

Shows the terminal in the expected design and flow environment. Correct directory navigation allows the scripts to locate platform files and result paths.

<img width="1917" height="1136" alt="Screenshot 2026-09-26 115433" src="https://github.com/user-attachments/assets/7f1f6a11-a3f1-412e-a533-93b958aa35d6" />

### Figures 4–6: GUI Timing and Net Inspection

**Figure 4 — OpenROAD GUI with timing commands**

Shows a floorplan database and loaded SDC constraints. GUI timing views should be supported by saved text reports that identify the analyzed stage and constraints.

<img width="720" height="425" alt="image" src="https://github.com/user-attachments/assets/6913a39c-3479-4651-ae41-d43621f5bd71" />

**Figure 5 — Timing Report**

Shows a setup slack value of approximately -0.156. The units depend on the loaded constraints and timing session. The path type, units, corner, and SDC must be confirmed before using this value in a report.

<img width="717" height="427" alt="image" src="https://github.com/user-attachments/assets/93eb94c4-f66a-4b0a-9d87-a0f539de5cf7" />

**Figure 6 — VSS ground net**

The Inspector identifies the VSS net as a ground net and marks it as routed in the loaded database. This provides evidence of power/ground geometry but does not independently establish complete supply connectivity or power-integrity sign-off.

<img width="719" height="429" alt="image" src="https://github.com/user-attachments/assets/a41904c6-ead6-4980-b5fb-9914295aba74" />

### Figures 7–10: Synthesis and Floorplanning

**Figure 7 — Synthesis-related terminal output**

Shows a flow command and the technology-library information used by the run. The complete log should be retained for debugging.

<img width="719" height="428" alt="image" src="https://github.com/user-attachments/assets/0f78a409-544f-4152-ac7f-6781de755163" />

**Figure 8 — Synthesis outputs**

Shows the design being linked and database/constraint artifacts being written. These outputs are inputs to later physical-design stages.

<img width="719" height="430" alt="image" src="https://github.com/user-attachments/assets/67af8f93-e9d9-4a45-ac8e-2d3714a215de" />

**Figure 9 — Invalid floorplan target**

Shows the error caused by the misspelled target `floorplane`. The correct target is `floorplan`.

<img width="719" height="428" alt="image" src="https://github.com/user-attachments/assets/5c7d4776-44f8-4014-99d7-a4df7a9206d5" />

**Figure 10 — Tapcell and PDN preparation**

Shows floorplan-stage outputs being copied into the base result directory. Supply-pin warnings still need to be reviewed.

<img width="719" height="429" alt="image" src="https://github.com/user-attachments/assets/9476807d-a6d0-4be8-aa8b-44a10afcfbf0" />

### Figures 11–12: Macro and Power/Ground Geometry

**Figure 11 — DAC macro in the floorplan**

Shows the DAC hard macro and its physical placement. Macro placement affects routing channels, pin access, and congestion.

<img width="719" height="427" alt="image" src="https://github.com/user-attachments/assets/73c92165-1ea5-451e-a466-a4f7aaa512b3" />

**Figure 12 — Power/ground layers**

Shows visible metal layers and power/ground structures. A layout image alone does not prove that routing is DRC-clean.

<img width="719" height="428" alt="image" src="https://github.com/user-attachments/assets/cde18cf1-ebbf-4378-ad06-a71e20df193f" />

### Figures 13–15: Placement

**Figure 13 — Global placement statistics**

Shows placement statistics and an initial Make error. That run must be treated as failed until the first error is identified.

<img width="714" height="382" alt="image" src="https://github.com/user-attachments/assets/79d910b4-d2d8-4db7-b896-7fc59412b3de" />

**Figure 14 — Detailed placement results**

Shows placement legalization checks and area/utilization metrics. The displayed checks report no wrong-region, row-alignment, site-alignment, or adjacent-cell overlap problems.

<img width="718" height="389" alt="image" src="https://github.com/user-attachments/assets/b18ad9f5-4993-49e2-ac0a-c414870e0bf0" />

**Figure 15 — Placed standard cells and DAC macro**

Shows the placed design in the OpenROAD GUI, including a standard cell with `PLACED` status. This is a placement-stage snapshot.

<img width="719" height="388" alt="image" src="https://github.com/user-attachments/assets/24207b77-b2b4-4869-b0c8-2f28b8668573" />

### Figures 16–19: Clock-Tree Synthesis and Heat Map

**Figure 16 — CTS start**

Shows the `CLK` net and the clock sinks used by the clock-tree synthesis stage.

<img width="719" height="388" alt="image" src="https://github.com/user-attachments/assets/19d48282-9dc5-4822-a719-ea57a57da9d7" />

**Figure 17 — CTS final timing checks**

Reports no setup violations and no hold violations at the displayed CTS final stage. This is not proof of post-route timing closure across all corners.

<img width="719" height="387" alt="image" src="https://github.com/user-attachments/assets/9669843e-5fab-4c71-b4f6-a4efa1cfb145" />

**Figure 18 — Post-CTS GUI**

Shows the placed design with routing layers visible. The image does not independently establish route completion or clock sign-off.

<img width="720" height="386" alt="image" src="https://github.com/user-attachments/assets/03a0a423-aa1d-4ec8-a755-bf8c7d02071a" />

**Figure 19 — Heat-map view**

Shows a localized concentration near the DAC macro. The selected heat-map metric must be identified before interpreting the displayed colors as congestion or another physical-design issue.

<img width="719" height="382" alt="image" src="https://github.com/user-attachments/assets/6fb72709-d362-4164-aa8a-d561a5c8cf5a" />

### Figures 20–22: Macro Blockages, Sequential Cells, and Technology Layers

**Figure 20 — DAC macro and blockages**

Shows the DAC macro and visible routing/placement blockages. Blockages can restrict routing resources and influence congestion around a macro.

<img width="719" height="426" alt="image" src="https://github.com/user-attachments/assets/aa2498ad-8fe5-43ca-b901-4b330e335560" />

**Figure 21 — Sequential standard cell**

Shows a sequential standard cell associated with the CPU register logic. Sequential elements store state and are central to clocked timing analysis.

<img width="719" height="429" alt="image" src="https://github.com/user-attachments/assets/5abd8ffd-4374-4f2c-8598-33e336630c4f" />

**Figure 22 — Technology-layer information**

Shows the floorplan and a technology-layer panel identifying a via layer and its properties. Technology-layer information contributes to routing and parasitic estimation.

<img width="719" height="424" alt="image" src="https://github.com/user-attachments/assets/08a10202-e6cf-4b50-99ec-29e59d4f582b" />

### Figure 23: Global Routing and Routed-Net Inspection

Shows the VSDBabySoC design in the OpenROAD GUI at the `5_route` stage using SKY130HD technology.

The layout displays metal layers, vias, and interconnect geometry. The Inspector marks `net258` as `ROUTED`.

The console shows routing-related and timing-analysis commands, including:

```tcl
grt::have_routes
estimate_parasitics -global_routing
sta::find_timing
sta::find_requireds
```

<img width="859" height="466" alt="image" src="https://github.com/user-attachments/assets/8629ddae-0a1b-403b-bd07-3b57d8d89149" />

This provides evidence of routed-net geometry and global-routing-related timing analysis in the loaded database.

However, a single routed net does not establish that all nets are routed or that DRC/LVS verification has passed.

### Multi-Corner Timing Graphs

The final pages contain the following additional evidence:

- A terminal view associated with the 16-corner timing reports.
- A Hold Slack vs PVT Corners graph.
- A Worst Slack (max) vs PVT Corners graph.
- A summary of the observed timing behavior.
- A conclusion identifying the need for setup timing optimization.

<img width="862" height="468" alt="image" src="https://github.com/user-attachments/assets/7808b322-80fe-45b2-b641-f2ce9b595167" />

The graphs report positive hold slack across all 16 corners and negative setup slack at some corners, with the worst reported setup slack at `ss_n40C_1v28`.

## 14. Routing Debug Checklist

If a routing stage fails, use the following checklist:

- Re-run the failing target and save the full terminal output.
- Identify the first actual error rather than relying only on the final Make error.
- Check routing congestion reports.
- Inspect macro placement and blockage geometry.
- Review pin-access limitations.
- Check the LEF and technology files.
- Review routing-resource availability and execution resource/time limits.
- Identify the specific global-routing failure from the actual log.
- Do not treat a heat-map image as proof of successful routing.
- After routing succeeds, generate the appropriate parasitic data.
- Run setup and hold STA for every required corner.
- Preserve the generated reports as evidence.

## 15. STA and PVT-Corner Evidence Checklist

Before presenting the project as timing-verified, ensure that:

- The correct Liberty files are used for each corner.
- Macro timing models are compatible with the analysis.
- SDC clocks and I/O constraints are valid.
- The correct database and parasitic data are loaded.
- Setup and hold reports are saved separately for every required PVT corner.
- Process, voltage, and temperature are recorded in report filenames.
- The reported slack values come from actual timing runs.
- The 16-corner summary matches the raw reports.
- Post-route timing is regenerated using appropriate routed parasitics.
- Unconstrained endpoints, warnings, and errors are reviewed.
- DRC and LVS status is supported by separate verification reports.

A layout screenshot is not sufficient evidence for DRC/LVS sign-off.

## 16. Results Summary

| Implementation or analysis stage | Evidence and observation |
|---|---|
| Synthesis | Design linking and ODB/SDC output artifacts are shown |
| Floorplanning | Floorplan database and constraints are shown |
| Tapcell and PDN | Stage outputs are shown; PLL supply-pin warnings need review |
| Global placement | An initial run failed; a later run progressed |
| Detailed placement | Displayed legalization checks report zero issues in the listed categories |
| CTS | 11,444 clock sinks; no setup or hold violations reported at the displayed CTS final stage |
| Routing inspection | Routed-net geometry and routing-related analysis are shown |
| Hold STA | Positive reported hold slack across 16 corners, ranging from 0.19 ns to 1.68 ns |
| Setup STA | Setup slack ranges from -29.05 ns to 9.03 ns in the reported corner analysis |
| Timing sign-off | Further setup timing investigation and optimization are required |
| DRC/LVS | Separate sign-off reports are not established by the supplied evidence |

## 17. Conclusion

Static Timing Analysis of the SKY130HD VSDBabySoC design is reported across 16 PVT corners. The project covers synthesis, floorplanning, placement, clock-tree synthesis, routing-related inspection, and multi-corner timing evaluation.

The reported hold slack is positive across all 16 corners, ranging from 0.19 ns to 1.68 ns. However, setup slack is negative at several corners, with the worst reported value of -29.05 ns at `ss_n40C_1v28`. The highest reported setup slack is 9.03 ns at `ff_100C_1v95`.

These results demonstrate the importance of multi-corner STA in identifying critical operating conditions. The next priorities are to investigate setup timing failures, confirm successful routing, analyze timing with appropriate routed parasitics, and retain the required verification reports.
