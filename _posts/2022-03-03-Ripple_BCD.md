---
title:  "BCD Ripple Counter"
excerpt: "BCD Ripple Counter"

categories:
  - Counter
tags:
  - [FPGA, Verilog, Logic, Counter]

toc: true
toc_sticky: true

date: 2022-03-03
last_modified_at: 2022-03-03
---

# Logic

![RP_T](/images/2022-03-03-RP_T/logic.png)

---

# Design Source

```verilog
module BCD_Ripple(
    output [3:0]q, [3:0]q_,
    input clk, L
    );
    
    wire T0;
    and(T0, q[1], q[2]);
     
    JK_FF JKFF0(q[0], q_[0], clk, 1'b1, 1'b1, L, L);
    JK_FF JKFF1(q[1], q_[1], q[0], 1'b1, 1'b1, q_[3], L);
    JK_FF JKFF2(q[2], q_[2], q[1], 1'b1, 1'b1, L, L);
    JK_FF JKFF3(q[3], q_[3], q[0], 1'b1, 1'b1, T0, L);
endmodule
```
---

# Simulation Source

```verilog
module Tb_BCD_RIPPLE();
    reg clk, L;
    wire [3:0]q;
    wire [3:0]q_;
    
    BCD_Ripple sim_BCD(q, q_, clk, L);
    
    initial
    begin
        L=1'b0;
        
        clk=1'b0;
        forever
            #10 clk = ~clk;
    end
    
    initial
    begin
        #20 L=1'b1;
        #1000 $stop;
    end
endmodule
```
---

# Simulation data

![RP_T](/images/2022-01-31-RP_T/tb.png)