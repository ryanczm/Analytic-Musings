---
layout: post
title: How Processors Work - My Learnings

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

This article or note documents my exploration into seeing how computers work at a low level. I think it's good to even have a basic understanding because every time you write or a computer executes code, you can connect it back to your understanding.  So, just by pressing the right buttons and sending the right signals to a computer, you can get it to manipulate information for you! That is crazy to think about.

I make heavy use of the semi-deterministic framework (see my post on metacognition: thinking about thinking). The idea is to look at abstractions as input-output. And the lowest level of abstraction closest to hardware without worrying about the circuitry of the processor is: machine code or assembly. These notes were from Hussein Nasser's OS course and ChatGPT 4o. I'm not a developer but a quant/ds.

# Processors

## Abstractions (Take for Granted)

* Compiler. How the first compilers were made directly from hand-coding in machine code.
* How compiler generates machine code. Instructions generating instructions.
* Visual drivers. How processor/GPU handles screen displaying.
* Peripherals (Video, audio). How these technologies convert bits (current) to light and soundwaves.
* Physical layer. How these technologies convert bits to signals.
* Circuitry. How processor hardware is made. Magic of chip supply chain (aka Conway's Material World).

## Basic Model of Computation

* Fetch Decode Execute Cycle - Computers work according to fetch-execute-decode cycle, and repeat this. In practice, this is extremely complex - multiple cores, multiple programs, pipelining, etc. (Img: pipeline, FED cycle)
* Memory - A useful abstraction is to view it is a (near) infinitely long list. But IRL, it's a bunch of flip flop/latches each with their own wire that can be written/read to, any one of them. 
* Instruction Set - An instruction is a sequence of bits that belongs to the instruction set family, which is unique to each processor. 32 or 64 bits. E.g x86 ISA. AVX is a SIMD extension to x86. Says how to interpret the sequence of bits.
* Instruction - Bit sequence, segmented into opcode, data, register address, etc. 
* Assembly - The lowest level of abstractions computers give humans access to without getting into actual hardware. That's why Hotz says to learn programming you should learn assembly and C.
* Program and Source Code - A program refers to low level instructions. Source code refers to written code by humans.
* Compiler or Interpreter - A program that takes source code and converts it to machine program code.
* MMU - Unit that maps physical to virtual memory. Why? Imagine two binaries referencing that same address: `0x400000000`. Problem. Luckily with VM/MMU, we can fix this.

## Basic Processor Model

* Abstraction Layers
  * Atoms > Diodes/transistor > logic gates > circuits > components > processor core > cores
* Clock
  * Tells computer to execute instructions.
* Control Unit
  * Coordinates clock, fetch-execute-decode cycle.
* ALU
  * Executes instructions, aka opcodes.
* Registers
  * General purpose - Accumulator, base, counter, data, source, destination, stack pointer, stack base pointer
  * Pointer - Instruction pointer
  * Segment - Code, data, extra, stack, GP F, GP G
  * Flags - Carry, parity, aux, zero, sign, trap, interrupt enable, direction, overflow, etc
* Cache
* Simplified Unit
  * This is a bare bones, simple model of a processor. Real life processors are much more complex beasts. 
  * For example, i9 13900 has 24 cores. Within each core, multiple ALU, FPU, VPU, etc. 64 bit words. 
* Bus/Lines
  * Set of wires connecting CPU to memory and CPU to peripherals, storage, etc.

## Instruction Sets: x86 

* Control Flow
  * Jumps - The heart of control flow and loops. 
  * Conditional Jumps - Conditional jump instructions make use of flags in the flag register which is updated from ALU computations.
* Data Transfer (Bus Management)
  * Moving - To copy data (the bit pattern via current) from one place to another (memory to register, register to register, memory to register). Moving to and from
  * Push, Pop - Between stack and registers/memory
  * In/Out - Transfer data between IO ports (bus) and registers 
* Arithmetic
* Logical
* Flag Control
* Floating Point
* Interrupts (IO)
  * Int

## Cache

* Cache controller circuit handles this. Cache placement and retrieval policy. Extremely complex. Store blocks of memory: tag, index, offset. 
* Register (1ns), L1 (2ns), L2 (10ns), L3 (20ns), memory (100ns), storage (100μs). Speed diffs due to physical structure: cache is SRAM, memory is DRAM.
 **Data** & **instruction** separate caches. E.g when data is written to a memory address, write to cache too, or when CPU retrieves data from memory address, first check cache via cache lines.
* Assembly - Not directly accessible via assembly (except for 1 or 2 instructions). Programmers cannot directly modify cache but can write cache-optimized code.
* Replacement Policy - When memory is read/written, it gets cached (the whole block/line via tag, index, offset). Then evicted based on **replacement**: LRU, FIFO, random replacement etc. Eviction MUST occur as cache is much smaller than memory!
 

## Building Blocks of Languages

* Atomic Operations
  * Data type manipulation
  * Compound data type manipulation
* Composites
  * Branching (Control Flow) - executing instructions based on the result of another instruction.
  * Iteration - Repeating sequence of instructions fixed times/condition is met.
  * Functions - Calling and returning, executing same sequence of instructions in different data.
