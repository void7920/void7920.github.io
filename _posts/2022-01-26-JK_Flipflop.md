---
title:  "JK Flip-Flop"
excerpt: "Sequential Logic JK Flip-Flop"

categories:
  - Sequential
tags:
  - [FPGA, Verilog, Logic, Sequential]

toc: true
toc_sticky: true

date: 2022-01-26
last_modified_at: 2022-01-26
---

![JKFF1](/images/2022-01-26-JK_FLIPFLOP/logic4.png)

# Truth Table

| &nbsp; &nbsp; P' &nbsp; &nbsp; | &nbsp; &nbsp; R' &nbsp; &nbsp; | &nbsp; &nbsp; clk &nbsp; &nbsp; | &nbsp; &nbsp; J &nbsp; &nbsp; | &nbsp; &nbsp; K &nbsp; &nbsp; | &nbsp; &nbsp; Q<sub>n+1</sub> &nbsp; &nbsp; |
|:---:|:---:|:---:|:---:|:---:|:---:|
|  0  |  0  |  X  |  X  |  X  |  X  |
|  0  |  1  |  X  |  X  |  X  |  1  |
|  1  |  0  |  X  |  X  |  X  |  0  |
|  1  |  1  |  0  |  X  |  X  |  Q<sub>n</sub>  |
|  1  |  1  |  1  |  0  |  1  |  0  |
|  1  |  1  |  1  |  1  |  0  |  1  |
|  1  |  1  |  1  |  1  |  1  |  Q<sub>n</sub>'  |

## Boolean Equation

Q<sub>n+1</sub> = JQ<sub>n</sub>' + K'Q<sub>n</sub>

---

# Logic
JK FF
![JKFF1](/images/2022-01-26-JK_FLIPFLOP/logic.png)

JK FF w. D
![JKFF2](/images/2022-01-26-JK_FLIPFLOP/logic2.png)

JK FF Master-Slave
![JKFF3](/images/2022-01-26-JK_FLIPFLOP/logic3.png)
---

# Design Source

## Dataflow modeling

```verilog
module JK_FF(
    q,
    q_,
    clk,
    pre_n,
    clr_n,
    j,
    k
    );
    
    output q;
    output q_;
    input clk;
    input pre_n;
    input clr_n;
    input j;
    input k;
    
    assign q = ~(q_ & ~(j & clk & q_) & pre_n);
    assign q_ = ~(q & ~(k & clk & q) & clr_n);
endmodule
```

## Behavioral modeling

```verilog
module JK_FF(
	q,
	clk,
	pre_n,
	clr_n,
	j,
	k
	);
	
	output reg q;
	input clk;
	input pre_n;
	input clr_n;
	input j;
	input k

	always@(posedge clk, negedge pre_n, negedge clr_n)
	begin
		if(pre_n == 1'b0) // !pre_n
			q <= 1'b1;

		else if(clr_n == 1'b0) // !clr_n
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


## with D Flipflop

```verilog
module JK_FF(
	q,
	clk,
	pre_n,
	clr_n,
	j,
	k
	);
	
	output q;
	input clk;
	input pre_n;
	input clr_n;
	input j;
	input k;
	
	D_FF D(.q(q), .clk(clk), .pre_n(pre_n), .clr_n(clr_n), .d( ((j & ~q) | (~k & q)) ));
endmodule
```

---

# Simulation Source

```verilog
module Tb_JK_FF();
    reg clk;
    reg preset_n;
    reg clear_n;
    reg J;
    reg K;
    wire Q;
    
    JK_FF sim_JK(.q(Q), .clk(clk), .pre_n(preset_n), .clr_n(clear_n), .j(J), .k(K));
    
    initial
    begin
        clk = 1'b0;
        preset_n = 1'b0;
        clear_n = 1'b0;
        J = 1'b0;
        K = 1'b0;
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
            data_J;
            data_K;
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
    
    task data_J;
        forever #45 J = $urandom%2;
    endtask
       
    task data_K;
        forever #80 K = $urandom%2;
    endtask
endmodule
```
---

# Simulation data

![Tb_JKFF](/images/2022-01-26-JK_FLIPFLOP/tb.png)
