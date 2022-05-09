---
title:  "D Flip-Flop / Latch"
excerpt: "Sequential Logic D Flip-Flop"

categories:
  - Sequential
tags:
  - [FPGA, Verilog, Logic, Sequential]

toc: true
toc_sticky: true
 
date: 2022-01-26
last_modified_at: 2022-01-27
---

![DFF1](/images/2022-01-26-D_FLIPFLOP/logic3.png)

# Truth Table

| &nbsp; &nbsp; P' &nbsp; &nbsp; | &nbsp; &nbsp; R' &nbsp; &nbsp; | &nbsp; &nbsp; clk &nbsp; &nbsp; | &nbsp; &nbsp; D &nbsp; &nbsp; | &nbsp; &nbsp; Q<sub>n+1</sub> &nbsp; &nbsp; |
|:---:|:---:|:---:|:---:|:---:|:---:|
|  0  |  0  |  X  |  X  |  X  |
|  0  |  1  |  X  |  X  |  1  |
|  1  |  0  |  X  |  X  |  0  |
|  1  |  1  |  0  |  X  |  Q<sub>n</sub>  |
|  1  |  1  |  1  |  0  |  0  |
|  1  |  1  |  1  |  1  |  1  |

## Boolean Equation

Q<sub>n+1</sub> = D

---

# Logic
D Latch
![DLatch](/images/2022-01-26-D_FLIPFLOP/logic.png)

D FF master-slave
![DFF](/images/2022-01-26-D_FLIPFLOP/logic2.png)

---

## Dataflow modeling

```verilog
module D_FF_(
    q,
    q_,
    clk,
    pre_n,
    clr_n,
    d
    );
    
    output q;
    output q_;
    input clk;
    input pre_n;
    input clr_n;
    input d;
    
    assign q = ~(q_ & ~(d & clk) & pre_n);
    assign q_ = ~(q & ~(~(d & d) & clk) & clr_n);
endmodule
```

## Behavioral modeling

```verilog
module D_FF(
	q,
    q_,
	clk,
	pre_n,
	clr_n,
	d
	);
	
	output reg q;
    output q_;
	input clk;
	input pre_n;
	input clr_n;
	input d;

    assign q_ = ~q;
    
	always@(posedge clk, negedge pre_n, negedge clr_n)
	begin
		if(pre_n == 1'b0) // !pre_n
			q <= 1'b1;

		else if(clr_n == 1'b0) // !clr_n
			q <= 1'b0;

		else
			q <= d;
	end
endmodule
```

## Master-Slave modeling

```verilog
module D_FF_MS(
    q,
    q_,
    clk,
    pre_n,
    clr_n,
    d
    );
    
    output q;
    output q_;
    input clk;
    input pre_n;
    input clr_n;
    input d;
    
    wire DQ, DQ_;
    
    D_FF_ Master(.q(DQ), .q_(DQ_), .clk(clk), .pre_n(pre_n), .clr_n(clr_n), .d(d));
    SR_Latch Slave(.q(q), .q_(q_), .en(~(clk&clk)), .s(DQ), .r(DQ_));
endmodule
```

---

# Simulation Source

```verilog
module Tb_D_FF();
    reg clk;
    reg preset_n;
    reg clear_n;
    reg D;
    wire Q;
    
    D_FF sim_D(.q(Q), .clk(clk), .pre_n(preset_n), .clr_n(clear_n), .d(D));
    
    initial
    begin
        clk = 1'b0;
        preset_n = 1'b0;
        clear_n = 1'b0;
        D = 1'b0;
    end
    
    initial
    begin
        main;
    end
    
    task main;
        fork
            clk_gen;
            pre_op;
            rst_op;
            data;
        join
    endtask
    
    task clk_gen;
        forever #20 clk = ~clk;
    endtask
    
    task pre_op;
        #45 preset_n = 1'b1;
    endtask
    
    task rst_op;
        #105 clear_n = 1'b1;
    endtask
    
    task data;
        forever #25 D = ~D;
    endtask
endmodule
```
---

# Simulation data

![Tb_DFF](/images/2022-01-26-D_FLIPFLOP/tb.png)
