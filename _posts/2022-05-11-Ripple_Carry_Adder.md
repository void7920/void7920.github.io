---
title:  "Ripple Carry Adder"
excerpt: "Combinational Logic Ripple Carry Adder"

categories:
  - Combinational
tags:
  - [FPGA, Verilog, Logic, Combinational]

toc: true
toc_sticky: true

date: 2022-05-11
last_modified_at: 2022-05-11
---

# Logic

![RCA](/images/2022-05-11-RCA/Logic.png)

---

# Design Source

```verilog
module Ripple_Carry_Adder #(parameter size = 4)(
    c_out,      // carry out
    s,          // sum
    a,          // a
    b           // b
    );
    
    output c_out;
    output [size-1:0]s;
    input [size-1:0]a;
    input [size-1:0]b;
    
    wire [size-1:0]carry;
    
    assign c_out = carry[size-1];
    
    genvar i;
    generate
        for(i=0; i<size; i=i+1)
        begin:RCA
            if(i==0)
            begin: FA
                Full_Adder_Dataflow fa(.c_out(carry[i]), .s(s[i]), .c_in(1'b0), .a(a[i]), .b(b[i]));
            end
            
            else
            begin: FA
                Full_Adder_Dataflow fa(.c_out(carry[i]), .s(s[i]), .c_in(carry[i-1]), .a(a[i]), .b(b[i]));
            end
        end
    endgenerate
endmodule
```

# Design Source with CLA

CLA is Carry Lookahead Adder

recommend using maximum size 4 bit at CLA


```verilog
module cla_nbit#(parameter msb = 4)(
    co,         // carry out
    s,          // sum
    ci,         // carry in
    a,          // a
    b           // b
    );
    
    output co;
    output [msb-1:0]s;
    input ci;
    input [msb-1:0]a;
    input [msb-1:0]b;
    
    wire [msb-1:0]c;
    wire [msb-1:0]cc;
    
    assign co = cc[msb-1];
    
    genvar i;
    generate
        for(i=0; i<msb; i=i+1)
        begin:PA
            if(i==0)
            begin: FA
                fa FA(.co(c[i]), .s(s[i]), .ci(ci), .a(a[i]), .b(b[i]));
            end
            
            else
            begin: FA
                fa FA(.co(c[i]), .s(s[i]), .ci(cc[i-1]), .a(a[i]), .b(b[i]));
            end
        end
    endgenerate
    
    cl #(msb) CLA(.c(cc), .g(a&b), .p(a^b), .ci(ci));
endmodule
```

<details>
<summary>Neg T FF</summary>
<div markdown="1">

Carry Lookahead logic

```verilog
module cl #(parameter msb = 4)(
    c,
    g,
    p,
    ci
    );
    
    output [msb-1:0]c;
    input [msb-1:0]g;
    input [msb-1:0]p;
    input ci;
    
    genvar i;
    generate
        for(i=0; i<msb; i=i+1)
        begin
            if(i==0)
                assign c[i] = g[i] | (p[0]&ci);
            else
                assign c[i] = g[i] | (p[i]&c[i-1]);
        end
    endgenerate
endmodule
```

</div>
</details>

# Simulation Source

```verilog
module Tb_RCA();
    parameter size = 4;
    
    reg [size:0]in0;
    reg [size-1:0]in1;
    
    wire [size:0]s;
    
    Ripple_Carry_Adder #(size) sim_RCA(.c_out(s[size]), .s(s[size-1:0]), .a(in0), .b(in1));
    
    initial
    begin
        in0 = 0;
        in1 = 0;
    end
    
    initial
    begin
        #50  in0[size-1:0]=16; in1[size-1:0]=$random%(2**size);
        #50  in0[size-1:0]=15; in1[size-1:0]=$random%(2**size);
        #50  in0[size-1:0]=3; in1[size-1:0]=$random%(2**size);
        #50  in0[size-1:0]=12; in1[size-1:0]=$random%(2**size);
        #50  in0[size-1:0]=10; in1[size-1:0]=$random%(2**size);
        #50  in0[size-1:0]=9; in1[size-1:0]=$random%(2**size);
        #50  in0[size-1:0]=8;  in1[size-1:0]=$random%(2**size); 
        #50  in0[size-1:0]=13; in1[size-1:0]=$random%(2**size);
    end
endmodule
```
---

# Simulation data

![Tb_RCA](/images/2022-05-11-RCA/tb.png)
