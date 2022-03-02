---
title:  "Half Subtractor"
excerpt: "Combinational Logic Half Subtactor"

categories:
  - Combinational
tags:
  - [FPGA, Verilog, Logic, Combinational]

toc: true
toc_sticky: true

date: 2022-02-21
last_modified_at: 2022-02-21
---

# Truth Table

| &nbsp; &nbsp; A &nbsp; &nbsp; | &nbsp; &nbsp; B &nbsp; &nbsp; | &nbsp; &nbsp; D &nbsp; &nbsp; | &nbsp; &nbsp; Br &nbsp; &nbsp; |
|:---:|:---:|:---:|:---:|:---:|
|  0  |  0  |  0  |  0  |
|  1  |  0  |  1  |  1  |
|  0  |  1  |  1  |  0  |
|  1  |  1  |  0  |  0  |

## Boolean Equation

D = A ⊕ B

Br = A' × B

---

# Logic

![HS](/images/2022-02-21-HS/logic.png)

---

# Design Source

## Structual modeling

```verilog
module Half_Subtractor_Structural(
    br_out,     // borrow out
    d,          // difference
    a,          // a
    b,          // b
    );
   
    output br_out;
    output d;
    input a;
    input b;
    
    xor(d, a, b);
    and(br, ~a, b);
endmodule
```

## Dataflow modeling

```verilog
module Half_Subtractor_Dataflow(
    br, 
    d,
    a, 
    b
    );
    
    output br;
    output d;
    input a;
    input b;
    
    assign d = a ^ b;
    assign br = ~a & b;
endmodule
```

## Behavioral modeling

```verilog
module Half_Subtractor_Behavioral(
    br,     // borrow
    d,      // difference
    a,      // a
    b       // b
    );
    
    output reg br;
    output reg d; 
    input a;
    input b;
    
    always@(*)
    begin
        case({a, b})
            2'b00 : 
                begin
                    br = 1'b0;
                    d = 1'b0;        
                end
            
            2'b01 : 
                begin
                    br = 1'b1;
                    d = 1'b1;        
                end
            
            2'b10 : 
                begin
                    br = 1'b0;
                    d = 1'b1;       
                end
            
            2'b11 : 
                begin
                    br = 1'b0;
                    d = 1'b0;        
                end
        endcase
    end
endmodule
```
---

# Simulation Source

```verilog
module Tb_HS();
    reg A, B;
    wire Br, D;
    
    Half_Subtractor_Behavioral sim_HS0(Br, D, A, B);
//    Half_Subtractor_Dataflow sim_HS1(Br, D, A, B);
//    Half_Subtractor_Structural sim_HS2(Br, D, A, B);
    
    initial
    begin
        A=0;
        B=0;
    end
    
    initial
    begin
        #100 A=0; B=1;
        #100 A=1; B=0;
        #100 A=1; B=1;
    end
endmodule
```
---

# Simulation data

![Tb_HS](/images/2022-02-21-HS/tb.png)
