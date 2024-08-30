# 1. Number representation

## Introduction

Bits can represent anything. To calculate how many bits you need to represent a specific number of things, remember that N bits = 2^N things, or log2(N things) = N bits 

e.g. if I want to represent 32 types of cat and want to know how many bits I need: log2(32 cats) = 5 bits

## Number Representation: Conversions

When we use regular digits, we tend to refer to base 10 notation. For example: 342 is actually (3 x 10^2) + (4 x 10^1) + (2 x 10^1). However, there are also other types of notations: **hexadecimal** (base 16) and **binary** (base 2). 

- Hexadecimal digits: 0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F
- Binary digits: 0, 1

How much is 34 in binary? 

34 = **1** x 2^5 + **0** x 2^4 + **0** x 2^3 + **0** x 2^2 + **1** x 2^1 + **0** x 2^0 = **100010**

How much is 34 in hexadecimal?

34 = **2** x 16^1 + **2** x 16^0 = **22**

165 = **A** x 16^1 + **5** x 16^0 = **A5**

Decimal to Hexadecimal to Binary conversion table:
<img width="444" alt="image" src="https://github.com/user-attachments/assets/3a7e5312-e6b5-4b20-8cdd-573812cef658">


## Overflow, Sign and Magnitude, One's Complement

