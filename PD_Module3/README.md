# ============================================================
# SKY130 MODULE 3 - DESIGN LIBRARY CELL USING MAGIC & NGSPICE
# ============================================================

mkdir -p Sky130-Module-3/{SKY130_D3_SK1/results,SKY130_D3_SK2,SKY130_D3_SK3,screenshots}
cd Sky130-Module-3


# ============================================================
# README.md
# ============================================================

cat > README.md <<'EOF'
# Sky130 Module 3 - Design Library Cell using Magic Layout and ngspice Characterization

## Objective

This module covers the design, simulation, layout, extraction and characterization of a CMOS inverter using the Sky130 technology.

## Module Contents

### SKY130_D3_SK1 - Labs for CMOS Inverter ngspice Simulations

- SKY_L0 - IO placer revision
- SKY_L1 - SPICE deck creation for CMOS inverter
- SKY_L2 - SPICE simulation lab for CMOS inverter
- SKY_L3 - Switching Threshold Vm
- SKY_L4 - Static and dynamic simulation of CMOS inverter
- SKY_L5 - Lab steps to git clone vsdstdcelldesign

### SKY130_D3_SK2 - Inception of Layout - CMOS Fabrication Process

- SKY_L1 - Create Active regions
- SKY_L2 - Formation of N-well and P-well
- SKY_L3 - Formation of gate terminal
- SKY_L4 - Lightly doped drain formation
- SKY_L5 - Source-drain formation
- SKY_L6 - Local interconnect formation
- SKY_L7 - Higher-level metal formation
- SKY_L8 - Sky130 basic layers and LEF using inverter
- SKY_L9 - Create standard-cell layout and extract SPICE netlist

### SKY130_D3_SK3 - Sky130 Tech File Labs

- SKY_L1 - Final SPICE deck using Sky130 technology
- SKY_L2 - Characterize inverter using Sky130 model files
- SKY_L3 - Magic tool options and DRC rules
- SKY_L4 - Sky130 PDK download and lab setup
- SKY_L5 - Load Sky130 technology rules in Magic
- SKY_L6 - Fix poly.9 error
- SKY_L7 - Poly resistor spacing
- SKY_L8 - DRC error as geometrical construct
- SKY_L9 - Missing/incorrect DRC rules

## Tools Used

- ngspice
- Magic
- Sky130 PDK
- Git
- Linux
- SPICE

## Main Design

CMOS Inverter

Inputs:
- A / IN

Outputs:
- Y / OUT

Power:
- VDD
- GND

## Expected Flow

SPICE schematic
        |
        v
ngspice simulation
        |
        v
CMOS inverter layout
        |
        v
Magic DRC
        |
        v
SPICE extraction
        |
        v
Post-layout simulation
        |
        v
Characterization

## Author

Sky130 Module 3 Submission
EOF


# ============================================================
# SKY130_D3_SK1/inverter.spice
# ============================================================

cat > SKY130_D3_SK1/inverter.spice <<'EOF'
* SKY130 CMOS Inverter - Basic SPICE Deck

.title SKY130 CMOS Inverter

.param VDD=1.8

VDD vdd 0 {VDD}
VIN in  0 PULSE(0 {VDD} 0 10p 10p 5n 10n)

* PMOS
M1 out in vdd vdd sky130_fd_pr__pfet_01v8
+ L=0.15u W=1.0u

* NMOS
M2 out in 0 0 sky130_fd_pr__nfet_01v8
+ L=0.15u W=0.5u

Cload out 0 10f

.model sky130_fd_pr__pfet_01v8 PMOS
.model sky130_fd_pr__nfet_01v8 NMOS

.tran 10p 50n

.control
run
plot v(in) v(out)
.endc

.end
EOF


# ============================================================
# SKY130_D3_SK1/inverter_tb.spice
# ============================================================

cat > SKY130_D3_SK1/inverter_tb.spice <<'EOF'
* CMOS Inverter Testbench

.title CMOS Inverter Testbench

VDD vdd 0 1.8
VIN in 0 PULSE(0 1.8 0 10p 10p 5n 10n)

Mpmos out in vdd vdd PMOS W=1u L=0.15u
Mnmos out in 0 0 NMOS W=0.5u L=0.15u

Cload out 0 10f

.model PMOS PMOS
.model NMOS NMOS

.tran 10p 50n

.control
run
write inverter.raw v(in) v(out)
plot v(in) v(out)
.endc

.end
EOF


# ============================================================
# SKY130_D3_SK1/simulation_commands.txt
# ============================================================

cat > SKY130_D3_SK1/simulation_commands.txt <<'EOF'
# Basic ngspice simulation

ngspice inverter.spice

# Inside ngspice:

run
plot v(in) v(out)

# DC transfer characteristic

dc VIN 0 1.8 0.01

# Transient analysis

tran 10p 50n

# Exit

quit
EOF


# ============================================================
# SKY130_D3_SK1/results/README.md
# ============================================================

cat > SKY130_D3_SK1/results/README.md <<'EOF'
# Simulation Results

Place the following simulation screenshots in this directory:

1. CMOS inverter input-output waveform
2. DC transfer characteristic
3. Switching threshold Vm
4. Rise and fall transition
5. Static power behavior
6. Dynamic switching behavior

The screenshots should be captured from ngspice.
EOF


# ============================================================
# SKY130_D3_SK2/inverter.spice
# ============================================================

cat > SKY130_D3_SK2/inverter.spice <<'EOF'
* Extracted CMOS Inverter SPICE Netlist
* Sky130 standard-cell layout

.subckt inverter A Y VPWR VGND

* PMOS
M1 Y A VPWR VPWR sky130_fd_pr__pfet_01v8

* NMOS
M2 Y A VGND VGND sky130_fd_pr__nfet_01v8

.ends inverter
EOF


# ============================================================
# SKY130_D3_SK2/layout_notes.md
# ============================================================

cat > SKY130_D3_SK2/layout_notes.md <<'EOF'
# CMOS Inverter Layout

## Main Layers

The CMOS inverter layout contains:

- N-well
- P-diffusion
- N-diffusion
- Poly
- Local interconnect
- Metal1
- Contacts
- Power rail
- Ground rail

## Layout Structure

PMOS is placed inside the N-well.

NMOS is placed in the P-substrate.

The polysilicon gate is shared by PMOS and NMOS.

The drains are connected together to form the output.

The PMOS source connects to VPWR.

The NMOS source connects to VGND.

## Standard Cell Structure

Top:
VPWR

PMOS region

Output

NMOS region

Bottom:
VGND

The layout should maintain correct Sky130 design-rule spacing.
EOF


# ============================================================
# SKY130_D3_SK2/inverter.mag
# ============================================================

cat > SKY130_D3_SK2/inverter.mag <<'EOF'
magic
tech sky130A
timestamp 0

<< nwell >>
rect 0 0 100 100

<< ndiff >>
rect 20 40 80 70

<< pdiff >>
rect 20 10 80 40

<< poly >>
rect 45 10 55 70

<< locali >>
rect 20 35 45 45
rect 55 35 80 45

<< metal1 >>
rect 0 70 100 80
rect 0 0 100 10
rect 45 35 55 45

<< ndcontact >>
rect 25 40 35 50

<< pdcontact >>
rect 25 20 35 30

<< metal1 >>
rect 20 75 80 80
rect 20 0 80 5

<< end >>
EOF


# ============================================================
# SKY130_D3_SK2/extracted.spice
# ============================================================

cat > SKY130_D3_SK2/extracted.spice <<'EOF'
* Post-layout extracted inverter netlist

.subckt inverter A Y VPWR VGND

M1 Y A VPWR VPWR sky130_fd_pr__pfet_01v8
+ L=0.15u W=1.0u

M2 Y A VGND VGND sky130_fd_pr__nfet_01v8
+ L=0.15u W=0.5u

Cpar Y VGND 10f

.ends inverter
EOF


# ============================================================
# SKY130_D3_SK2/magic_commands.txt
# ============================================================

cat > SKY130_D3_SK2/magic_commands.txt <<'EOF'
# Start Magic with Sky130 technology

magic -T sky130A inverter.mag

# Display layout

load inverter.mag

# Check DRC

drc check

# Show DRC errors

drc why

# Extract SPICE

extract all

ext2spice lvs

ext2spice

# Save the layout

writeall
EOF


# ============================================================
# SKY130_D3_SK3/sky130_inverter.spice
# ============================================================

cat > SKY130_D3_SK3/sky130_inverter.spice <<'EOF'
* Sky130 CMOS Inverter Characterization Deck

.title Sky130 CMOS Inverter Characterization

.param VDD=1.8

VDD VPWR 0 {VDD}

VIN A 0 PULSE(0 {VDD} 0 10p 10p 5n 10n)

* PMOS
M1 Y A VPWR VPWR sky130_fd_pr__pfet_01v8
+ L=0.15u W=1.0u

* NMOS
M2 Y A 0 0 sky130_fd_pr__nfet_01v8
+ L=0.15u W=0.5u

CLOAD Y 0 10f

.tran 10p 50n

.control
run
plot v(A) v(Y)
.endc

.end
EOF


# ============================================================
# SKY130_D3_SK3/characterization.spice
# ============================================================

cat > SKY130_D3_SK3/characterization.spice <<'EOF'
* CMOS Inverter DC Characterization

VDD VDD 0 1.8
VIN IN 0 0

M1 OUT IN VDD VDD PMOS W=1u L=0.15u
M2 OUT IN 0 0 NMOS W=0.5u L=0.15u

.model PMOS PMOS
.model NMOS NMOS

.dc VIN 0 1.8 0.01

.control
run
plot v(IN) v(OUT)

* Switching threshold can be obtained
* from the point where VIN approximately equals VOUT.

.endc

.end
EOF


# ============================================================
# SKY130_D3_SK3/drc_commands.txt
# ============================================================

cat > SKY130_D3_SK3/drc_commands.txt <<'EOF'
# Magic Sky130 DRC commands

magic -T sky130A inverter.mag

# Inside Magic:

drc check
drc count
drc why

# Search for DRC errors

drc find

# Display the next DRC error

drc next

# Clear DRC markers

drc off

# Re-enable DRC

drc on
EOF


# ============================================================
# SKY130_D3_SK3/poly9_fix.md
# ============================================================

cat > SKY130_D3_SK3/poly9_fix.md <<'EOF'
# Poly.9 DRC Error

## Purpose

The poly.9 rule checks the required spacing and geometrical relationship
between polysilicon structures according to the Sky130 technology rules.

## Debugging Procedure

1. Open the layout in Magic.
2. Run:

drc check

3. Locate the poly.9 error.
4. Run:

drc why

5. Inspect the highlighted geometry.
6. Measure the distance between the relevant poly shapes.
7. Modify the layout according to the Sky130 design-rule requirement.
8. Run DRC again.

## Verification

The layout must be rechecked after modification.

Commands:

drc check
drc count
drc why

The final layout should have no remaining unintended DRC violations.
EOF


# ============================================================
# SKY130_D3_SK3/poly_resistor_spacing.md
# ============================================================

cat > SKY130_D3_SK3/poly_resistor_spacing.md <<'EOF'
# Poly Resistor Spacing to Diffusion and Tap

The poly resistor layout must maintain the spacing required by the
Sky130 technology design rules.

Important checks include:

- Poly to diffusion spacing
- Poly to tap spacing
- Poly width
- Poly enclosure
- Contact spacing
- Diffusion spacing

## Magic Verification

Use:

drc check

Then inspect each reported error using:

drc why

After fixing the geometry, run:

drc check

again.
EOF


# ============================================================
# SKY130_D3_SK3/drc_geometry.md
# ============================================================

cat > SKY130_D3_SK3/drc_geometry.md <<'EOF'
# DRC Error as a Geometrical Construct

A DRC error is generally caused by a violation of a geometrical
relationship between layout layers.

Examples include:

- Minimum width violation
- Minimum spacing violation
- Enclosure violation
- Extension violation
- Overlap violation
- Contact size violation

## Debugging Method

1. Run DRC.
2. Select the reported error.
3. Use "drc why".
4. Identify the layers involved.
5. Measure the geometry.
6. Compare it with the Sky130 rule.
7. Modify the geometry.
8. Run DRC again.

This process converts a textual DRC error into a physical layout
geometry that can be corrected.
EOF


# ============================================================
# SKY130_D3_SK3/missing_rules.md
# ============================================================

cat > SKY130_D3_SK3/missing_rules.md <<'EOF'
# Missing or Incorrect DRC Rules

## Debugging Approach

When a DRC result appears incorrect:

1. Identify the reported rule.
2. Identify the layers involved.
3. Inspect the corresponding Sky130 rule.
4. Check the Magic technology file.
5. Verify layer names and layer properties.
6. Compare the rule with the physical layout.
7. Correct the technology rule only when the lab specifically
   requires a technology-file modification.
8. Re-run DRC.

## Verification

Always verify the final result with:

drc check
drc count

The purpose is to ensure that the layout is interpreted correctly
by Magic using the Sky130 technology rules.
EOF


# ============================================================
# SKY130_D3_SK3/magic_setup.txt
# ============================================================

cat > SKY130_D3_SK3/magic_setup.txt <<'EOF'
# Sky130 Magic Setup

# Check whether Magic is installed

magic -version

# Start Magic using Sky130 technology

magic -T sky130A

# Load a layout

load ../SKY130_D3_SK2/inverter.mag

# DRC

drc check
drc count
drc why

# Extraction

extract all
ext2spice

# Save

writeall
EOF


# ============================================================
# GIT SETUP
# ============================================================

git init

git add .

git commit -m "Add Sky130 Module 3 design library cell and characterization labs"


echo "============================================================"
echo " SKY130 MODULE 3 COMPLETED SUCCESSFULLY"
echo "============================================================"
