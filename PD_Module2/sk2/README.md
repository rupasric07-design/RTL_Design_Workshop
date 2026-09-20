
# SKY130_D2_SK2 - Library Binding and Placement

## Anurag Institute Chip Design Program - Physical Design

This section covers library binding, initial placement, placement
optimization, library requirements and congestion-aware placement.

## Topics Covered

1. Netlist binding and initial placement
2. Optimize placement using estimated wire-length and capacitance
3. Final placement optimization
4. Need for libraries and characterization
5. Congestion-aware placement using RePlAce

---

# 1. NETLIST BINDING AND INITIAL PLACEMENT

After synthesis, the RTL is converted into a gate-level netlist.

The cells in the netlist are mapped to standard cells available
in the technology library.

The placement stage assigns physical locations to the standard
cells inside the floorplan.

Main objectives:

- Connect all required cells
- Reduce wire length
- Reduce congestion
- Meet timing requirements

---

# 2. OPTIMIZE PLACEMENT USING ESTIMATED WIRE-LENGTH AND CAPACITANCE

During placement, estimated wire length is used to evaluate
possible cell locations.

Shorter interconnect generally helps reduce:

- Wire resistance
- Wire capacitance
- Propagation delay
- Routing congestion

Placement optimization considers timing and estimated
interconnect capacitance.

---

# 3. FINAL PLACEMENT OPTIMIZATION

After initial placement, optimization is performed to improve
the physical implementation.

The placement tool can modify cell locations to improve:

- Timing
- Wire length
- Congestion
- Cell density

The final placement should provide sufficient routing resources
for the next physical-design stages.

---

# 4. NEED FOR LIBRARIES AND CHARACTERIZATION

Technology libraries provide information required for physical
design and timing analysis.

Important library information includes:

- Cell area
- Cell dimensions
- Pin information
- Timing information
- Power information
- Input capacitance
- Output characteristics

Characterization determines electrical and timing behaviour of
standard cells under specified conditions.

---

# 5. CONGESTION-AWARE PLACEMENT USING REPLACE

RePlAce is a placement engine used in OpenLANE.

It performs placement while considering:

- Cell density
- Wire length
- Routing congestion
- Timing-related objectives

Congestion-aware placement helps prevent highly crowded regions
that may create routing difficulties.

---

# PLACEMENT FLOW

RTL
  |
  v
SYNTHESIS
  |
  v
GATE-LEVEL NETLIST
  |
  v
LIBRARY BINDING
  |
  v
INITIAL PLACEMENT
  |
  v
PLACEMENT OPTIMIZATION
  |
  v
CONGESTION-AWARE PLACEMENT
  |
  v
FINAL PLACEMENT

---

# OPENLANE PLACEMENT FLOW

The basic OpenLANE flow is:

./flow.tcl -interactive

package require openlane 0.9

prep -design picorv32a

run_synthesis

run_floorplan

run_placement

---

# CONCLUSION

Library binding connects synthesized logic to available technology
cells. Placement then determines physical cell locations while
optimizing wire length, capacitance, timing and congestion.
RePlAce performs congestion-aware placement to produce a suitable
physical arrangement for subsequent routing stages.
EOF
