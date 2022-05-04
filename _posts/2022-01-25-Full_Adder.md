---
title:  "Full Adder"
excerpt: "Combinational Logic Full Adder"

categories:
  - Combinational
tags:
  - [FPGA, Verilog, Logic, Combinational]

toc: true
toc_sticky: true
 
date: 2022-01-25
last_modified_at: 2022-03-10
---

# Truth Table

| &nbsp; &nbsp; A &nbsp; &nbsp; | &nbsp; &nbsp; B &nbsp; &nbsp; | &nbsp; &nbsp; C<sub>in<sub> &nbsp; &nbsp; | &nbsp; &nbsp; S &nbsp; &nbsp; | &nbsp; &nbsp; C<sub>out<sub> &nbsp; &nbsp; |
|:---:|:---:|:---:|:---:|:---:|
|  0  |  0  |  0  |  0  |  0   |
|  1  |  0  |  0  |  1  |  0   |
|  0  |  1  |  0  |  1  |  0   |
|  1  |  1  |  0  |  0  |  1   |
|  0  |  0  |  1  |  1  |  0   |
|  1  |  0  |  1  |  0  |  1   |
|  1  |  1  |  1  |  1  |  1   |

## Boolean Equation

S = A ⊕ B ⊕ C<sub>in</sub>

C<sub>out</sub> = A × B + C<sub>in</sub>(A ⊕ B)

---

# Logic

![FA](/images/2022-01-25-FA/logic.png)

---

# Design Source

## Structual modeling

```verilog
module Full_Adder_Structural(
	c_out,
	s,
	a,
	b,
	c_in
	);

	output c_out, 
	output s;
	input a; 
	input b;
	input c_in;

	wire w1, w2, w3;

	xor(w1, a, b);
	and(w2, T1, c_in);
	and(w3, a, b);
	
	xor(s, a, b, c_in);
	or(c_out, w2, w3);	
endmodule
```

## Dataflow modeling

```verilog
module Full_Adder_Dataflow(
	c_out,
	s,
	a,
	b,
	c_in
	);

	output c_out; 
	output s;
	input a; 
	input b;
	input c_in;

	assign s = (a ^ b ^ c_in);
	assign c_out = (a&b) | ((a^b) & c_in);

//	assign {c_out, s} = a + b + c_in;
endmodule
```

## Behavioral modeling

```verilog
module Full_Adder_behavioral(
	c_out,
	s,
	a,
	b,
	c_in
	);

	output reg c_out;
	output reg s;
	input a; 
	input b;
	input c_in;

	always@(a, b, c_in)
	begin
//		{c_out, s}  = a + b + c_in;
		case({c_in, b, a})
			3'b000:
			begin
				s = 1'b0;
				c_out = 1'b0;
			end

			3'b001:
			begin
				s = 1'b1;
				c_out = 1'b0;
			end

			3'b010:
			begin
				s = 1'b1;
				c_out = 1'b0;
			end

			3'b011:
			begin
				s = 1'b0;
				c_out = 1'b1;
			end
			
			3'b100:
			begin
				s = 1'b1;
				c_out = 1'b0;
			end

			3'b101:
			begin
				s = 1'b0;
				c_out = 1'b1;
			end

			3'b110:
			begin
				s = 1'b0;
				c_out = 1'b1;
			end

			3'b111:
			begin
				s = 1'b1;
				c_out = 1'b1;
			end
		endcase
	end
endmodule
```
---

# Simulation Source

```verilog
module Tb_FA();
 	reg A, B, C_in;
	wire C, S;
	

	Full_Adder_Structural sim_FA( C, S, A, B, C_in );
//	Full_Adder_Dataflow sim_FA( C, S, A, B, C_in );
//	Full_Adder_behavioral sim_FA( C, S, A, B, C_in );

	initial
	begin
		A=0;
		B=0;
		C_in=0;
	end

	initial
	begin
		#100 A=0; B=0; C_in=1;
		#100 A=0; B=1; C_in=0;
		#100 A=0; B=1; C_in=1;
		#100 A=1; B=0; C_in=0;
		#100 A=1; B=0; C_in=1;
		#100 A=1; B=1; C_in=0;
		#100 A=1; B=1; C_in=1;
	end
endmodule
```
---

# Simulation data

![Tb_FA](/images/2022-01-25-FA/tb.png)
