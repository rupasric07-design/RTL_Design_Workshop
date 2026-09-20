# =======================================
# SKY130_D2_SK3
# CELL DESIGN AND CHARACTERIZATION FLOWS
# =======================================


# ------------------------------------------------------------
# README
# ------------------------------------------------------------


# SKY130_D2_SK3 - Cell Design and Characterization Flows

## Topics

1. Inputs for cell design flow
2. Circuit design step
3. Layout design step
4. Typical characterization flow

---

## 1. Inputs for Cell Design Flow

Important inputs include:

- PDK
- Technology rules
- Cell specification
- Supply voltage
- Timing requirements
- Drive strength
- Layout rules

---

## 2. Circuit Design Step

The standard cell is designed at transistor level.

Typical flow:

Specification
      |
      v
Transistor Design
      |
      v
Schematic
      |
      v
SPICE Simulation
      |
      v
Functional Verification

Important parameters include:

- Delay
- Rise time
- Fall time
- Power
- Input capacitance

---

## 3. Layout Design Step

The transistor-level circuit is converted into physical layout.

Typical flow:

Schematic
      |
      v
Layout
      |
      v
DRC
      |
      v
LVS
      |
      v
Parasitic Extraction

---

## 4. Typical Characterization Flow

Cell Layout
      |
      v
DRC / LVS
      |
      v
SPICE Simulation
      |
      v
Timing Characterization
      |
      v
Power Characterization
      |
      v
Liberty File

The resulting library information is used by synthesis,
placement, routing and timing-analysis tools.

---

# CONCLUSION

Standard-cell design converts a logical cell specification into
a transistor-level circuit and physical layout. Characterization
then generates timing and power information required by the
digital implementation flow.
EOF


# ------------------------------------------------------------
# CELL DESIGN FLOW
# ------------------------------------------------------------

cat > cell_design_flow.txt <<'EOF'
============================================================
SKY130 STANDARD CELL DESIGN AND CHARACTERIZATION
============================================================

PDK
 |
 v
CELL SPECIFICATION
 |
 v
CIRCUIT DESIGN
 |
 v
SPICE SIMULATION
 |
 v
LAYOUT DESIGN
 |
 v
DRC
 |
 v
LVS
 |
 v
PARASITIC EXTRACTION
 |
 v
CHARACTERIZATION
 |
 v
LEF / GDS / LIB
'### ⭐ Day 03 Complete'
============================================================

