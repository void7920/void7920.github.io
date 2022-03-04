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
last_modified_at: 2022-03-04
---

# Logic

![RP_BCD](/images/2022-03-03-RP_BCD/logic.png)

---

# Design Source

```verilog
module BCD_Ripple(
    q,
    q_,
    clk,
    clr,
    L
    );
    
    output [3:0]q;
    output [3:0]q_;
    input clk;
    input clr; 
    input L;
    
    wire T0;
    and(T0, q[1], q[2]);
     
    JK_neg JKFF0(q[0], q_[0], clk, 1'b0, clr, L, L);
    JK_neg JKFF1(q[1], q_[1], q[0], 1'b0, clr, q_[3], L);
    JK_neg JKFF2(q[2], q_[2], q[1], 1'b0, clr, L, L);
    JK_neg JKFF3(q[3], q_[3], q[0], 1'b0, clr, T0, L);
endmodule
```
---

# Simulation Source

```verilog
module Tb_Ripple_BCD();
    reg clk, L, clr;
    wire [3:0]q;
    wire [3:0]q_;
    
    BCD_Ripple sim_BCD(q, q_, clk, clr, L);
    
    initial
    begin
        L = 1'b0;
        clr = 1'b1;
        
        clk = 1'b0;
        forever
            #10 clk = ~clk;
    end
    
    initial
    begin
        #10 clr = ~clr;
        
        #20 L = 1'b1;
        #1000 $stop;
    end
endmodule
```
---

# Simulation data

![RP_BCD](/images/2022-01-31-RP_BCD/tb.png)

<details>
<summary>Neg D FF</summary>
<div markdown="1">

Neg D FF

```verilog
module JK_neg(
	q,
	q_,
	clk,
	pre,
	clr,
	j,
	k
	);
	
	output reg q;
    output q_;
	input clk;
	input pre;
	input clr;
	input j;
	input k;

    assign q_ = ~q;

	always@(negedge clk, posedge pre, posedge clr)
	begin
		if(pre == 1'b1) // pre
			q <= 1'b1;

		else if(clr == 1'b1) // clr
			q <= 1'b0;

		else
		begin
			case( { j, k } )
				2'b00: q <= q; 
				2'b01: q <= 1'b0;
				2'b10: q <= 1'b1;
				2'b11: q <= ~q;
			endcase
		end
	end
endmodule
```

</div>
</details>