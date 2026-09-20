# ============================================================
# SKY130_D2_SK4
# GENERAL TIMING CHARACTERIZATION PARAMETERS
# ============================================================
# ------------------------------------------------------------
# README
# ------------------------------------------------------------

cat > README.md <<'EOF'
# SKY130_D2_SK4 - General Timing Characterization Parameters

## Topics

1. Timing threshold definitions
2. Propagation delay and transition time

---

## 1. Timing Threshold Definitions

Timing characterization uses voltage thresholds to measure signal
transitions.

Common threshold levels include:

- 10% of VDD
- 50% of VDD
- 90% of VDD

These thresholds are used for measuring signal transition and
propagation delay.

---

## Rise Transition

A rising signal moves from a low voltage to a high voltage.

Rise transition can be measured between defined low and high
voltage thresholds.

---

## Fall Transition

A falling signal moves from a high voltage to a low voltage.

Fall transition is measured between defined voltage thresholds.

---

## 2. Propagation Delay and Transition Time

Propagation delay is the time difference between an input
transition and the corresponding output transition.

Transition time describes how quickly a signal changes between
specified voltage thresholds.

---

# IMPORTANT PARAMETERS

## Input Slew

The rate at which the input signal changes.

## Output Load

The capacitance driven by the output of the cell.

## Propagation Delay

Time difference between input and output timing thresholds.

## Rise Transition

Time required for the output to move from the low threshold
to the high threshold.

## Fall Transition

Time required for the output to move from the high threshold
to the low threshold.

## Setup Time

Minimum time for which data must be stable before the active
clock edge.

## Hold Time

Minimum time for which data must remain stable after the active
clock edge.

---

# DELAY DEPENDENCE

Cell delay depends on:

- Input slew
- Output capacitance
- Supply voltage
- Process corner
- Temperature
- Cell architecture

---

# CONCLUSION

Timing characterization provides the delay and transition
information required for timing analysis and digital physical
design.
EOF


# ------------------------------------------------------------
# TIMING PARAMETERS FILE
# ------------------------------------------------------------

cat > timing_parameters.txt <<'EOF'
============================================================
SKY130 GENERAL TIMING CHARACTERIZATION PARAMETERS
============================================================

INPUT SLEW
Rate at which the input signal changes.

OUTPUT LOAD
Capacitance driven by the cell output.

PROPAGATION DELAY
Time between input and output timing thresholds.

RISE TRANSITION
Time taken by output to move from low threshold to high threshold.

FALL TRANSITION
Time taken by output to move from high threshold to low threshold.

SETUP TIME
Required data stability before the active clock edge.

HOLD TIME
Required data stability after the active clock edge.

============================================================
EOF


echo "SKY130_D2_SK4 CREATED"
find . -maxdepth 2 -type f | sort
