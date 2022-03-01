---
title:  "Binary to Gray Converter"
excerpt: "Combinational Logic Binary to Gray Converter"

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

| &nbsp; &nbsp; Decimal &nbsp; &nbsp; | &nbsp; &nbsp; Binary &nbsp; &nbsp; | &nbsp; &nbsp; Gray &nbsp; &nbsp; |
|:---:|:---:|:---:|
|  00  |  0000  |  0000  |
|  01  |  0001  |  0001  |
|  02  |  0010  |  0011  |
|  03  |  0011  |  0010  |
|  04  |  0100  |  0110  |
|  05  |  0101  |  0111  |
|  06  |  0110  |  0101  |
|  07  |  0111  |  0100  |
|  08  |  1000  |  1100  |
|  09  |  1001  |  1101  |
|  10  |  1010  |  1111  |
|  11  |  1011  |  1110  |
|  12  |  1100  |  1010  |
|  13  |  1101  |  1011  |
|  14  |  1110  |  1001  |
|  15  |  1111  |  1000  |

---

# Logic

// ![B2G](/images/2022-03-01-B2G/logic.png)

---

# Design Source

## Structual modeling

```verilog
module Bin_to_Gray_Converter_Structural#(parameter size = 4)(
    gray,
    bin
    );
    
    output [size-1:0]gray;
    input [size-1:0]bin;
    
    genvar i;
    for(i=0; i<size; i=i+1)
    begin
        if(i<size-1)
            xor(gray[i], bin[i+1], bin[i]);
        
        else
            buf(gray[i], bin[i]); 
    end
endmodule
```

## Dataflow modeling

```verilog
module Bin_to_Gray_Converter_Dataflow #(parameter size = 4)(
    gray,
    bin
    );
    
    output [size-1:0]gray;
    input [size-1:0]bin;
    
    assign gray = (bin >> 1) ^ bin;
endmodule
```

## Behavioral modeling

```verilog
module Bin_to_Gray_Converter_Behavioral #(parameter size = 4)(
    gray,
    bin
    );
    
    output reg [size-1:0]gray;
    input [size-1:0]bin;
    
    always@(bin)
    begin
        gray = bin>>1 ^ bin; 
    end
endmodule
```
---

# Simulation Source

```verilog
module Tb_B2G();
    parameter size = 4;
    
    reg [size-1:0]b;
    wire [size-1:0]g;
    
    Bin_to_Gray_Converter_Behavioral sim_BG0(g, b);
//    Bin_to_Gray_Converter_Dataflow sim_BG2(g, b);
//    Bin_to_Gray_Converter_Structural sim_BG1(g, b);
    
    initial
    begin
        b = 4'b0000;
        
        #100 b=4'b0001;
        #100 b=4'b0010;
        #100 b=4'b0011;
        #100 b=4'b0100;
        #100 b=4'b0101;
        #100 b=4'b0110;
        #100 b=4'b0111;
        #100 b=4'b1000;
        #100 b=4'b1001;
        #100 b=4'b1010;
        #100 b=4'b1011;
        #100 b=4'b1100;
        #100 b=4'b1101;
        #100 b=4'b1111;
    end
endmodule
```
---

# Simulation data

// ![Tb_B2G](/images/2022-03-01-B2G/tb.png)