---
title:  "Parallel Adder / Subtractor"
excerpt: "Combinational Logic Parallel Adder / Subtractor"

categories:
  - Combinational
tags:
  - [FPGA, Verilog, Logic, Combinational]

toc: true
toc_sticky: true

date: 2022-02-21
last_modified_at: 2022-02-21
---

# Logic

// ![PAS](/images/2022-02-21-PAS/logic.png)

---

# Design Source

```verilog
module Parallel_Adder_Subtractor #(parameter size = 4)(
    c_out,      // carry out
    s,          // sum
    ovf,        // overflow
    sel,         // mode 0 = add / 1 = sub
    a,          // a
    b,          // b
    );
    
    output reg c_out;
    output reg [size-1:0]s;
    output ovf;
    input sel;
    input [size-1:0]a;
    input [size-1:0]b;
    
    wire [size-1:0]carry;
    wire [size-1:0]xr;
    
    assign c_out = carry[size-1];
    assign ovf = carry[size-1] | carry[size-2];
    
    genvar i;
    generate
        for(i=0; i<size; i=i+1)
        begin:PAS
            xor(xr[i], sel, b[i]);
             
            if(i==0)
            begin: FA
                Full_Adder_Behavioral fa(.c_out(carry[i]), .s(s[i]), .c_in(sel), .a(a[i]), .b(xr[i]));
            end
            
            else
            begin: FA
                Full_Adder_Behavioral fa(.c_out(carry[i]), .s(s[i]), .c_in(carry[i-1]), .a(a[i]), .b(xr[i]));
            end
        end
    endgenerate
endmodule
```

```verilog
module Tb_PAS();
    parameter size = 4;
    
    reg [size:0]in0;  // maximum input is 2^(size-1), becuz in[size] is "sel"
    reg [size-1:0]in1;
    
    wire [size:0]s;
    
    Parallel_Adder_Subtractor #(size) sim_PAS(.c_out(s[size]), .s(s[size-1:0]), .ovf(), .sel(in0[size]), .a(in0), .b(in1));
    
    initial
    begin
        in0 = 0;
        in1 = 0;
    end
    
    initial
    begin
        #50  in0[size]=0; in0[size-1:0]=16; in1=14;
        #50  in0[size]=1; in0[size-1:0]=15; in1=15;
        #50  in0[size]=0; in0[size-1:0]=3; in1=12;
        #50  in0[size]=1; in0[size-1:0]=12; in1=4;
        #50  in0[size]=0; in0[size-1:0]=10; in1=8;
        #50  in0[size]=1; in0[size-1:0]=9; in1=3;
        #50  in0[size]=0; in0[size-1:0]=8; in1=10;
        #50  in0[size]=1; in0[size-1:0]=13; in1=11;
    end
endmodule
```
---

# Simulation data

// ![Tb_PAS](/images/2022-02-21-PAS/tb.png)

# Simulation data
