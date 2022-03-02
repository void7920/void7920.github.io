---
title:  "Gary to Binary Converter"
excerpt: "Combinational Logic Gray to Binary Converter"

categories:
  - Combinational
tags:
  - [FPGA, Verilog, Logic, Combinational]

toc: true
toc_sticky: true

date: 2022-03-01
last_modified_at: 2022-03-01
---

# Conversion Table

| &nbsp; &nbsp; Decimal &nbsp; &nbsp; | &nbsp; &nbsp; Gray &nbsp; &nbsp; | &nbsp; &nbsp; Binary &nbsp; &nbsp; |
|:---:|:---:|:---:|
|  00  |  0000  |  0000  |
|  01  |  0001  |  0001  |
|  02  |  0011  |  0010  |
|  03  |  0010  |  0011  |
|  04  |  0110  |  0100  |
|  05  |  0111  |  0101  |
|  06  |  0101  |  0110  |
|  07  |  0100  |  0111  |
|  08  |  1100  |  1000  |
|  09  |  1101  |  1001  |
|  10  |  1111  |  1010  |
|  11  |  1110  |  1011  |
|  12  |  1010  |  1100  |
|  13  |  1011  |  1101  |
|  14  |  1001  |  1110  |
|  15  |  1000  |  1111  |

---

# Logic

![G2B](/images/2022-03-01-G2B/logic.png)

---

# Design Source

## Structual modeling

```verilog
module Gray_to_Bin_Converter_Structural #(parameter size = 4)(
    bin,
    gray
    );
    
    output [size-1:0]bin;
    input [size-1:0]gray;
    
    genvar i;
    
    for(i=0; i<size; i=i+1)
    begin
        if(i<size-1)
            xor(b[i], g[i], b[i+1]);
        
        else
            buf(b[i], g[i]);
    end
endmodule
```

## Dataflow modeling

```verilog
module Gray_to_Bin_Converter_Dataflow #(parameter size = 4)(
    bin,
    gray
    );
    
    output [size-1:0]bin;
    input [size-1:0]gray;
    
    genvar i;
    
    for(i=0; i<size; i=i+1)
    begin
        assign bin[i] =^ (gray >> i);
    end
endmodule
```

## Behavioral modeling

```verilog
module Gray_to_Bin_Converter_Behavioral #(parameter size = 4)(
    bin,
    gray
    );
    
    output reg [size-1:0]bin;
    input [size-1:0]gray;
    
    always@(gray)
    begin
        bin[i] =^ gray[i]>>1;
    end
endmodule
```
---

# Simulation Source

```verilog
module Tb_G2B();
    parameter size = 4;
    
    reg [size-1:0]g;
    wire [size-1:0]b;
    
    Gray_to_Bin_Converter_Behavioral sim_GB0(b, g);
//    Gray_to_Bin_Converter_Dataflow sim_GB1(b, g);
//    Gray_to_Bin_Converter_Structural sim_GB2(b, g);
    
    initial
    begin
        g = 4'b0000;
        
        #100 g=4'b0001;
        #100 g=4'b0010;
        #100 g=4'b0011;
        #100 g=4'b0100;
        #100 g=4'b0101;
        #100 g=4'b0110;
        #100 g=4'b0111;
        #100 g=4'b1000;
        #100 g=4'b1001;
        #100 g=4'b1010;
        #100 g=4'b1011;
        #100 g=4'b1100;
        #100 g=4'b1101;
        #100 g=4'b1111;
    end
endmodule
```
---

# Simulation data

![Tb_G2B](/images/2022-03-01-G2B/tb.png)
