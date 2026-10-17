# Assignment 

# Sequence Detector – RTL Design, Synthesis and Gate Level Simulation

## Objective

The objective of this experiment is to design and verify a sequence detector for the bit sequence:

```text
1111001
```

The complete flow includes RTL design, functional simulation, synthesis using Yosys, gate-level netlist generation, and Gate Level Simulation (GLS). The generated waveforms are analyzed using GTKWave.

---

# 1. Design Description

The sequence detector is implemented using a Finite State Machine (FSM). The FSM monitors the serial input `din` on every clock cycle.

The design progresses through different states as matching bits are received. When the complete sequence `1111001` is detected, the output `detected` becomes high.

### Input and Output Signals

| Signal | Type | Description |
|---|---|---|
| `clk` | Input | Clock signal |
| `reset` | Input | Reset signal |
| `din` | Input | Serial input data |
| `detected` | Output | Indicates successful sequence detection |

### Target Sequence

```text
1111001
```

---

# 2. RTL Design Code

The following Verilog code implements the sequence detector.

```verilog
`timescale 1ns/1ps

module sequence_detector (
input  wire clk,
input  wire reset,
input  wire din,
output reg  detected
);

localparam integer STATE_W = 3;
localparam integer NUM_STATES = 7;
// Target sequence: 1111001

reg [STATE_W-1:0] state;
reg [STATE_W-1:0] next_state;
reg next_detected;

always @(*) begin
    next_state = 'd0;
    next_detected = 1'b0;

    case (state)

        0: begin
            if (din == 1'b0) begin
                next_state = 0;
                next_detected = 1'b0;
            end else begin
                next_state = 1;
                next_detected = 1'b0;
            end
        end

        1: begin
            if (din == 1'b0) begin
                next_state = 0;
                next_detected = 1'b0;
            end else begin
                next_state = 2;
                next_detected = 1'b0;
            end
        end

        2: begin
            if (din == 1'b0) begin
                next_state = 0;
                next_detected = 1'b0;
            end else begin
                next_state = 3;
                next_detected = 1'b0;
            end
        end

        3: begin
            if (din == 1'b0) begin
                next_state = 0;
                next_detected = 1'b0;
            end else begin
                next_state = 4;
                next_detected = 1'b0;
            end
        end

        4: begin
            if (din == 1'b0) begin
                next_state = 5;
                next_detected = 1'b0;
            end else begin
                next_state = 4;
                next_detected = 1'b0;
            end
        end

        5: begin
            if (din == 1'b0) begin
                next_state = 6;
                next_detected = 1'b0;
            end else begin
                next_state = 1;
                next_detected = 1'b0;
            end
        end

        6: begin
            if (din == 1'b0) begin
                next_state = 0;
                next_detected = 1'b0;
            end else begin
                next_state = 1;
                next_detected = 1'b1;
            end
        end

        default: begin
            next_state = 'd0;
            next_detected = 1'b0;
        end

    endcase
end

always @(posedge clk) begin
    if (reset) begin
        state <= 'd0;
        detected <= 1'b0;
    end
    else begin
        state <= next_state;
        detected <= next_detected;
    end
end

endmodule
```

---

# 3. Testbench Code

The following testbench generates the clock, applies reset, provides the input bit sequence, counts detections, and generates a VCD waveform file.

```verilog
`timescale 1ns/1ps

module tb;

reg clk = 1'b0;
reg reset = 1'b1;
reg din = 1'b0;
wire detected;

sequence_detector dut (
    .clk(clk),
    .reset(reset),
    .din(din),
    .detected(detected)
);

always #6 clk = ~clk;

// Assessment instance: 24eg104d18

task drive_bit(input reg b);
    begin
        @(negedge clk);
        din = b;
        @(posedge clk);
        #1;
        $display("TIME=%0t NS DIN=%b DETECTED=%b", $time, din, detected);
    end
endtask

integer detection_count = 0;

always @(negedge clk) begin
    if (!reset && detected)
        detection_count = detection_count + 1;
end

initial begin
    $dumpfile("dump.vcd");
    $dumpvars(0, tb);

    // Initial reset.
    reset = 1'b1;
    repeat (4) @(posedge clk);
    @(negedge clk);
    reset = 1'b0;

    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);
    drive_bit(1'b1);
    drive_bit(1'b0);
    drive_bit(1'b0);

    // Final reset.
    @(negedge clk);
    reset = 1'b1;
    repeat (2) @(posedge clk);

    #1;
    $display("FINAL_DETECTION_COUNT=%0d", detection_count);
    $finish;
end

endmodule
```

---

# 4. RTL Simulation

Before synthesis, the RTL design is simulated using the testbench.

## Compile the RTL Design

```bash
iverilog -o tb.out sequence_detector.v tb.v
```

## Run Simulation

```bash
vvp tb.out
```

The simulation generates the following VCD file:

```text
dump.vcd
```

## Open the Waveform

```bash
gtkwave dump.vcd
```

---

## RTL Simulation Waveform

The RTL waveform is used to verify the behavior of the sequence detector before synthesis.

Signals observed:

- `clk`
- `reset`
- `din`
- `detected`

<img width="1920" height="1200" alt="gtkwave of assesment" src="https://github.com/user-attachments/assets/b8c22ed5-a108-4c81-af34-7e9fa8f14e8c" />


---

# 5. RTL Synthesis Using Yosys

The Verilog RTL design is synthesized using Yosys.

## Start Yosys

```bash
yosys
```

## Read the Verilog Design

```tcl
read_verilog sequence_detector.v
```

## Set the Top Module

```tcl
hierarchy -top sequence_detector
```

## Perform Synthesis

```tcl
synth -top sequence_detector
```

## Display Statistics

```tcl
stat
```

## Generate the Gate-Level Netlist

```tcl
write_verilog sequence_detector_netlist.v
```

## Exit Yosys

```tcl
exit
```

---

# 6. Synthesis Result

The RTL design was successfully synthesized using Yosys.

The synthesis process converts the behavioral RTL description into a gate-level implementation consisting of combinational logic and sequential elements.

## Yosys Synthesis Output

<img width="1920" height="1200" alt="assesment of graphical shematic" src="https://github.com/user-attachments/assets/b25e6687-66fe-41ef-b736-61a7c70a59ed" />


# 7. Gate-Level Schematic

The synthesized schematic can be generated using the Yosys `show` command.

```bash
yosys
```

Inside Yosys:

```tcl
read_verilog sequence_detector.v
hierarchy -top sequence_detector
synth -top sequence_detector
show
```

The schematic displays the logic generated after synthesis.

## Synthesized Schematic Output

<img width="1920" height="1200" alt="assesment of output stats" src="https://github.com/user-attachments/assets/db05f9da-3cb6-49d6-adb3-598e911449dc" />


# 8. Gate Level Simulation (GLS)

After synthesis, Gate Level Simulation is performed using the synthesized netlist.

For correct functional GLS using the SKY130 simulation models, compile using the `POST_SYNTH_SIM` and `FUNCTIONAL` definitions.

## GLS Compilation Command

```bash
sudo iverilog -DPOST_SYNTH_SIM -DFUNCTIONAL -I src/include/ -I ../../sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/ -I src/module/ src/module/testbench.v
```

## Run Gate-Level Simulation

```bash
./a.out
```

## Open the Generated VCD Waveform

```bash
gtkwave dump.vcd
```

---

# 9. Gate Level Simulation Waveform

The GTKWave output is used to verify the behavior of the synthesized sequence detector.
<img width="1920" height="1200" alt="gtkwave of gls" src="https://github.com/user-attachments/assets/a1dcbdd0-6f97-4772-a9f5-481189a8cb2e" />

The following signals are observed:

| Signal | Description |
|---|---|
| `clk` | System clock |
| `reset` | Resets the FSM |
| `din` | Serial input data |
| `detected` | Indicates successful detection |

When the input sequence matches the target sequence `1111001`, the `detected` signal is asserted.

## GLS Waveform Output


<img width="1920" height="1200" alt="gtkwave of gls" src="https://github.com/user-attachments/assets/9ec20a5a-9b6c-406d-ac79-ce579e5ea3f3" />



# 10. Waveform Verification

The waveform verifies the operation of the sequence detector.

The verification process includes:

1. Applying reset to initialize the FSM.
2. Providing the input data stream through `din`.
3. Monitoring state progression indirectly through the output behavior.
4. Checking the `detected` signal when the target sequence is received.
5. Comparing the synthesized design behavior through Gate Level Simulation.

The waveform confirms that the synthesized netlist responds to the applied input sequence.

---

# Results

The following tasks were successfully completed:

- RTL design of the sequence detector
- Testbench development
- RTL functional simulation
- VCD waveform generation
- Waveform analysis using GTKWave
- RTL synthesis using Yosys
- Gate-level netlist generation
- Gate-level schematic generation
- Gate Level Simulation (GLS)
- Verification of synthesized design behavior

---

# Conclusion

A sequence detector for the target sequence `1111001` was successfully implemented using Verilog HDL.

The design was first verified at the RTL level using the developed testbench. The RTL was then synthesized using Yosys to generate a gate-level representation.

Finally, Gate Level Simulation was performed using the synthesized design and the functional simulation models. The generated waveform was analyzed using GTKWave to verify the behavior of the `clk`, `reset`, `din`, and `detected` signals.

Thus, the complete RTL design, synthesis, and Gate Level Simulation flow was successfully completed.
