
# SKY130_D2_SK1 - Chip Floor Planning Considerations

## Anurag Institute Chip Design Program - Physical Design

This section covers the fundamentals of chip floorplanning and
the initial physical-design flow using SKY130 and OpenLANE.

## Topics Covered

1. Utilization factor and aspect ratio
2. Concept of pre-placed cells
3. De-coupling capacitors
4. Power planning
5. Pin placement and logical cell placement blockage
6. Steps to run floorplan using OpenLANE
7. Review floorplan files and steps to view floorplan
8. Review floorplan layout in Magic

---

# 1. UTILIZATION FACTOR AND ASPECT RATIO

## Utilization Factor

Utilization represents the fraction of the available core area
occupied by standard cells.

Utilization = Cell Area / Core Area

High utilization can result in routing congestion.

## Aspect Ratio

Aspect Ratio = Core Height / Core Width

An aspect ratio of 1 represents a square core.

---

# 2. CONCEPT OF PRE-PLACED CELLS

Pre-placed cells or macros are large blocks whose locations are
decided before standard-cell placement.

Examples:

- Memory
- PLL
- Analog IP
- Large macros

Their placement affects routing, timing and congestion.

---

# 3. DE-COUPLING CAPACITORS

Decoupling capacitors help maintain a stable local supply voltage
during switching activity.

They provide local charge and help reduce supply-voltage
fluctuations.

---

# 4. POWER PLANNING

Power planning distributes VDD and VSS throughout the chip.

Typical structures include:

- Power rings
- Power straps
- Standard-cell power rails

Proper power planning helps provide reliable power to the cells.

---

# 5. PIN PLACEMENT AND LOGICAL CELL PLACEMENT BLOCKAGE

I/O pins should be placed considering:

- Connectivity
- Timing
- Routing distance
- Congestion

Placement blockages prevent standard cells from being placed in
reserved regions.

---

# 6. STEPS TO RUN FLOORPLAN USING OPENLANE

Start OpenLANE:

./flow.tcl -interactive

Load OpenLANE:

package require openlane 0.9

Prepare design:

prep -design picorv32a

Run synthesis:

run_synthesis

Run floorplan:

run_floorplan

---

# 7. REVIEW FLOORPLAN FILES

After running floorplanning, check the run directory:

ls -ltr

Typical floorplan result directory:

designs/picorv32a/runs/<RUN_TAG>/results/floorplan/

Check generated files:

ls -ltr

View the DEF file:

less <floorplan.def>

Press q to exit.

---

# 8. REVIEW FLOORPLAN LAYOUT IN MAGIC

Magic is used to view and inspect the physical layout.

The floorplan can be loaded using:

- SKY130 technology file
- LEF file
- DEF file

Typical command:

magic -T <SKY130_TECH_FILE> \
lef read <MERGED_LEF> \
def read <FLOORPLAN_DEF> &

---

# MAGIC FLOORPLAN SCREENSHOTS

## Floorplan Magic View

![Floorplan Magic](images/floorplan_magic.png)

## Detailed Magic Layout

![Detailed Magic Layout](images/detailed_magic_layout.png)

---

# FLOORPLAN CHECKLIST

- [x] Utilization factor
- [x] Aspect ratio
- [x] Pre-placed cells
- [x] De-coupling capacitors
- [x] Power planning
- [x] Pin placement
- [x] Placement blockage
- [x] OpenLANE floorplan
- [x] Floorplan DEF review
- [x] Magic layout review
- [x] Magic screenshots included

---

# CONCLUSION

Chip floorplanning determines the physical organization of a
design before placement and routing. Proper utilization, aspect
ratio, macro placement, pin placement and power planning help
provide sufficient resources for routing and physical-design
optimization.
EOF

cat > floorplan_config.tcl <<'EOF'
# ============================================================
# SKY130 FLOORPLAN CONFIGURATION
# ============================================================

set ::env(FP_CORE_UTIL) 50
set ::env(FP_ASPECT_RATIO) 1.0
set ::env(FP_CORE_MARGIN) 4
set ::env(FP_IO_MODE) 1

set ::env(FP_CORE_VMETAL) 4
set ::env(FP_CORE_HMETAL) 3

set ::env(PL_TARGET_DENSITY) 0.60
EOF

cat > run_floorplan.tcl <<'EOF'
# ============================================================
# SKY130 OPENLANE FLOORPLAN FLOW
# ============================================================

package require openlane 0.9

prep -design picorv32a

run_synthesis

run_floorplan
EOF

cat > view_floorplan_magic.sh <<'EOF'
#!/bin/bash

TECH_FILE="<PATH_TO_SKY130A>/libs.tech/magic/sky130A.tech"
MERGED_LEF="<PATH_TO_RUN>/tmp/merged.lef"
FLOORPLAN_DEF="<PATH_TO_RUN>/results/floorplan/<FLOORPLAN_DEF>"

magic -T "$TECH_FILE" \
lef read "$MERGED_LEF" \
def read "$FLOORPLAN_DEF" &
EOF

chmod +x view_floorplan_magic.sh

cat > COMMANDS.txt <<'EOF'
============================================================
SKY130_D2_SK1 COMMANDS
============================================================

./flow.tcl -interactive

package require openlane 0.9

prep -design picorv32a

run_synthesis

run_floorplan

ls -ltr

cd designs/picorv32a/runs/<RUN_TAG>/results/floorplan

ls -ltr

less <floorplan.def>

Press q to exit.

MAGIC:

magic -T <SKY130_TECH_FILE> \
lef read <MERGED_LEF> \
def read <FLOORPLAN_DEF> &

============================================================
EOF

cat > GITHUB_SUBMIT.txt <<'EOF'
# ============================================================
# GITHUB SUBMISSION
# ============================================================

git init

git add .

git status

git commit -m "Added SKY130_D2_SK1 chip floor planning"

git branch -M main

git remote add origin YOUR_GITHUB_REPOSITORY_URL

git push -u origin main

# Replace YOUR_GITHUB_REPOSITORY_URL with your actual
# GitHub repository URL.
EOF

echo ""
echo "============================================================"
echo "SKY130_D2_SK1 READY"
echo "============================================================"
find . -maxdepth 2 -type f | sort

echo ""
echo "IMAGES:"
ls -lh images/

echo ""
echo "============================================================"
echo "NOW SUBMIT TO GITHUB"
echo "============================================================"
echo ""
echo "git init"
echo "git add ."
echo "git commit -m \"Added SKY130_D2_SK1 chip floor planning\""
echo "git branch -M main"
echo "git remote add origin YOUR_GITHUB_REPOSITORY_URL"
echo "git push -u origin main"
echo ""
echo "============================================================"
