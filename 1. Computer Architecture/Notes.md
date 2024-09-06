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


# 2. C Intro - Basics

## Compile vs interpret

Compilation and interpretation are the two main ways that a program gets run by a computer. 

- **C** is a **compiled** language, which means that the entire program is converted into a machine-specific executable before it can be run.
- **Java**, on the other hand, is an **interpreted** language. Java programs are first compiled into a **platform-independent bytecode**, which can then be run on any machine that has a **Java Virtual Machine (JVM)**.

Advantages and disadvantages of both compilation and interpretation. _Compiled_ languages are generally **faster** and more **efficient** than interpreted languages, but they can be **more difficult to debug** and they can be **machine-specific** (**Compiled and Executable files need to be rebuilt on each new system**). Interpreted languages are **easier to debug**, but they can be **slower** and **less efficient**.

- **Two-step process of compilation** in C: first, the .c file/program is compiled into **object files** (.o files), and then these object files are **linked** together with **libraries** to form the executable (**Assembling** is done automatically, hidden). **Makefiles** can help you to only recompile the files that you have changed, which can save you a lot of time. Thanks to this, you only need to compile the file with the new changes, and not all of them again.

<img width="436" alt="image" src="https://github.com/user-attachments/assets/f8620c84-1d27-460b-8d09-0154e5cf477e">

Why do people do scientific computation in Python? 
- Python has very good libraries for accessing GPU-specific resources.
- Python allows to drive multiple machines very easily (Spark)
- Python can also call low-level C code to do work (**Cython**)


The **C pre-processor (CPP)**: the C preprocessor is a program that processes your c code **before** it is compiled. It can be used to define macros, include header files, and conditionally compile code. Macros: macros are a way to define a piece of code that can be reused multiple times. Macros can be helpful for making your code more readable and maintainable, but they can also be **difficult to debug and lead to errors because macros are simply text-substituting, not variable assignment like in Python or other languages**.

Here is an example of a C preprocessor macro:

      #define PI 3.14159
      
This macro defines the value of PI to be 3.14159. You can then use the macro in your C code like this:

      area = PI * radius * radius;

## C v. Java and C Syntax

- C is a function language
- C is a machine dependant language
- Manual storage (vs garbage collection, allocation & initialization)

      int main(int argc, char *argv[])

**Purpose**: This is the standard declaration for the main function in C programs. It's used to receive command-line arguments passed to the program when it's executed from the terminal.
**Breakdown**:
- int main: Declares the main function, the entry point of the program.
- int argc: Stores the number of command-line arguments.
- char *argv[]: An array of character pointers, each pointing to a string representing a command-line argument.

**Why it's used instead of void main()**:
- Portability: More compatible across different operating systems and compilers.
- Standard compliance: Adheres to the C standard.
- Functionality: Provides a way to access and process command-line arguments.

For example, you might have a program that calculates the area of a rectangle. You could run it like this:

      area_calculator 5 10
      
Here, 5 and 10 are the command-line arguments. The program can use these arguments to calculate the area of a rectangle with a width of 5 units and a height of 10 units.

The int main(int argc, char *argv[]) part of the C code is like a special box that catches these command-line arguments.

- argc tells the program how many arguments were given (in this case, 2: the program name and the two numbers).
- argv is a list of the arguments themselves (in this case, argv[0] would be the program name, argv[1] would be "5", and argv[2] would be "10").


## Basic C Syntax

**What evaluates as false in C?**
- 0 (integer)
- null
- stdbool.h (C99)

Everything else is true.

- Must declare the type of the variable.
- Const are assigned when a value can't change during execution of the program. You can have different types of const (const int x, const float y, etc.)
- enum is a group of related integer constants. Assigns an order and it remains unchanged. enum color {green, blue, red}
- you have to declare type of what you return in a function
- variables & functions need to be declared before they are used. int number_of_people () {return 3; }
- typedef allow you to declare new data types and define the bit width
        typedef uint8_t BYTE;
        BYTE b1, b2;
- struct are structured type of variables
        struct myStructure {
        int myNum;
        char myLetter;
        };

      struct myStructure s1;
      s1.myNum = 13;
      s1.myLetter = 'B';

- better to add brackets in if statements to avoid identation confusion
- add breaks after each case in switch statements

## C Intro: Pointers, Arrays, Strings: Pointers and Bugs

- A variable may be initialized in its declaration. If not, we need to remember to initialize it afterwards, otherwise it will hold garbage inside (undefined content).
- A lot of C has **undefined behaviour**: **Heisenbugs** (bugs that are random/hard to reproduce, and seem to disappear or change when debugging). Bohrbugs are the opposite, they are reproducible.

Addresses vs values (pointers)

Memory could be thought as a very big array that starts at 0 and continues up to some value. Each cell of that array stores a value at a specific address. **An address refers or points to a particular memory allocation**. 

**Pointers are variables that contains the address of a variable**.

(I guess pointers also would have a value and an address, as they are variables as well...)

      int *p;
- Tells the compiler that variable p is the address of an int.
  
      p = &y;
- Tells the compiler to assign the address of y to p.
- & is called the “address operator” in this context.

      z = *p;
- Tells the compiler to assign the value at the address in p to z.
- * is called the “dereference operator” in this context.
 
<img width="438" alt="image" src="https://github.com/user-attachments/assets/9e5620ed-21fa-4f7c-8f5c-af556d615449">

to change a variable pointed to we can do:

      *p = 5;

Let's see an example of why pointers are important:

      void addOne (int x) {
          x = x + 1;
      }
      
      int y = 3;
      addOne(y);

The result would be that y = 3, still... Why?

1. Function Definition: The function addOne takes an integer x and increments it by 1
2. Passing by Value: When you call addOne(y), the value of y (which is 3) is copied into the function’s parameter x. This means x is a separate variable from y.
3. Local Change: Inside the function, x is incremented by 1, so x becomes 4. However, this change only affects the local variable x within the function. The original variable y outside the function remains unchanged.
4. After the Function Call: Once the function finishes executing, the local variable x is discarded, and y is still 3 because it was never directly modified.

So, y stays 3 because the function addOne only changes its local copy of the value, not the original variable y.

The correct way of incrementing y value is using pointers: 

      void addOne (int *p) {
          *p = *p + 1;
      }
      
      int y = 3;
      
      addOne(&y);

**DANGER**: pointers should always point to a variable. For example, 

      int *ptr;
      *ptr = 5;

in this code we are telling the program to assign the value 5 at _any_ address because the pointer was declared without initialization (it holds garbage, points to any direction), and we lose control of memory allocation.

**Why pointers?**: in spite of being a big source of bugs, pointers allow us to pass (to someone or something) a large data structure as a pointer instead of having to copy the whole data structure. It is faster and more efficient.
