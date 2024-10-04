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
 
# Operating Systems

## Basics

* Model
  * A low level program/process that coordinates other processes and always runs. Userspace programs interact with kernel (core process) via system calls.
  * An OS is more than kernel! Tooling, GUI, command lines, UI, etc, all 'APIs' to kernel. A distro (e.g Ubuntu) is a compiled, slightly customized kernels with additional bells and whistles.
  * The problem gets more complex when you realize a CPU has multiple cores (e.g 12) ...
  * **All running their own (different) processes (scheduling/switching) and the kernel process copy too! Which have their own threads! Sharing the same infinitely long memory list!** With multiprocessing/multithreading!
* Kernel
  * Can think of it as meta-instructions. E.g a scheduler is instructions itself in kernel space that dictates how userspace process/thread instructions are spun up/executed.

## Responsibilities

* Process Management & Scheduling - Coordinate different processes & itself (including startup)
* File (Disk) Management - Navigating disk by implementing the filesystem, coordinating from/to memory and disk. Storage is mostly blocks of bytes.
* Memory Management - Managing memory allocation, virtual memory
* IO - Managing input and output from devices: mouse/keyboard, monitor, etc. A problem: can understand internals of processes, but not how the visuals (GPU or monitor) are done.
* Networking - Implementing network protocols/stack.

## Process Management & Scheduling

* Scheduling Theory
  * Takes from operations management, e.g queue (FIFO), shortest job first (SJF), round robin time slice (RR), multi-level feedback queue (MLFQ). E.g have a circular buffer/queue (of pointers to PCBs) that RRs through processes, matter of switching current context, and selecting new context.
* Virtualization
  * The key idea is that programs (instructions/binaries) need their own carefully protected space/memory in the infinitely long list to operate in without fear of outside world
* Process Control Block (PCB)
  * In kernel space. Process scheduling state, page info, IPC info (semaphore, mutex, shared memory, messages) , state, privileges, PID, registers content (e.g `eax` ,`eip`, `esp`), scheduling info, memory management info.
  * KPT or kernel process table: mapping table/list from PID to PCB.
* Context Switch
  * Hardware timer interrupt on a core (another circuit, programmable by kernel). Forces CPU to look up the IVT (interrupt vector table), say `int 0x20`, and use the right interrupt handler address in kernel space.
  * In this case, updates PCB, selects next task (from linked PCB list), loads in PCB (e.g registers), starts executing. But how does it know what order to do so?
* Threads (Mini-process)
  * Idea - Processes isolate address space but threads are parallel entities that share address space.
  * Threads - Region of memory within a process, has own TCB, 
  * Thread Control Block - Own registers (PC, SP, BP, HP, etc), pointer to parent PCB. KTB: PID, TID, TCB

## Process Memory Layout

* States
  * Running, ready, blocked (IO waiting)
  * Analogy: computer scientist takes out a recipe for baking cake and starts baking it. But diff people can follow same receipt and based on their IO, the cake (state) goes differently.
* Stack
  * Abstraction maintained by stack pointer in `sp`. `bp` points to bottom of current frame. To allocate in nested call, push some values (func params, local vars) onto the stack, return address on link register, increment pointer. 
  * Some careful management of addresses in `sp` and `bp` to create the abstraction, all references via offsets (high to low) But to plan out offsets nicely, the entire call structure must be known by compiler! Hence compilation.
  * In reality, it's thread stacks, not process stacks. So multiple stacks in the stack region.
* Heap
  * Low to high. For dynamic allocation for non-local variables at runtime. E.g you `malloc` and `free` some bytes then put in an array. That goes on heap.
  * Remain until explicitly removed. Pointers (absolute reference) for heap. Heap synonymous with pointers. Memory leak: heap memory with no reference counts lying around, unused. Dangling pointers.
  * GC can be manual (programmer writes code that compiles to do GC, runtime) or compiler native (compiler will output GC instructions automatically).
* Code Section
  * Where the code (binary instructions) are. Your source code, library code, standard library code, etc. Load from storage INTO here.
* Data 
  * Where global scope, static variables are defined (aka not in function scope). Address won't change unless destroyed. Pointer to start e.g `[#DATA,4]`, will not change, can offset! Anything can access.
* Runtime vs Compile
  * Ideally, you'd lay out everything at compile time. But sometimes, inputs (IO) will determine runtime state.

## Concurrency (Process & Threads)

* Model
  * Multiple processes in memory. 
  * Async/time slice view (Core view) - One core executes threads from multiple processes in a time-sliced fashion. ALways context switching. Boom boom. In native Python, this is timeslicing.
  * Sync/parallel view (Process view) - One process has threads on multiple processors executing. Boom boom. 
* Why?
  * Why would we want to have one process having threads executing on multiple cores? Simple: reduce CPU bound tasks and IO bound tasks wait time!
  * If you are distributing threads across cores, you perform better for CPU bound tasks. When thread is IO bound or waiting you just context switch to another process.
* Core Communication, APIC and IPI
  * How do cores communicate? E.g kernel scheduler on one core instructs another core to execute a thread. 
  * IPI (inter processor interrupt) is a signal from one core to another. The APIC (local and global) is circuitry. Each local APIC has an ICR (interrupt command register).
  * By writing to this ICR via `wrmsr`, you can get one core to interrupt another core. Cool!
  * APIC (advanced programmable interrupt controller) circuitry has buses linking the cores!
* Kernel Concurrency
  * The same logic happens for kernel space! Say core 1 handles a network packet. Core 2 is running the scheduler, core 3 is handling filesystem etc.

## Concurrency Control

* Categories
  * Multicore vs Single Core - Synch problems can occur on the single core case and multicore case.
  * Race conditions - Multiple threads have instructions to access shared memory concurrently (could be multicore or single core). Whichever is faster wins.
  * Data races - Multiple threads are mutating some shared data 
  * Deadlock - Two or more threads are waiting to for each other to release resources.
  * Livelock - Threads keep changing state in response to each other.
  * Starvation - Thread is denied access to resources for a long time because higher priority things are given preference.
  * Priority Inversion - The higher priority thread  gets starved wrongly.
* Synchronization Idea
  * Apparently, you can do this with programming (higher level) or atomic ops (assembly like `lock` or `xchg`), available in a C interface or something.
  * The GIL mechanism in Python interpreter is an example: cannot have multiple threads running across cores synchronously.
  * Multithreading in Python is single core interleave time slice. Multiprocessing in Python is true multicore.
* Sync Primitives
  * Mutex (Binary Lock)
  * Semaphore

## (Virtual) Memory Management 

* Fragmentation
  * Allocating contiguous blocks of physical memory leads to alot of holes. So, we want to emulate the illusion of contiguous memory, virtually, while physical memory is all over the place.
* MMU & Paging
  * The circuit that handles this. Split memory into smaller blocks, called pages. Allocate pages and keep track of pages! Map logical pages to virtual pages. Page is the granular unit. 
* Shared Memory
  * E.g (fixed) library code. Say 5 processes of same program. ALl need library code in the code section. Do we load in 5 times? No! We use virtual memory to point to one part of library code. E.g multithreading/multiprocessing.
* Direct Memory Access (DMA)
  * Alternate circuitry that lets peripherals or disk bypass CPU direct to RAM. This frees up CPU to do other tasks (no bus hogging). Separate lane?
  * More of a hardware modification. 

## Storage & Files

* SSDs (Storage)
  * Structure - NAND flash, circuitry, controller, cache (DRAM). See [Adkins on FTL](https://www.youtube.com/watch?v=o68T7c82foA&ab_channel=JonathanAdkins) & [Branch Education on SSD](https://www.youtube.com/watch?v=r-SivgEpA1Q&ab_channel=BranchEducation)
  * Charge flash trap - RAM is just transistor & capacitor. SSD is expensive charge flash trap.
  * Page and block - Read/write at 100μs (1e3 times slower than RAM). Divided into pages: 4kb-16kb and blocks (128 pages or 512kb to 2mb). Page is the atomic unit of storage. One block is a unit of pages.
  * Read, write, erasure - Read & write at page level. Cannot only write to blank pages (with existing charge flash trap state). But can only erase at the block level. This is related to hardware design. 
  * SSD Controller - The controller (processor inside SSD) coordinates. Maintain FTL table, communicate with processor.
* SSD Algorithms
  * Garbage collection - Controller does this when idle. Find mixed blocks, copy valid pages in block to fresh new block (consolidation), and erase old block. Minimize entropy (block level).
  * Flash translation layer - A table (array) of LBA (logical block address) to physical address mapping (e.g at LBA addr 5 is phys addr 2051). Maintained in SSD cache/DRAM by controller. And persisted in a special reserved NAND area. 
  * OS & LBA - The OS can then treat SSD as a massive array of virtual blocks (LBA). It has no idea where the physical address is.
  * Overall - FTL tables in SSD DRAM is updated, pointer to physical block. E.g OS updates LBA 1 from data X to Y. Controller shifts pointer from page A to B, writes new data to B, marks A invalid. 
  * Wear levelling - NAND cells have write limit. So you want to distribute your erasing/writing to optimize shelf life (amortize loss across pages).
  * Locality - Matters less in SSD than in HDD (because of NAND flash vs physical rotating disk needle). Can scatter across the SSD blocks. But matters for write amp. FTL helps.
* Filesystem
  * Human readable file/directory structure - Similar to IP address/DNS, a human readable directory is needed for humans to navigate and find things! 
  * File metadata to LBA - The filesystem job is to map file data and metadata to LBAs, then SSD controller handles LBA to PBA, SSD infra handles the actual electrons.
  * Filesystem - Abstraction above storage. Key is caching: speed. Remember, the key is to load files quickly and manage it.
  * Data Format vs FS - FS manages how to retrieve the files. But the files themselves are bits. How the data is encoded in bits is the ... data format or database! E.g parquet, arrow, etc. 
  * Waiter vs cook analogy - FS is the waiter/staff system in a restaurant. Format is the cook and the food.
  * FAT32 - Some old 32bit FS.

# Databases & Object Storage