---
title:  "Half Adder"
excerpt: "Combinational Logic Half Adder"

category:
  - Combinational
tags:
  - [FPGA, Verilog, Logic, Combinational]

toc: true
toc_sticky: true
 
date: 2022-01-25
last_modified_at: 2022-01-25
---

# Truth Table

|  A  |  B  |  S  |  C  |
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
module HA_Structual(
    	C,
	S,
	A,
	B
    	);

	output C;
	output S;
	input A;
	input B;
    
	xor(S, A, B);
	and(C, A, B);
endmodule
```

## Dataflow modeling

```verilog
module HA_Dataflow(
	C,
	S,
	A,
	B
    	);

	output C;
	output S;
	input A;
	input B;

	assign S = A ^ B;
	assign C = A & B;
endmodule
```

## Behavioral modeling

```verilog
module HA_behavioral(
	C,
	S,
	A,
	B
    	);

	output reg C;
	output reg S;
	input A;
	input B;

	case( { b, a } )
		2'b00 : 
		begin
			S = 1'b0;
			C = 1'b0;
		end

		2'b01 : 
		begin
			S = 1'b1;
			C = 1'b0;
		end
            
            	2'b10 :
		begin
			S = 1'b1;
			C = 1'b0;
		end
            
            	2'b11 :
		begin
			S = 1'b1;
			C = 1'b1;
		end
	endcase
endmodule
```
---

# Simulation Source

```verilog
module Tb_HA();
	reg A, B;
	wire C, S;
    
	HA_Structual sim_HA( C, S, A, B );
//	HA_HA_Dataflow sim_HA( C, S, A, B );
//	HA_behavioral sim_HA( C, S, A, B );
    
	initial
	begin
		A=0;
		B=0;
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
