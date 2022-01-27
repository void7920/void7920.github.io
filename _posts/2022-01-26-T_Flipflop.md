---
title:  "T Flip-Flop"
excerpt: "Sequential Logic T Flip-Flop"

categories:
  - Sequential
tags:
  - [FPGA, Verilog, Logic, Sequential]

toc: true
toc_sticky: true

date: 2022-01-26
last_modified_at: 2022-01-27
---

![TFF1](/images/2022-01-26-T_FLIPFLOP/logic4.png)

# Truth Table

| &nbsp; &nbsp; P' &nbsp; &nbsp; | &nbsp; &nbsp; R' &nbsp; &nbsp; | &nbsp; &nbsp; clk &nbsp; &nbsp; | &nbsp; &nbsp; T &nbsp; &nbsp; | &nbsp; &nbsp; Q<sub>n+1</sub> &nbsp; &nbsp; |
|:---:|:---:|:---:|:---:|:---:|:---:|
|  0  |  0  |  X  |  X  |  X  |
|  0  |  1  |  X  |  X  |  1  |
|  1  |  0  |  X  |  X  |  0  |
|  1  |  1  |  0  |  X  |  Q<sub>n</sub>  |
|  1  |  1  |  1  |  0  |  Q<sub>n</sub>  |
|  1  |  1  |  1  |  1  |  Q<sub>n</sub>'  |

## Boolean Equation

Q<sub>n+1</sub> = TQ<sub>n</sub>' + T'Q<sub>n</sub>

---

# Logic
T FF
![TFF1](/images/2022-01-26-T_FLIPFLOP/logic.png)

T FF w. D FF
![TFF2](/images/2022-01-26-T_FLIPFLOP/logic2.png)

---

# Design Source

## Dataflow modeling

```verilog
module T_FF(
    q,
    q_,
    clk,
    pre_n,
    clr_n,
    t
    );
    
    output q;
    output q_;
    input clk;
    input pre_n;
    input clr_n;
    input t;
    
    assign q = ~(q_ & ~(t & clk & q_) & pre_n);
    assign q_ = ~(q & ~(t & clk & q) & clr_n);
endmodule
```

## Behavioral modeling

```verilog
module T_FF(
	q,
	clk,
	pre_n,
	clr_n,
	t
	);
	
	output reg q;
	input clk;
	input pre_n;
	input clr_n;
	input t;

	always@(posedge clk, negedge pre_n, negedge clr_n)
	begin
		if(pre_n == 1'b0) // !pre_n
			q <= 1'b1;

		else if(clr_n == 1'b0) // !clr_n
			q <= 1'b0;

		else if(t)
			q <= ~q;
	end
endmodule
```

## with D Flipflop

```verilog
module T_FF(
	q,
	clk,
	pre_n,
	clr_n,
	t
	);
	
	output q;
	input clk;
	input pre_n;
	input clr_n;
	input t;
	
	D_FF D(.q(q), .clk(clk), .pre_n(pre_n), .clr_n(clr_n), .d(t^q));
endmodule
```

---

# Simulation Source

```verilog
module Tb_T_FF();
    reg clk;
    reg preset_n;
    reg clear_n;
    reg T;
    wire Q;
    
    T_FF sim_T(.q(Q), .clk(clk), .pre_n(preset_n), .clr_n(clear_n), .t(T));
    
    initial
    begin
        clk = 1'b0;
        preset_n = 1'b0;
        clear_n = 1'b0;
        T = 1'b0;
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
        #65 preset_n = 1'b1;
    endtask
    
    task rst_op;
        #155 clear_n = 1'b1;
    endtask
    
    task data;
        forever #250 T = ~T;
    endtask
endmodule
```
---

# Simulation data

![Tb_TFF](/images/2022-01-26-T_FLIPFLOP/tb.png)
