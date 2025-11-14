# **Simulation of Sequence Detector Using Mealy Machine in Verilog HDL**

---

## **Aim**
To design and simulate a **Sequence Detector using Mealy Machine** in Verilog HDL that detects a specific bit pattern from a given binary input sequence.

---

## **Apparatus Required**
- Computer with **Vivado Design Suite** installed  
---

## **Theory**

A **Sequence Detector** identifies a specific sequence of bits from a stream of binary input data.  
**Mealy Machine** – Output depends on both the current state and the input.

In the **Mealy Machine**, the output can change immediately in response to an input, making it faster than the Moore type.  
The Mealy model uses **state transitions** and **output logic** based on both the current state and the input signal.

For example, a **sequence detector** that detects `"1011"`:
- It produces an output `1` whenever the sequence `1011` appears in the input stream.
- Overlapping sequences are also detected.

---
## State Diagram 
<img width="363" height="550" alt="image" src="https://github.com/user-attachments/assets/7861102e-c301-422a-bfa8-d8d887a8652b" />

## Verilog Code
```
`timescale 1ns / 1ps
module mealy(clk,rst,in,out);
input clk;
input rst;
input in;
output reg out;
parameter S0 = 3'b000,
          S1 = 3'b001,
          S2 = 3'b010,
          S3 = 3'b011,
          S4 = 3'b100;
reg [2:0] current_state,next_state;
always @(posedge clk or posedge rst) 
begin
if (rst)
    current_state <= S0;
else
    current_state <= next_state;
end

always @(*) 
begin
out = 0;          
case (current_state)
S0: 
begin
if (in) 
    next_state = S1;
else    
    next_state = S0;
end
S1: begin
if (in) 
    next_state = S2; 
else    
    next_state = S0;
end

S2: begin
if (in) 
    next_state = S2; 
else    
    next_state = S3;
end

S3: 
begin
if (in) 
    next_state = S4; 
else    
    next_state = S0;
end
S4: 
begin
if (in) 
begin
    out = 1;           
    next_state = S0;   
end
else
    next_state = S3;    
end

default:next_state = S0;
endcase
end
endmodule

```
## Testbench
```

module mealy_tb;
reg clk, rst, in;
wire out;
mealy uut (clk,rst,in,out);
initial  clk = 0;
always #5 clk = ~clk;   
initial 
begin
rst = 1; in = 0;
#12 rst = 0;
#10 in=0;
#10 in=1;
#10 in=1;
#10 in=1;
#10 in=0;
#10 in=1;
#10 in=1;
#10 in=0; 
#50 $finish;
end
endmodule
```
## Simulation Output 
---

<img width="1919" height="1079" alt="Screenshot 2025-10-22 111100" src="https://github.com/user-attachments/assets/d71200bc-7ca5-442a-8677-1bc627f94a6b" />


---
## Result

The Mealy Machine Sequence Detector for the bit pattern "11011" was successfully designed and simulated using Verilog HDL.
Simulation results confirm that the circuit correctly detects the pattern and supports overlapping sequences.

