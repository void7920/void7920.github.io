---
title:  "Half Adder"
excerpt: "Combinational Logic Half Adder"

categories:
  - Combinational
tags:
  - [FPGA, Verilog, Logic, Combinational]

toc: true
toc_sticky: true
 
date: 2022-01-25
last_modified_at: 2022-03-03
---

# Truth Table

| &nbsp; &nbsp; A &nbsp; &nbsp; | &nbsp; &nbsp; B &nbsp; &nbsp; | &nbsp; &nbsp; S &nbsp; &nbsp; | &nbsp; &nbsp; C &nbsp; &nbsp; |
|:---:|:---:|:---:|:---:|
|  0  |  0  |  0  |  0  |
|  0  |  1  |  1  |  0  |
|  1  |  0  |  1  |  0  |
|  1  |  1  |  0  |  1  |

## Boolean Equation

S = A ⊕ B

C = A × B

---

# Logic

![HA](/images/2022-01-25-HA/logic.png)

---

# Design Source

## Structual modeling

```verilog
module Half_Adder_Structual(
    c,
	s,
	a,
	b
    );

	output c;
	output s;
	input a;
	input b;
    
	xor(s, a, b);
	and(c, a, b);
endmodule
```

## Dataflow modeling

```verilog
module Half_Adder_Dataflow(
	c,
	s,
	a,
	b
    );

	output c;
	output s;
	input a;
	input b;

	assign {c, s} = a + b;
//	assign s = a ^ b;
//	assign c = a & b;
endmodule
```

## Behavioral modeling

```verilog
module Half_Adder_behavioral(
	c,
	s,
	a,
	b
    );

	output reg c;
	output reg s;
	input a;
	input b;

	always@(a, b)
	begin
		{c, s} = a + b;
//		case( { b, a } )
//			2'b00 : 
//			begin
//				s = 1'b0;
//				c = 1'b0;
//			end
//
//			2'b01 : 
//			begin
//				s = 1'b1;
//				c = 1'b0;
//			end
//
//		    2'b10 :
//			begin
//				s = 1'b1;
//				c = 1'b0;
//			end
//
//		    2'b11 :
//			begin
//				s = 1'b1;
//				c = 1'b1;
//			end
		endcase
	end
endmodule
```
---

# Simulation Source

```verilog
module Tb_HA();
	reg A, B;
	wire C, S;
    
	Half_Adder_Structual sim_HA( C, S, A, B );
//	Half_Adder_Dataflow sim_HA( C, S, A, B );
//	Half_Adder_behavioral sim_HA( C, S, A, B );
    
	initial
	begin
		A = 0;
		B = 0;
	end
    
	initial
	begin
		#100 A=0; B=1;
		#100 A=1; B=0;
		#100 A=1; B=1;
	end
endmodule
```
---

# Simulation data

![Tb_HA](/images/2022-01-25-HA/tb.png)
