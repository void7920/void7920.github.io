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
last_modified_at: 2022-03-04
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
module Tb_USR();
    parameter size=4;
    
    reg clk;
    reg clr_n;
    reg s0;
    reg s1;
    reg l;
    reg r;
    reg [size-1:0]in;
    wire [size-1:0]out;
    
    Universal_Shift_Register #(size) sim_USR(.o(out), .clk(clk), .reset_n(clr_n), .sel0(s0), .sel1(s1), .left(l), .right(r), .i(in) );
    
    initial
    begin
        clk = 1'b0;
        clr_n = 1'b0;
        s0 = 1'b0;
        s1 = 1'b0;
        l = 1'b0;
        r = 1'b0;
    end
    
    initial
    begin       
        main;
    end
    
    task main;
        fork
            clk_gen;
            clr_n_op;
            load_op;
            sel0_op;
            sel1_op;
            lshift;
            rshift;
        join
    endtask
    
    task clk_gen;
        forever #10 clk = ~clk;
    endtask
    
    task clr_n_op;
        #10 clr_n = ~clr_n;
    endtask
    
    task load_op;
        begin
            forever #40 in = $urandom%16;
        end
    endtask
    
    task sel0_op;
        begin
            forever #20 s0 = ~s0;
        end
    endtask
    
    task sel1_op;
        begin
            forever #30 s1 = ~s1;
        end
    endtask
    
    task lshift;
        begin
            forever
            begin
                #60 l = ~l;
                #20 l = ~l;
            end
        end
    endtask
    
    task rshift;
        begin
            forever
            begin
                #40 r = ~r;
                #20 r = ~r;
            end
        end
    endtask

endmodule
```
---

# Simulation data
