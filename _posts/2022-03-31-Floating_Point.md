---
title:  Floating Point
excerpt: "How to work in Floating Point"

categories:
  - Verilog
tags:
  - [Verilog, Logic, Counter]

toc: true
toc_sticky: true

date: 2022-03-31
last_modified_at: 2022-03-31
---

# IEEE 754 Floating Point standard
## 16 bit (Half Prescision)

| <center><span style="color:skyblue">15</span></center> | <center><span style="color:turquoise">14</span></center> | <center><span style="color:turquoise">13</span></center> | <center><span style="color:turquoise">12</span></center> | <center><span style="color:turquoise">11</span></center> | <center><span style="color:turquoise">10</span></center> | <center><span style="color:pink">9</span></center> | <center><span style="color:pink">8</span></center> | <center><span style="color:pink">7</span></center> | <center><span style="color:pink">6</span></center> | <center><span style="color:pink">5</span></center> | <center><span style="color:pink">4</span></center> | <center><span style="color:pink">3</span></center> | <center><span style="color:pink">2</span></center> | <center><span style="color:pink">1</span></center> | <center><span style="color:pink">0</span></center> |
|:---:|:---:|:---:|
| <center><span style="color:skyblue">S</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> |

15bit - <span style="color:skyblue">Sign</span> **(1bit size)**

14 ~ 10bit - <span style="color:turquoise">Exponentional</span> **(5bit size)**

9 ~ 0bit - <span style="color:pink">Mantissa</span> **(10bit size)**

## 32 bit (single Prescision)

| <center><span style="color:skyblue">31</span></center> | <center><span style="color:turquoise">30</span></center> | <center><span style="color:turquoise">29</span></center> | <center><span style="color:turquoise">28</span></center> | <center><span style="color:turquoise">27</span></center> | <center><span style="color:turquoise">26</span></center> | <center><span style="color:turquoise">25</span></center> | <center><span style="color:turquoise">24</span></center> | <center><span style="color:turquoise">23</span></center> | <center><span style="color:pink">22</span></center> | <center><span style="color:pink">21</span></center> | <center><span style="color:pink">20</span></center> | <center><span style="color:pink">19</span></center> | <center><span style="color:pink">18</span></center> | <center><span style="color:pink">17</span></center> | <center><span style="color:pink">16</span></center> | <center><span style="color:pink">15</span></center> | <center><span style="color:pink">14</span></center> | <center><span style="color:pink">13</span></center> | <center><span style="color:pink">12</span></center> | <center><span style="color:pink">11</span></center> | <center><span style="color:pink">10</span></center> | <center><span style="color:pink">9</span></center> | <center><span style="color:pink">8</span></center> | <center><span style="color:pink">7</span></center> | <center><span style="color:pink">6</span></center> | <center><span style="color:pink">5</span></center> | <center><span style="color:pink">4</span></center> | <center><span style="color:pink">3</span></center> | <center><span style="color:pink">2</span></center> | <center><span style="color:pink">1</span></center> | <center><span style="color:pink">0</span></center> |
|:---:|:---:|:---:|
| <center><span style="color:skyblue">S</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> |

31bit - <span style="color:skyblue">Sign</span> **(1bit size)**

30 ~ 23bit - <span style="color:turquoise">Exponentional</span> **(8bit size)**

22 ~ 0bit - <span style="color:pink">Mantissa</span> **(23bit size)**

## 64 bit (double Prescision)

| <center><span style="color:skyblue">63</span></center> | <center><span style="color:turquoise">62</span></center> |<center><span style="color:turquoise">61</span></center> | <center><span style="color:turquoise">60</span></center> | <center><span style="color:turquoise">59</span></center> | <center><span style="color:turquoise">58</span></center> | <center><span style="color:turquoise">57</span></center> | <center><span style="color:turquoise">56</span></center> | <center><span style="color:turquoise">55</span></center> | <center><span style="color:turquoise">54</span></center> | <center><span style="color:turquoise">53</span></center> | <center><span style="color:turquoise">52</span></center> | <center><span style="color:pink">51</span></center> | <center><span style="color:pink">50</span></center> | <center><span style="color:pink">49</span></center> | <center><span style="color:pink">48</span></center> | <center><span style="color:pink">47</span></center> | <center><span style="color:pink">46</span></center> | <center><span style="color:pink">45</span></center> | <center><span style="color:pink">44</span></center> | <center><span style="color:pink">43</span></center> | <center><span style="color:pink">42</span></center> | <center><span style="color:pink">41</span></center> | <center><span style="color:pink">40</span></center> | <center><span style="color:pink">39</span></center> | <center><span style="color:pink">38</span></center> | <center><span style="color:pink">37</span></center> | <center><span style="color:pink">36</span></center> | <center><span style="color:pink">35</span></center> | <center><span style="color:pink">34</span></center> | <center><span style="color:pink">33</span></center> | <center><span style="color:pink">32</span></center> | <center><span style="color:pink">31</span></center> | <center><span style="color:pink">30</span></center> | <center><span style="color:pink">29</span></center> | <center><span style="color:pink">28</span></center> | <center><span style="color:pink">27</span></center> | <center><span style="color:pink">26</span></center> | <center><span style="color:pink">25</span></center> | <center><span style="color:pink">24</span></center> | <center><span style="color:pink">23</span></center> | <center><span style="color:pink">22</span></center> | <center><span style="color:pink">21</span></center> | <center><span style="color:pink">20</span></center> | <center><span style="color:pink">19</span></center> | <center><span style="color:pink">18</span></center> | <center><span style="color:pink">17</span></center> | <center><span style="color:pink">16</span></center> | <center><span style="color:pink">15</span></center> | <center><span style="color:pink">14</span></center> | <center><span style="color:pink">13</span></center> | <center><span style="color:pink">12</span></center> | <center><span style="color:pink">11</span></center> | <center><span style="color:pink">10</span></center> | <center><span style="color:pink">9</span></center> | <center><span style="color:pink">8</span></center> | <center><span style="color:pink">7</span></center> | <center><span style="color:pink">6</span></center> | <center><span style="color:pink">5</span></center> | <center><span style="color:pink">4</span></center> | <center><span style="color:pink">3</span></center> | <center><span style="color:pink">2</span></center> | <center><span style="color:pink">1</span></center> | <center><span style="color:pink">0</span></center> |
|:---:|:---:|:---:|
| <center><span style="color:skyblue">S</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:turquoise">E</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> | <center><span style="color:pink">M</span></center> |

63bit - <span style="color:skyblue">Sign</span> **(1bit size)**

62 ~ 52bit - <span style="color:turquoise">Exponentional</span> **(11bit size)**

51 ~ 0bit - <span style="color:pink">Mantissa</span> **(52bit size)**


# Calculation Method

Half Prescision

(-1)<sup><span style="color:skyblue">Sign</span></sup> x (1.0 + 0.<span style="color:pink">Mantissa</span>) x 2<sup>(<span style="color:turquoise">Exponentional</span>+15)</sup>

---

Signle Prescision

(-1)<sup><span style="color:skyblue">Sign</span></sup> x (1.0 + 0.<span style="color:pink">Mantissa</span>) x 2<sup>(<span style="color:turquoise">Exponentional</span>+127)</sup>

---

Double Prescision

(-1)<sup><span style="color:skyblue">Sign</span></sup> x (1.0 + 0.<span style="color:pink">Mantissa</span>) x 2<sup>(<span style="color:turquoise">Exponentional</span>+1023)</sup>

---

<details>
<summary>example</summary>
<div markdown="1">

---

23.35 = 10111.010110011001001101<sub>2</sub>

10111.01011001100<sub>2</sub> -> (-1)<sup><span style="color:skyblue">0</span></sup> x (1.0 + 0.<span style="color:pink">01110101100100</span>) x 2<sup>(<span style="color:turquoise">(127+4)</span>-127)</sup>

<span style="color:skyblue">0</span><span style="color:turquoise">10000011</span><span style="color:pink">01110101100110011001101<span>

---

<span style="color:skyblue">1</span><span style="color:turquoise">10000101</span><span style="color:pink">11011010100000000000000<span>

(-1)<sup><span style="color:skyblue"></span>1</sup> x (1.0 + 0.<span style="color:pink">110110101</span>) x 2<sup>(<span style="color:turquoise">(127+6)</span>-127)</sup>

-1110110.101<sub>2</sub>

-118.625

</div>
</details>

# in Verilog

```verilog

module FP16(
  FP16,
  sign,
  exp,
  mantissa
  );

  input [31:0]FP16; // 32bit

  assign sign = FP[31];
  assign exp = FP[30:23];
  assign mantissa = FP[22:0];
  
endmodule

```
