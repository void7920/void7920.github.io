---
title:  "Demultiplexer 1x4"
excerpt: "Combinational Logic Demultiplexer 1x4"

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

| &nbsp; &nbsp; S<sub>1<sub> &nbsp; &nbsp; | &nbsp; &nbsp; S<sub>0<sub> &nbsp; &nbsp; | &nbsp; &nbsp; A &nbsp; &nbsp; | &nbsp; &nbsp; B &nbsp; &nbsp; | &nbsp; &nbsp; C &nbsp; &nbsp; | &nbsp; &nbsp; D &nbsp; &nbsp; |
|:---:|:---:|:---:|:---:|:---:|:---:|
|  0  |  0  |  Y  |  0  |  0  |  0  |
|  0  |  0  |  0  |  Y  |  0  |  0  |
|  1  |  0  |  0  |  0  |  Y  |  0  |
|  1  |  1  |  0  |  0  |  0  |  Y  |

## Boolean Equation

---

# Logic

// ![DEMUX](/images/2022-02-21-DEMUX/logic.png)

---

# Design Source

## Structual modeling

```verilog
module module Demultiplexer_1x4_Structural(
    o0,     // output 0 
    o1,     // output 1
    o2,     // output 2
    o3,     // output 3
    i,      // input 
    s       // mode sel
    );
    
    output o0;
    output o1;
    output o2;
    output o3;
    input i; 
    input [1:0]s;
    
    and(o0, i, ~s[1], ~s[0]);
    and(o1, i, ~S[1], s[0]);
    and(o2, i, s[1], ~s[0]);
    and(o3, i, s[1], s[0]);
endmodule
```

## Dataflow modeling

```verilog
module Demultiplexer_1x4_Dataflow(
    o0,     // output 0 
    o1,     // output 1
    o2,     // output 2
    o3,     // output 3
    i,      // input 
    s       // mode sel
    );
    
    output o0;
    output o1;
    output o2;
    output o3;
    input i; 
    input [1:0]s;
    
    assign o0 = i & ~s[1] & ~s[0];
    assign o1 = i & ~s[1] & s[0];
    assign o2 = i & s[1] & ~s[0];
    assign o3 = i & s[1] & s[0];
endmodule
```

## Behavioral modeling

```verilog
module Demultiplexer_1x4_Behavioral(
    o0,     // output 0 
    o1,     // output 1
    o2,     // output 2
    o3,     // output 3
    i,      // input 
    s       // mode sel
    );
    
    output o0;
    output o1;
    output o2;
    output o3;
    input i; 
    input [1:0]s;
    
    always@(*)
    begin
        casex(s)
            'b00:
                begin
                    o0 = 1;
                    o1 = 0;
                    o2 = 0;
                    o3 = 0;
                end
            'b01:
                begin
                    o0 = 0;
                    o1 = 1;
                    o2 = 0;
                    o3 = 0;
                end
            'b10:
                begin
                    o0 = 0;
                    o1 = 0;
                    o2 = 1;
                    o3 = 0;
                end
            'b11:
                begin
                    o0 = 0;
                    o1 = 0;
                    o2 = 0;
                    o3 = 1;
                end
        endcase
    end
endmodule
```
---

# Simulation Source

```verilog
module Tb_DEMUX();
    reg Y;
    reg [1:0]S;
    wire [3:0]A;
    
    Demultiplexer_1x4_Behavioral sim_DEMUX0(A[0], A[1], A[2], A[3], Y, S);
//    Demultiplexer_1x4_Dataflow sim_DEMUX1(A[0], A[1], A[2], A[3], Y, S);
//    Demultiplexer_1x4_Structural sim_DEMUX2(A[0], A[1], A[2], A[3], Y, S);
    
    initial
    begin
        Y=0;
        S=0;
    end
    
    initial
    begin
        #100 Y=0; S=2'b01;
        #100 Y=0; S=2'b10;
        #100 Y=0; S=2'b11;
        #100 Y=1; S=2'b00;
        #100 Y=1; S=2'b01;
        #100 Y=1; S=2'b10;
        #100 Y=1; S=2'b11;
    end
endmodule
```
---

# Simulation data

// ![Tb_DEMUX](/images/2022-02-21-DEMUX/tb.png)

# Simulation data
