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

![FS](/images/2022-02-21-FS/logic.png)

---

# Design Source

## Structual modeling

```verilog
module Full_Subtractor_Structural(
    br_out,     // borrow out
    d,          // difference
    a,          // a
    b,          // b
    br_in       // borrow in
    );
   
    output br_out;
    output d;
    input a;
    input b;
    input br_in;
        
    wire w1, w2, w3;
    
    xor(w1, a, b);
    and(w2, ~w1, br_in);
    and(w3, ~a, b);
    
    xor(d, a, b, br_in);
    or(br_out, w2, w3);
endmodule
```

## Dataflow modeling

```verilog
module Full_Subtractor_Dataflow(
    br_out,     // borrow out
    d,          // difference
    a,          // a
    b,          // b
    br_in       // borrow in
    );
   
    output br_out;
    output d;
    input a;
    input b;
    input br_in;
    
    assign d = (a ^ b ^ br_in);
    assign br_out = (~a & b) | (~(a ^ b) & br_in);
endmodule
```

## Behavioral modeling

```verilog
module Full_Subtractor_Behavioral(
    br_out,     // borrow out
    d,          // difference
    a,          // a
    b,          // b
    br_in       // borrow in
    );
   
    output reg br_out;
    output reg d;
    input a;
    input b;
    input br_in;
    
    always@(*)
    begin
        case({a, b, br_in})
            3'b000:
                begin
                    d = 1'b0;
                    br_out = 1'b0;
                end
            3'b001:
                begin
                    d = 1'b1;
                    br_out = 1'b1;
                end
            3'b010:
                begin
                    d = 1'b1;
                    br_out = 1'b1;
                end
            3'b011:
                begin
                    d = 1'b0;
                    br_out =1'b1;
                end 
            3'b100:
                begin
                    d = 1'b1;
                    br_out = 1'b0;
                end
            3'b101:
                begin
                    d = 1'b0;
                    br_out = 1'b0;
                end
            3'b110:
                begin
                    d = 1'b0;
                    br_out = 1'b0;
                end
            3'b111:
                begin
                    d = 1'b1;
                    br_out = 1'b1;
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

![Tb_FS](/images/2022-02-21-FS/tb.png)
