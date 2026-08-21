# Day 03 • Sequential Optimizations for Unused Outputs

> **"If the output never sees it, synthesis has no reason to keep it."**

In **Day 03**, I explored **sequential logic optimization for unused outputs** using **Yosys synthesis**.

The experiment focuses on a **3-bit counter** and demonstrates how synthesis identifies sequential logic that does not contribute to the required output and optimizes it.

---

## 📚 What I Explored

- Unused Outputs & Unused Sequential Logic
- Optimization of Counter Logic
- Output Observability
- Effect of Output Logic on Optimization
- Summary

---

## 🔹 Sequential Optimizations for Unused Outputs

Sequential optimization for unused outputs is the process of **identifying and removing sequential logic that does not contribute to any required output of the design**.

In RTL, we may have registers, flip-flops, counters, or state variables that are updated on every clock cycle but whose values are **never used by the output logic**.

Although these elements are present in the Verilog code, they may not be necessary in the actual hardware.

During synthesis, **Yosys analyzes the connectivity and observability of sequential logic**.

If a register or its associated logic has no effect on any observable output, Yosys can optimize it away from the synthesized netlist.

---

## 🔹 Counter Output Uses Only One Bit

### RTL Code

![RTL Code - Counter Optimization](screenshots/gvim_counter_opt.png)

The design contains a **3-bit counter**:

```text
count[2:0]
```

The counter starts from:

```text
000
```

and increments on every rising edge of `clk`.

The asynchronous reset forces the counter back to zero:

```verilog
if (reset)
    count <= 3'b000;
```

However, the output is connected only to:

```verilog
assign q = count[0];
```

This means that although the RTL declares three counter bits, only **`count[0]`** can affect the external output.

### Counter Behavior

```text
count = 000 → q = 0
count = 001 → q = 1
count = 010 → q = 0
count = 011 → q = 1
count = 100 → q = 0
count = 101 → q = 1
```

---

## 🔬 Yosys Optimization

![Yosys Counter Optimization](screenshots/counter_opt_show.png)

Only **`count[0]`** is connected to the external output.

The upper bits **`count[2]`** and **`count[1]`** do not directly contribute to the observable output.

The count diamond is a **2:1 mux construct** showing bit-select and route logic. The labels such as `2:1 - 1:0` and `1:0 - 2:1` are Yosys bit-index annotations for the mux inputs and outputs.

Output `q` comes directly from one of the mux branches.

Therefore, Yosys can analyze their observability and eliminate sequential logic that is not required to preserve the required output behavior.

---

## 🔹 Output Uses the Complete Counter

The RTL was then modified so that the output depends on the **entire 3-bit counter**.

![Complete Counter RTL](screenshots/gvim_counter_opt2_using3bit.png)

The output now checks whether the complete counter value is equal to:

```text
3'b100
```

Therefore:

```text
count = 000 → q = 0
count = 001 → q = 0
count = 010 → q = 0
count = 011 → q = 0
count = 100 → q = 1
count = 101 → q = 0
...
```

The output is asserted only when:

```text
count = 100
```

---

## 🔹 Optimization Is Different Here

In the first design:

```text
q = count[0];
```

Only one bit was required to determine the output.

But in the modified design:

```text
q = (count[2:0] == 3'b100);
```

Every counter bit participates in determining the output.

Therefore, removing **`count[2]`** or **`count[1]`** would change the functionality of the design.

---

## 🔄 Comparison

| Design | Output Logic | Required Counter Bits | Optimization |
|--------|--------------|-----------------------|--------------|
| First Design | `q = count[0]` | `count[0]` | Unused logic can be optimized |
| Modified Design | `q = (count[2:0] == 3'b100)` | `count[2:0]` | All counter bits are required |

---

## 📌 Key Learnings

- A **3-bit counter** is implemented in the RTL.
- Only **`count[0]`** is connected to the output `q` in the first design.
- The upper bits **`count[2]`** and **`count[1]`** do not affect the observable output.
- Yosys analyzes **output observability** during synthesis.
- Unnecessary sequential logic can be **eliminated or simplified**.
- When all counter bits affect the output, they cannot be removed.
- Synthesis can produce **optimized hardware from seemingly larger RTL**.
- Output connectivity plays an important role in sequential optimization.

---

## 📁 Files

```text
Day3/
│
├── counter_opt.v
├── counter_opt2_using3bit.v
│
├── screenshots/
│   ├── gvim_counter_opt.png
│   ├── counter_opt_show.png
│   └── gvim_counter_opt2_using3bit.png
│
└── README.md
```

---

## 🛠️ Tools Used

- **Yosys**
- **VirtualBox**
- **Verilog HDL**
- **GVim**
- **Dot Viewer**
- **SKY130 Standard Cell Library**
- **GitHub**

---

## 📸 Screenshots

### RTL Counter Design

![gvim_counter_opt](screenshots/gvim_counter_opt.png)

### Yosys Synthesized Counter

![counter_opt_show](screenshots/counter_opt_show.png)

### RTL Using Complete 3-bit Counter

![gvim_counter_opt2_using3bit](screenshots/gvim_counter_opt2_using3bit.png)

---

## 📝 Summary

- A **3-bit counter** is implemented in the RTL.
- Only **`count[0]`** is connected to the output `q` in the first experiment.
- The upper bits **`count[2]`** and **`count[1]`** do not affect the observable output.
- Yosys analyzes this **output observability** during synthesis.
- Unnecessary sequential logic can be **eliminated or simplified**.
- When the output depends on the complete counter, all three counter bits are required.
- This demonstrates how synthesis can produce **optimized hardware from seemingly larger RTL**.

---

## ✅ Completion

| Topic | Status |
|------|--------|
| Unused Outputs | ✅ Completed |
| Unused Sequential Logic | ✅ Completed |
| Counter Logic Optimization | ✅ Completed |
| Output Observability | ✅ Completed |
| Complete Counter Output | ✅ Completed |
| Yosys Synthesis | ✅ Completed |
| Netlist Analysis | ✅ Completed |
| Sequential Optimization | ✅ Completed |

---

### ⭐ Day 03 Complete

> **"The RTL describes the possibility — synthesis keeps only what the hardware actually needs."**

**Day 03 Completed Successfully! ✅**
