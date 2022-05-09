---
title:  "Multiplier"
excerpt: "Multiplier"

categories:
  - Multiplier
tags:
  - [FPGA, Verilog, Logic]

toc: true
toc_sticky: true

date: 2022-04-13
last_modified_at: 2022-04-13
---

# Table

| | | | | | a<sub>3</sub> | a<sub>2</sub> | a<sub>1</sub> | a<sub>0</sub> |
| X | | | | | b<sub>3</sub> | b<sub>2</sub> | b<sub>1</sub> | b<sub>0</sub> |
|:---:|:---:|:---:|
| | | | | | a<sub>3</sub>b<sub>0</sub> | a<sub>2</sub>b<sub>0</sub> | a<sub>1</sub>b<sub>0</sub> | a<sub>0</sub>b<sub>0</sub> |
| | | | | a<sub>3</sub>b<sub>1</sub> | a<sub>2</sub>b<sub>1</sub> | a<sub>1</sub>b<sub>1</sub> | a<sub>0</sub>b<sub>1</sub> | |
| | | | a<sub>3</sub>b<sub>2</sub> | a<sub>2</sub>b<sub>2</sub> | a<sub>1</sub>b<sub>2</sub> | a<sub>0</sub>b<sub>2</sub> | | |
| | | a<sub>3</sub>b<sub>3</sub> | a<sub>2</sub>b<sub>3</sub> | a<sub>1</sub>b<sub>3</sub> | a<sub>0</sub>b<sub>3</sub> | | | |
| | r7 | r6 | r5 | r4 | r3 | r2 | r1 | r0 |

---

# Design Source

## Dataflow Modeling

```verilog
module Multiplier #(parameter msb = 4)(
  y,
  a,
  b
  )

  output [msb*2-1:0]y;
  input [msb-1:0]a;
  input [msb-1:0]b;

  assign y = a * b;
endmodule
```
---

## Modular Modeling

```verilog
module Multiplier #(parameter msb = 4)(
	output [msb*2-1:0]y;
	input [msb-1:0]a, b;
	);

	reg [msb-1:0] ram [msb-1:0];

	integer i, j;
	
		
endmodule
```
---


# Simulation Source

```verilog

```
---

# Simulation data

<details>
<summary>Half Adder</summary>
<div markdown="1">

---

Half Adder

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

	assign s = a ^ b;
	assign c = a & b;
endmodule
```

</div>
</details>

<details>
<summary>Full Adder</summary>
<div markdown="1">

Full Adder

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
endmodule
```

</div>
</details>