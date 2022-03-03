---
title:  "Universal Shift Register"
excerpt: "Universal Shift Register"

categories:
  - Register
tags:
  - [FPGA, Verilog, Logic, Register]

toc: true
toc_sticky: true

date: 2022-01-27
last_modified_at: 2022-03-03
---

# Logic

![USR](/images/2022-01-27-USR/logic.png)
---

# Design Source

```verilog
module Universal_Shift_Register #(parameter size=4) (
    o,
    clk,
    clear_n,
    sel0,
    sel1,
    left,
    right,
    i
    );
    
    output [size-1:0]o;
    input clk;
    input clear_n;
    input sel0;
    input sel1;
    input left;
    input right;
    input [size-1:0]i;
    
    wire [size-1:0]muxo;
        
    genvar v;
    generate
        for(v=0; v<size; v=v+1)
        begin : USR
            if(v == 0) 
            begin : MUX 
                MUX4x1 mux(.o(muxo[v]), .sel0(sel0), .sel1(sel1), .i0(o[v]), .i1(o[v+1]), .i2(left), .i3(i[v]));
            end
                        
            else if(v == size-1)
            begin : MUX
                MUX4x1 mux(.o(muxo[v]), .sel0(sel0), .sel1(sel1), .i0(o[v]), .i1(right), .i2(o[v-1]), .i3(i[v]));
            end
            
            else
            begin : MUX
                MUX4x1 mux(.o(muxo[v]), .sel0(sel0), .sel1(sel1), .i0(o[v]), .i1(o[v+1]), .i2(o[v-1]), .i3(i[v]));
            end
            
            D_FF DFF(.q(o[v]), .q_(), .clk(clk), .pre_n(1'b1), .clr_n(clear_n), .d(muxo[v]));
        end
    endgenerate
    
endmodule
```
---

# Simulation Source

```verilog
module Tb_SISO();
    reg clk, clr_n, i;
    wire o;
    
    SISO sim_SISO(.o(o), .clk(clk), .clear_n(clr_n), .i(i));
    
    initial
    begin
        clr_n = 1'b0;
        i = 1'b0;
        
        clk =1'b0;
        forever
            #10 clk = ~clk;
    end
    
    initial
    begin
        #20 i=1'b0; clr_n=1'b0;
        #20 i=1'b1; clr_n=1'b0;
        #20 i=1'b1; clr_n=1'b1;
        #20 i=1'b0; clr_n=1'b1;
        #20 i=1'b1; clr_n=1'b1;
        #20 i=1'b0; clr_n=1'b1;
        #20 i=1'b0; clr_n=1'b1;
        #20 i=1'b0; clr_n=1'b1;
        #20 i=1'b0; clr_n=1'b1;
    end
endmodule
```
---

# Simulation data
