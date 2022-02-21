---
title:  "Multiplexer 4x1"
excerpt: "Combinational Logic Multiplexer 4x1"

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

| &nbsp; &nbsp; S<sub>1<sub> &nbsp; &nbsp; | &nbsp; &nbsp; S<sub>0<sub> &nbsp; &nbsp; | &nbsp; &nbsp; Y &nbsp; &nbsp; |
|:---:|:---:|:---:|
|  0  |  0  |  A  |
|  0  |  0  |  B  |
|  1  |  0  |  C  |
|  1  |  1  |  D  |

## Boolean Equation

---

# Logic

// ![MUX](/images/2022-02-21-MUX/logic.png)

---

# Design Source

## Structual modeling

```verilog
module Multiplexer_4x1_Structural(
    o,      // output 
    i0,     // input 0
    i1,     // input 1
    i2,     // input 2
    i3,     // input 3
    s       // mode sel
    );
    
    output o;
    input i0;
    input i1;
    input i2;
    input i3;
    input [1:0]s;
    
    wire w0, w1, w2, w3;
    
    and(w0, i0, ~s[0], ~s[1]);
    and(w1, i1, s[0], ~s[1]);
    and(w2, i2, ~s[0], s[1]);
    and(w3, i3, s[0], s[1]);
    
    or(o, w0, w1, w2, w3);
endmodule
```

## Dataflow modeling

```verilog
module Multiplexer_4x1_Dataflow(
    o,      // output 
    i0,     // input 0
    i1,     // input 1
    i2,     // input 2
    i3,     // input 3
    s       // mode sel
    );
    
    output o;
    input i0;
    input i1;
    input i2;
    input i3;
    input [1:0]s;
    
    assign o = (i0 & ~s[0] & ~s[1]) | (i1 & s[0] & ~s[1]) | (i2 & ~s[0] & s[1]) | (i3 & s[0] & s[1]);
endmodule
```

## Behavioral modeling

```verilog
module Multiplexer_4x1_Behavioral(
    o,      // output 
    i0,     // input 0
    i1,     // input 1
    i2,     // input 2
    i3,     // input 3
    s       // mode sel
    );
    
    output reg o;
    input i0;
    input i1;
    input i2;
    input i3;
    input [1:0]s;
    
    always@(*)
    begin
        casex(S)
            'b00: o=I0;
            'b01: o=I1;
            'b10: o=I2;
            'b11: o=I3;
        endcase     
    end
endmodule
```
---

# Simulation Source

```verilog
module Tb_MUX();
    reg [3:0]I;
    reg [1:0]S;
    wire Y;
    
    Multiplexer_4x1_Behavioral sim_MUX0(Y, I[0], I[1], I[2], I[3], S);
//    Multiplexer_4x1_Dataflow sim_MUX1(Y, I[0], I[1], I[2], I[3], S);
//    Multiplexer_4x1_Structural sim_MUX2(Y, I[0], I[1], I[2], I[3], S);
    
    initial
    begin
        I = 4'b0000;
        S = 2'b00;
    end
    
    initial
    begin
        #50 I=4'b0000; S=2'b01;
        #50 I=4'b0000; S=2'b10;
        #50 I=4'b0000; S=2'b11;
        #50 I=4'b0001; S=2'b00;
        #50 I=4'b0001; S=2'b01;
        #50 I=4'b0001; S=2'b10;
        #50 I=4'b0001; S=2'b11;
        #50 I=4'b0010; S=2'b00;
        #50 I=4'b0010; S=2'b01;
        #50 I=4'b0010; S=2'b10;
        #50 I=4'b0010; S=2'b11;
        #50 I=4'b0100; S=2'b00;
        #50 I=4'b0101; S=2'b01;
        #50 I=4'b0110; S=2'b10;
        #50 I=4'b0111; S=2'b11;
        #50 I=4'b1000; S=2'b00;
        #50 I=4'b1001; S=2'b01;
        #50 I=4'b1010; S=2'b10;
        #50 I=4'b1011; S=2'b11;
    end
endmodule
```
---

# Simulation data

// ![Tb_MUX](/images/2022-02-21-MUX/tb.png)

# Simulation data
