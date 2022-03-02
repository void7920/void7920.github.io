---
title:  "Full Subtractor"
excerpt: "Combinational Logic Full Subtractor"

categories:
  - Combinational
tags:
  - [FPGA, Verilog, Logic, Combinational]

toc: true
toc_sticky: true
 
date: 2022-02-21
last_modified_at: 2022-03-01
---

# Truth Table

| &nbsp; &nbsp; A &nbsp; &nbsp; | &nbsp; &nbsp; B &nbsp; &nbsp; | &nbsp; &nbsp; Br<sub>in<sub> &nbsp; &nbsp; | &nbsp; &nbsp; D &nbsp; &nbsp; | &nbsp; &nbsp; Br<sub>out<sub> &nbsp; &nbsp; |
|:---:|:---:|:---:|:---:|:---:|
|  0  |  0  |  0  |  0  |  0   |
|  0  |  0  |  1  |  1  |  1   |
|  0  |  1  |  0  |  1  |  1   |
|  0  |  1  |  1  |  0  |  1   |
|  1  |  0  |  0  |  1  |  0   |
|  1  |  0  |  1  |  0  |  0   |
|  1  |  1  |  0  |  0  |  0   |
|  1  |  1  |  1  |  1  |  1   |

## Boolean Equation

D = A ⊕ Br ⊕ Br<sub>in</sub> 

Br = B<sub>in</sub> (A ⊕ B)' + A'B
---

# Logic

![FS](/images/2022-01-25-FS/logic.png)

---

# Design Source

## Structual modeling

```verilog
module FA_Structural(
	C_out,
	S,
	A,
	B,
	C_in
	);

	output C_out, 
	output S;
	input A; 
	input B;
	input C_in;

	wire w1, w2, w3;

	xor(w1, A, B);
	and(w2, T1, C_in);
	and(w3, A, B);
	
	xor(S, A, B, C_in);
	or(C_out, w2, w3);	
endmodule
```

## Dataflow modeling

```verilog
module FA_Dataflow(
	C_out,
	S,
	A,
	B,
	C_in
	);

	output C_out, 
	output S;
	input A; 
	input B;
	input C_in;

	assign S = (A ^ B ^ C_in);
	assign C_out = (A&B) | ((A^B) & C_in);
endmodule
```

## Behavioral modeling

```verilog
module FA_behavioral(
	C_out,
	S,
	A,
	B,
	C_in
	);

	output reg C_out, 
	output reg S;
	input A; 
	input B;
	input C_in;

	always@(A, B, C_in)
	begin
		case({C_in, B, A})
			3'b000:
			begin
				S = 1'b0;
				C = 1'b0;
			end

			3'b001:
			begin
				S = 1'b1;
				C = 1'b0;
			end

			3'b010:
			begin
				S = 1'b1;
				C = 1'b0;
			end

			3'b011:
			begin
				S = 1'b0;
				C = 1'b1;
			end
			
			3'b100:
			begin
				S = 1'b1;
				C = 1'b0;
			end

			3'b101:
			begin
				S = 1'b0;
				C = 1'b1;
			end

			3'b110:
			begin
				S = 1'b0;
				C = 1'b1;
			end

			3'b111:
			begin
				S = 1'b1;
				C = 1'b1;
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
	

	FA_Structural sim_FA( C, S, A, B, C_in );
//	FA_Dataflow sim_FA( C, S, A, B, C_in );
//	FA_behavioral sim_FA( C, S, A, B, C_in );

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

![Tb_FS](/images/2022-01-25-FS/tb.png)
