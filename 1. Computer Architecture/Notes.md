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

There is an issue: **negative numbers**. How to represent them? 

If we have 5 bits, when we reach 31 (11111) and we add another one we go back to 00000. This is called **overflow**. 

If we use **unsigned** binary notation we only have **positive numbers** (like in C's **unsigned** integers). One solution for negative numbers would be to use **sign/magnitude**, that is to say, to use the first digit to represent positive (0) and negative (1): being 10001 = -1 and 00001 = 1. But here we encounter overflow again and once we hit 15 (01111), adding one number will lead us to negative 0 (10000) and then -1 (10001) and so on... therefore we do not have a single line bringing us to the minimum value to the maximum and viceversa.

<img width="380" alt="image" src="https://github.com/user-attachments/assets/b5b7f4c9-46d1-47db-85e0-d27185e3434e">

We could then try to use **one's complement**, which is basically substituting the 0's by 1's to have the counter negative part of a positive and viceversa: 00001 (1) is positive and 11110 it is its negative counterpart (-1). This way have a single line (binary odometer is correct):

<img width="442" alt="image" src="https://github.com/user-attachments/assets/12a6f7ca-9f1d-4d1a-8762-b0ed2272feec">

But here we still have overlap (postive 0 and negative 0)

## Number Representation: Two's Complement, Bias, and Summary

**Two's Complement** fixes overlapping by adding a one. 

- To have -1 we first substitute 0s by 1s like in One's complement: 00001 --> 11110
- and then add a one: 11110 --> 11111 = -1.
- This way 00000 = 0 and 11111 = -1. No overlapping.

<img width="408" alt="image" src="https://github.com/user-attachments/assets/db223a41-a141-451c-9744-0576b92f0cae">

Last issue is that we have more negatives than positives: -16 (10000), 15 (01111).

That is why we add a **bias**. From this:

<img width="282" alt="image" src="https://github.com/user-attachments/assets/cc41b50b-bf81-4104-ac2e-459190a81964">

we pull the values down (add a -15 bias) and go to this:

<img width="278" alt="image" src="https://github.com/user-attachments/assets/9014e9b2-fe00-41c2-96cb-7be1ead241dd">

We are here using positives as 1s and negatives as 0s. We also have positive up to 16 and negative up to -15. In this case we used a -15 bias as the bias formula is represented as **-(2^N-1 - 1)**. Having 5 bits it's then -(2^5-1 - 1) = -15.

Interesting fact is that it is the **same hardware** for **unsigned** and **Two's Complement**.
