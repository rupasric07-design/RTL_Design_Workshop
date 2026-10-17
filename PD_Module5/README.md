# Module 5 - Optimization in Synthesis

## Overview

This document focuses on writing synthesizable Verilog in a way that produces the intended hardware. The session covers conditional statements, latch inference, loops, generate constructs, and the implementation of commonly used digital circuits.

The practical exercises demonstrate how different RTL coding styles affect the synthesized hardware and how proper coding techniques can be used to obtain predictable combinational logic.

## 1. Conditional Statements in Verilog

Conditional statements are frequently used in RTL design to select one operation or data path depending on the value of a control signal.

The `if-else` construct is normally written inside procedural blocks such as `always`, `initial`, functions, or tasks. In combinational RTL, the conditions should be written carefully so that every possible input condition produces a defined output.

### Basic Syntax

```verilog
if (condition) begin
    // Statements executed when the condition is true
end else begin
    // Statements executed when the condition is false
end
````

The condition determines which section of the RTL is executed. The `begin` and `end` keywords are useful when multiple statements belong to the same condition.

The `else` section is optional. However, when describing combinational hardware, leaving an output unassigned for some conditions can result in unintended latch inference.

### Nested If-Else

Multiple conditions can be checked sequentially using an `else if` structure.

```verilog
if (condition1) begin
    // Statements for condition1
end else if (condition2) begin
    // Statements for condition2
end else begin
    // Statements when none of the conditions are satisfied
end
```

This style is useful when several control conditions determine the output of a combinational circuit.

---

## 2. Latch Inference

A latch may be inferred during synthesis when a combinational process does not specify an output value for every possible condition.

In combinational RTL, an output should normally receive a value regardless of which input combination occurs. If the output is left unchanged for a particular condition, the synthesis tool interprets this requirement as a need to remember the previous value. This results in latch hardware.

### Example of Latch Inference

```verilog
module ex (
    input wire a, b, sel,
    output reg y
);
    always @(a, b, sel) begin
        if (sel == 1'b1)
            y = a; // No 'else' - y is not assigned when sel == 0
    end
endmodule
```

When `sel` is `1`, the output receives the value of `a`.

When `sel` becomes `0`, there is no assignment for `y`. Therefore, the previous value of `y` has to be retained, which can cause the synthesis tool to infer a latch.

### Avoiding Unwanted Latches

A default assignment or a complete `if-else` / `case` structure can be used to make the combinational logic complete.

```verilog
module ex (
    input wire a, b, sel,
    output reg y
);
    always @(a, b, sel) begin
        case(sel)
            1'b1 : y = a;
            default : y = 1'b0; // Default assignment
        endcase
    end
endmodule
```

The `default` branch ensures that `y` receives a value even when the specified case condition is not selected.

---

## 3. Labs on If-Else and Case Statements

The following experiments demonstrate how incomplete conditions and different case structures influence RTL synthesis.

### Incomplete If Statement

This experiment demonstrates an `if` statement in which the output is assigned only when `i0` is active. The missing assignment for the other condition can result in latch inference during synthesis.

```verilog
module incomp_if (input i0, input i1, input i2, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
end
endmodule
```

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/fb6805e1-bff6-49c4-afeb-63a667b62113" />


---

### Synthesis Result 

The synthesis result of the previous RTL can be examined to identify the hardware generated because of the incomplete conditional assignment.

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/8b237a9d-71aa-4f3f-8952-73282e8e91d3" />


---

###  Nested If-Else

This experiment uses more than one condition to determine the value of the output. Since the final condition is not covered by an `else`, there can still be an execution path in which `y` remains unassigned.

```verilog
module incomp_if2 (input i0, input i1, input i2, input i3, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
    else if (i2)
        y <= i3;
end
endmodule
```

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/16ff98e7-bf73-4a5c-9b72-05231ea972c3" />


---

### Synthesis Result 

The synthesized circuit corresponding to the nested conditional RTL is shown below.


<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/038a5b60-9121-433b-b3a6-d092b60f1764" />


---

### Complete Case Statement

A `case` statement can be used when the output depends on the value of a select signal. In this example, the first two select combinations are handled explicitly and the remaining combinations are covered using `default`.

```verilog
module comp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
always @(*) begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
        default : y = i2;
    endcase
end
endmodule
```


<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/fd3163c8-66f4-4dcf-97f1-e85d4559feff" />


---

### Synthesis Result 

The synthesis output corresponding to the complete case structure is shown below.


<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/94faf56c-86bc-4e94-a04d-2b3837d7e05e" />


### Simulation Waveform

The GTKWave simulation waveform shows the behavior of the incomplete case design for different input and select combinations.

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/61dd2b2e-8b24-4311-837c-0850f33d454d" />

### Yosys Synthesis Result

The synthesized circuit generated using Yosys shows the gate-level implementation of the incomplete case design.

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/70f2dfce-54b3-4c47-8e06-b125d38427e3" />


---

### Incomplete Case Handling

This experiment illustrates the importance of carefully defining case conditions. Wildcard patterns can match multiple input combinations, so their placement and coverage must be considered carefully.

```verilog
module bad_case (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
always @(*) begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        2'b1?: y = i3; // '?' is a wildcard; be careful with incomplete cases!
    endcase
end
endmodule
```


<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/e8912969-931e-4bff-90ea-9f344e75c1da" />


---

### Partial Assignments in Case

Here, two outputs are controlled inside the same `case` statement. Since every case branch does not assign both outputs, some signals may remain unassigned for certain conditions. This can result in latch inference during synthesis.
### Verilog code:
 
```verilog
 module partial_case_assign (
    input i0, input i1, input i2,
    input [1:0] sel,
    output reg y, output reg x
);
always @(*) begin
    case(sel)
        2'b00: begin
            y = i0;
            x = i2;
        end
        2'b01: y = i1;
        default: begin
            x = i1;
            y = i2;
        end
    endcase
end
endmodule
```

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/01afbd4e-f472-4f36-8478-f9f1c8e57616" />


---

## 4 For Loops in Verilog

A `for` loop allows a group of statements to be repeated using a loop variable. In synthesizable RTL, loops are generally used to describe repetitive hardware structures rather than software-style runtime iteration.

For synthesis, the loop limits should normally be determinable during compilation so that the synthesis tool can expand the loop into the corresponding hardware.

### Syntax

```verilog
for (initialization; condition; increment) begin
    // Statements to execute
end
```

A `for` loop can be used inside procedural blocks such as `always` or `initial`.

### Example: 4-to-1 MUX Using a For Loop

The following example uses a loop to compare the select value with each possible input position.

```verilog
module mux_4to1_for_loop (
    input wire [3:0] data, // 4 input lines
    input wire [1:0] sel,  // 2-bit select
    output reg y           // Output
);
    integer i;
    always @(data, sel) begin
        y = 1'b0; // Default output
        for (i = 0; i < 4; i = i + 1) begin
            if (i == sel)
                y = data[i];
        end
    end
endmodule
```

The loop allows the same selection operation to be described without writing four separate conditional statements.

---

## 5. Generate Blocks in Verilog

Generate constructs are useful when several similar pieces of hardware have to be instantiated. Unlike a procedural `for` loop, a generate loop operates at elaboration time and can create multiple hardware instances.

The `genvar` keyword is commonly used as the loop variable for generate-for constructs.

### Example

```verilog
genvar i;
generate
    for (i = 0; i < 4; i = i + 1) begin : gen_loop
        and_gate and_inst (.a(in[i]), .b(in[i+1]), .y(out[i]));
    end
endgenerate
```

In this example, the generate loop creates multiple instances of `and_gate`. This approach is useful for repetitive and scalable hardware designs.

---

## 6. Ripple Carry Adder

A Ripple Carry Adder (RCA) performs binary addition by connecting several full adders in sequence.

For an `n`-bit RCA, `n` full-adder stages are required. The carry produced by one stage becomes the carry input of the next stage.

The carry therefore propagates from the least significant bit toward the most significant bit. This propagation gives the circuit its name: **Ripple Carry Adder**.

The basic relationship can be represented as:

* Each full adder receives two operand bits.
* A carry input is supplied from the previous stage.
* Each stage generates a sum bit and a carry output.
* The final carry becomes the most significant output bit.

---

## 7 Loops and Generate Blocks

The next set of experiments applies loops and generate constructs to practical digital circuits.

###  4-to-1 MUX Using For Loop

This implementation represents the multiplexer inputs as a vector and uses a loop to identify the selected input.

```verilog
module mux_generate (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
wire [3:0] i_int;
assign i_int = {i3, i2, i1, i0};
integer k;
always @(*) begin
    for (k = 0; k < 4; k = k + 1) begin
        if (k == sel)
            y = i_int[k];
    end
end
endmodule
```


<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/fb95ac57-a2fb-450d-bc7e-d05ba7f6ed5a" />


---

### 8-to-1 Demux Using Case

An 8-to-1 demultiplexer directs the input signal to one of eight output lines according to the three-bit select signal.

The following implementation first clears all output positions and then activates the selected position.

```verilog
module demux_case (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
always @(*) begin
    y_int = 8'b0;
    case(sel)
        3'b000 : y_int[0] = i;
        3'b001 : y_int[1] = i;
        3'b010 : y_int[2] = i;
        3'b011 : y_int[3] = i;
        3'b100 : y_int[4] = i;
        3'b101 : y_int[5] = i;
        3'b110 : y_int[6] = i;
        3'b111 : y_int[7] = i;
    endcase
end
endmodule
```


<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/e6001055-aac0-44f6-b0c4-c652df9765bc" />


---

### 8-to-1 Demux Using For Loop

The same demultiplexer functionality can also be described using a `for` loop. Instead of listing every select condition separately, the loop checks each output position against the select value.

```verilog
module demux_generate (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
integer k;
always @(*) begin
    y_int = 8'b0;
    for (k = 0; k < 8; k = k + 1) begin
        if (k == sel)
            y_int[k] = i;
    end
end
endmodule
```


<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/28c594aa-3bcd-4ef5-b54e-a8440e6c16ff" />


---

### 8-bit Ripple Carry Adder with Generate Block

This experiment demonstrates how a generate-for construct can be used to create the repeated full-adder stages of an 8-bit Ripple Carry Adder.

The first full adder handles the least significant bit. The remaining stages are generated using the `generate` construct, with the carry output of one stage connected to the carry input of the next stage.

```verilog
module rca (
    input [7:0] num1,
    input [7:0] num2,
    output [8:0] sum
);
wire [7:0] int_sum;
wire [7:0] int_co;

genvar i;
generate
    for (i = 1; i < 8; i = i + 1) begin
        fa u_fa_1 (.a(num1[i]), .b(num2[i]), .c(int_co[i-1]), .co(int_co[i]), .sum(int_sum[i]));
    end
endgenerate

fa u_fa_0 (.a(num1[0]), .b(num2[0]), .c(1'b0), .co(int_co[0]), .sum(int_sum[0]));

assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```

**Full Adder Module:**

```verilog
module fa (input a, input b, input c, output co, output sum);
    assign {co, sum} = a + b + c;
endmodule
```


<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/4d066e21-f6ac-4d77-b4f5-13205887373e" />


---

## Summary
 
 Module 5 introduced important RTL coding techniques used for synthesizable digital designs.

* Conditional statements can be used to describe selection and decision-making logic.
* Incomplete assignments in combinational blocks may result in unwanted latch inference.
* Complete `if-else` and `case` structures help ensure predictable combinational behavior.
* `for` loops provide a compact way to describe repetitive hardware structures.
* Generate blocks are useful for creating multiple instances of similar hardware during elaboration.
* Multiplexers and demultiplexers can be described using both conditional statements and loops.
* A Ripple Carry Adder can be constructed by connecting multiple full-adder stages.
* Proper RTL coding improves the correspondence between the intended design and the synthesized hardware.

Overall, the experiments provided practical understanding of how Verilog coding style influences the resulting hardware during synthesis.
