---
layout: post
title: How Processors Run Conditions & Loops (Core Dumped)

---

<style>
  .twitter-tweet {
    width: 30%;  /* Adjust the width as needed */
    margin: 0 auto;  /* Center the blockquote horizontally */
  
  }

    .twitter-tweet p {
    font-size: 14px !important; /* Change the font size */
  }

</style>

This article or note documents my exploration into seeing how computers work at a low level. I think it's good to even have a basic understanding because every time you write or a computer executes code, you can connect it back to your understanding.


### Basic Model of the Computation

* Fetch Decode Execute Cycle - Computers work according to fetch-execute-decode cycle, and repeat this. In practice, this is extremely complex - multiple cores, multiple programs, pipelining, etc. (Img: pipeline, FED cycle)
* Memory - A useful abstraction is to 
* Instruction Set - An instruction is a sequence of bits that belongs to the instruction set family, which is unique to each processor. 32 or 64 bits. E.g x86 ISA. AVX is a SIMD extension to x86. Says how to interpret the sequence of bits.
* Instruction - Bit sequence, segmented into opcode, data, register address, etc. 
* Assembly - The lowest level of abstractions computers give a human access to without getting into actual hardware. That's why Hotz says to learn programming you should learn assembly and C.
* Program and Source Code - A program refers to low level instructions. Source code refers to written code by humans.
* Compiler or Interpreter - A program that takes source code and converts it to machine program code.

### CPU Basic Model (Core)

* Clock - Tells computer to execute instructions.
* Control Unit - Coordinates clock, fetch-execute-decode cycle.
* ALU - Executes instructions, aka opcodes.
* Registers - Control and ALU use this to store stuff.
* Much More - It's not that simple.

### x86 ISA

* Registers
  * General purpose - Accumulator, base, counter, data, source, destination, stack pointer, stack base pointer
  * Pointer - Instruction pointer
  * Segment - Code, data, extra, stack, GP F, GP G
  * Flags - Carry, parity, aux, zero, sign, trap, interrupt enable, direction, overflow, etc
* Instructions
  * Jumps - The heart of control flow and loops. Conditional jump instructions make use of flags in the flag register which is updated from ALU computations.
  * Moving - To copy data (the bit pattern via current) from one place to another (memory to register, register to register, memory to register). By making the right connection, current flows.
  * Pushing and Popping - Moving data from registers to stack or vice versa and incrementing/decrementing stack pointers. Stack is used in function calls to store data from caller while jumping to the function instructions. Functions execute in LIFO fashion.

### Basic Building Blocks of Programming Languages

* Atomic Operations
  * Data type manipulation
  * Compound data type manipulation
* Composites
  * Branching (Control Flow) - executing instructions based on the result of another instruction.
  * Iteration - Repeating sequence of instructions fixed times/condition is met.
  * Functions - Calling and returning, executing same sequence of instructions in different data.
