# RISC-V

> Welcome to the RISC-V CPU Core Design Workshop, known as MYTH (Microprocessor for You in Thirty Hours). This repository is your one-stop destination for all the information and resources you'll need to participate in this exciting 5-day workshop offered by VLSI System Design (VSD) in collaboration with Redwood EDA.

## Overview

> The MYTH workshop is an intensive program that guides you through the process of designing a RISC-V based CPU core. In just five days, you'll gain a deep understanding of the RISC-V Instruction Set Architecture (ISA) and implement a simplified RISC-V core with a fundamental instruction set. This repository holds the key to your success in the MYTH workshop.

## Key Highlights

- **RISC-V ISA Basics**: You will dive into the essentials of the RISC-V ISA, laying the foundation for core design.
- **Five-Day Challenge**: In a remarkably short span of five days, you will take on the challenge of creating a RISC-V CPU core.
- **Innovative Design Approach**: We leverage the power of Transaction Level Verilog (TL-Verilog) for a streamlined and efficient design process.
- **Makerchip IDE Platform**: You will work with the Makerchip IDE platform, which provides a user-friendly environment for core development.

## Contents

In this repository, you will find:

- **Workshop Materials**: Comprehensive resources, including presentations, documentation, and design guidelines.
- **TL-Verilog Code**: Sample TL-Verilog code for RISC-V core design.
- **Makerchip Setup**: Instructions on setting up the Makerchip IDE for your design work.
- **Additional Resources**: Supplementary materials and links to deepen your understanding of RISC-V and core design.

## Get Started

To kickstart your journey into RISC-V CPU core design, simply explore the contents of this repository. The resources provided here will guide you through each step of the workshop. Enjoy your MYTH journey and the fascinating world of RISC-V CPU core design!

<a name="top"></a>
  
##### This Repository Guides you to complete the RISC-V-BASED MYTH Course Assignments - Guided by Kunal Ghosh


# Table of contents


# COURSE 
<details>
<summary> DAY 1: Introduction to RISCV ISA and GNU Compiler Toolchain</summary>
<br>

## Introduction to Risc-v Basic Keywords
- **Instruction Set Architecture(ISA)**
  - An Instruction Set Architecture (ISA) refers to the set of instructions that a computer's central processing unit (CPU) can understand and execute. It defines the interface between software and hardware, specifying the operations that a CPU can perform, the data types it can manipulate, and the memory addressing modes it supports.

- **Risc-V ISA**
  - Risc-V ISA is an open-source ISA that has simpler and fixed length instructions that allows us to create custom processors for specific needs without being tied to proprietary architectures
 
- **Tools Used for the flow**
  - As we are aware of the flow, we will be using Risc-v ISA ALP and the RTL used will be picorv32a (We will be using rv64i during initial stages)

# Goal : Any High level Program that is written should be able to get executed in our CHIP

### List of well-known extensions present in Risc-V ISA

``` rv32i``` ``` rv64i``` ```rv32imc``` ```rv64imc``` ```rv32imafdc``` ```rv64imafdc``` ```rv32imcb``` ```rv64imcb``` ```rv32imc_sv32``` ```rv64gcv```

### Extensions and their Applications

- **I (Integer)** :The I set includes the base integer instruction set for RISC-V. It provides fundamental integer arithmetic and logical operations, data movement, and control flow instructions.
  - ADD, SUB, AND, OR, XOR, ADDI, SLTI, JAL, BEQ, LW

- **M (Multiply and Divide)** : The M set adds integer multiplication and division instructions to the base integer set. These instructions are particularly useful for arithmetic-heavy computations.
  - MUL, MULH, DIV, REM
  
- **A (Atomic)** : The A set introduces atomic memory access instructions. These instructions enable multiple operations on memory locations to be performed atomically, ensuring that other processors or threads cannot observe intermediate states.
  - LR (Load-Reserved), SC (Store-Conditional), AMO (Atomic Memory Operation)
  
- **F (Single-Precision Floating-Point)**: The F set adds single-precision floating-point instructions. These instructions enable arithmetic operations on 32-bit floating-point numbers.
  - FADD.S, FSUB.S, FMUL.S, FDIV.S, FCVT.W.S, FCVT.S.W

- **D (Double-Precision Floating-Point)** : The D set includes double-precision floating-point instructions. These instructions allow arithmetic operations on 64-bit floating-point numbers.
  - FADD.D, FSUB.D, FMUL.D, FDIV.D, FCVT.W.D, FCVT.D.W

- **C (Compressed)** : The C set introduces a compressed instruction format that reduces the size of code. Compressed instructions maintain the same functionality as their non-compressed counterparts but use shorter encodings.
  - C.ADDI4SPN, C.LWSP, C.ADDI, C.SW, C.JALR, C.BEQZ

- **G (Atomic and Lock-Free Operations)** : The G set, also known as the "GAS Set," is an alternative to the A set. It focuses on providing atomic and lock-free instructions to simplify hardware implementation.
  - LRV (Load-Reserved Variant), SCV (Store-Conditional Variant), AMO (Atomic Memory Operation Variants)

- **V (Vector)** :The V set adds vector instructions to the ISA, enabling Single Instruction, Multiple Data (SIMD) operations. These instructions allow efficient parallel processing of data elements in vectors.
  - VADD, VMUL, VFMADD, VLW, VSW

- **S (Supervisor)** : The S set, often used in privileged modes, includes instructions for managing and interacting with the supervisor-level operations of the system, such as handling exceptions and interrupts.
  - ECALL, EBREAK, SRET, MRET, WFI

- **B (Bit Manipulation)** : The B set introduces instructions for bit manipulation operations, allowing efficient manipulation of individual bits in registers and memory.
  - ANDI, ORI, XORI, SLLI, SRLI, SRAI

## 1. Create a simple C program That calculates sum from 1 to N -> sum_1_to_N.c

_____Compile it using C compiler_____
```
gcc sum1ton.c -o sum1ton.o
./a.out
```
-o allows you to name your output file

![PIC 1](https://github.com/Akarsh-Hegde/pes_asic_class/assets/79705687/f64d395e-6718-4ce6-b145-db1cbe1e5264)



_____compile using riscv compiler and view the output_____
```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
spike pk sum1ton.o
```
![PIC 2](https://github.com/Akarsh-Hegde/pes_asic_class/assets/79705687/60f492fd-407a-4842-bbc8-479d9c1be77b)




- ```-O<number>``` : level of optimisation required
- ```-mabi``` : specifies the ABI (Application Binary Interface) to be used during code generation according to the requirements
- ```-march``` : specifies target architecture

_______We can check the different options available for all these fields using the commands_______ 
go to the directory where riscv64-unkonwn-elf is present
- -O1 : ``` riscv64-unkonwn-elf --help=optimizer```
- -mabi : ```riscv64-unknown-elf-gcc --target-help```
- -march : ```riscv64-unknown-elf-gcc --target-help```

_____To view the disassembled ALP code_____
```
riscv64-unkonwn-elf-objdump -d sum1ton.o
```

_____To debug the ALP generated by the compiler_____
```
spike -d pk sum1ton.o
```

![PIC 3](https://github.com/Akarsh-Hegde/pes_asic_class/assets/79705687/ab586741-11f1-47a8-8123-74d82fac9ee6)


- press ENTER : shows the first line and successive ENTER shows successive lines
- reg 0 a2 : checks content of register a2 0th core
- q : quit the debug process

##### Difference between the ALP commands when used different optimizers
- use the command ```riscv64-unknown-elf-objdump -d sum1ton.o | less```
- use ``` /instance``` to search for an instance 
- press ENTER
- press ```n``` to search next occurance
- press ```N``` to search for previous occurance. 
- use ```esc :q``` to quit

_____Contents of main when used -O1 optimizer_____

![PIC 4](https://github.com/Akarsh-Hegde/pes_asic_class/assets/79705687/0f187ec6-2171-4848-8106-6298fca32658)


_____contents of main when used -Ofast optimizer_____

![PIC 5](https://github.com/Akarsh-Hegde/pes_asic_class/assets/79705687/3f083b80-9067-439f-a7fe-d443e4b0882a)

## Integer number Representation (n-bit)
- Range of Unsigned numbers : [0, (2^n)-1 ]
* Range of signed numbes : Positive : [0 , 2^(n-1)-1]
                         Negative : [-1 to 2^(n-1)]

## 2. create a C program that shows the maximum and minimum values of 64bit unsigend and signed numbers

```
sign_unsign.c
```
![PIC 6](https://github.com/Akarsh-Hegde/pes_asic_class/assets/79705687/334c011a-b2ac-4ec6-9b87-2786abd445c5)



[Back to COURSE](https://github.com/yagnavivek/PES_ASIC_CLASS/tree/main#course)

</details>
<details>
<summary>DAY 2 : Introduction to ABI and Basic Verification Flow </summary>
<br>

## BASICS :

Instructions that act on signed or unsigned integers are called Base Integer Instructions
There are 47 Base Integer Instructions present in RISC-V ISA

### Types of Instruction based on encoding format

1. **R-Type (Register-Type):**
   - These instructions operate on registers and have a fixed format for their operands.
   - Examples: ADD, SUB, AND, OR, XOR, SLL, SRL, SRA, SLT, SLTU

2. **I-Type (Immediate-Type):**
   - These instructions have an immediate operand and one register operand.
   - Examples: ADDI, SLTI, SLTIU, XORI, ORI, ANDI, SLLI, SRLI, SRAI, LB, LH, LW, LBU, LHU, JALR

3. **S-Type (Store-Type):**
   - These instructions are used for storing values from registers to memory.
   - Examples: SB, SH, SW

4. **B-Type (Branch-Type):**
   - These instructions perform conditional branching based on comparisons.
   - Examples: BEQ, BNE, BLT, BGE, BLTU, BGEU

5. **U-Type (Upper Immediate-Type):**
   - These instructions have a larger immediate field for encoding larger constants.
   - Examples: LUI, AUIPC

6. **J-Type (Jump-Type):**
   - These instructions are used for unconditional jumps and function calls.
   - Examples: JAL

<img width="1000" height="420" alt="image" src="https://github.com/yagnavivek/PES_ASIC_CLASS/assets/93475824/e69043fb-684e-42eb-9e21-fd51943c1ec1">

**[number]** represents number of bits occupied by that field

1. **Opcode [7] :** The opcode is a field within a machine language instruction that indicates the operation to be performed by the instruction. It defines the type of operation, such as arithmetic, logic, memory access, or control flow. Opcodes are used by the CPU to determine how to execute the instruction.

2. **rd (Destination Register) [5]:** The "rd" field represents the destination register in an assembly language instruction. It indicates the register where the result of the operation will be stored. After executing the instruction, the computed value will be placed in this register.

3. **rs1 (Source Register 1) [5]:** The "rs1" field represents the first source register in an assembly language instruction. It indicates the register that holds the value used in the operation. For instructions that involve two operands, "rs1" typically corresponds to the first operand.

4. **rs2 (Source Register 2) [5]:** The "rs2" field represents the second source register in an assembly language instruction. It indicates the register that holds the value used in the operation. For instructions that involve three operands, "rs2" typically corresponds to the second operand.

5. **func7 and func3 (Function Fields)[7] [3]:** These fields further refine the operation specified by the opcode. The "func7" field is used to distinguish different variations of instructions within the same opcode category. The "func3" field is used to specify a more specific operation within the opcode category. Together, these fields allow for a finer level of instruction differentiation.

6. **imm (Immediate Value):** The "imm" field represents an immediate value that is part of the instruction. Immediate values are constants that are embedded within the instruction itself. They can be used for various purposes, such as specifying offsets, constants, or small data values directly within the instruction.
7. 
#### ABI : Application Binary Interface

The instructions generated by compiler using a target ISA can be accessed by OS and User directly
- The parts of ISA accessible to User : User ISA
- The parts of ISA accessible to OS : system ISA
The access is done using Sysytem calls with the help of ABI

==> If we want to access hardware resources of processor, it has to be done via registers using ABI(names)

### ABI Names : 
- ABI names for registers serve as a standardized way to designate the purpose and usage of specific registers within a software ecosystem. These names play a critical role in maintaining compatibility, optimizing code generation, and facilitating communication between different software components.

<img width="1000" height="600" src="https://github.com/yagnavivek/PES_ASIC_CLASS/assets/93475824/27d13974-1b70-4207-a2fb-05b232027323">

#### Data can be stored in register by 2 methods
1. Directly store in registers
2. Store into registers from memory

To store 64 bits of data from mem to reg, we use 8*8bit stores ie., m[0],m[1]......m[7]. 

- ___RISC-V uses Little Endian format to store the data ie., Least significant Byte is stored in m[0]___

## Simulate a C program using ABI function call (using registers) and execute 

The required program files are under day 2 folder
![PIC 1](https://github.com/Akarsh-Hegde/pes_asic_class/assets/79705687/fa0fa76c-457b-493c-bb94-f21364ef15ff)



![PIC 2](https://github.com/Akarsh-Hegde/pes_asic_class/assets/79705687/1d18c07a-aff8-4b37-96ee-bda450c47df7)




Here we can observe that at 5th line, inorder to comute the result ,its going to the "load"  function

### Further we will see how to run a C program on on RISC-V CPU

[Back to COURSE](https://github.com/yagnavivek/PES_ASIC_CLASS/tree/main#course)
</details>


<details>
<summary>DAY-3 : Digital Logic with TL-Verilog in Makerchip IDE</summary>
<br>

#### Task-1 : Logic Gates
![image](https://github.com/Pavan2280/RISC-V/assets/131603225/24cfbcd8-3ff2-4cae-b4fa-488e5c77af5c)


#### Task-2 : Lab - Makerchip platfrom
To use Makerchip IDE, you need to visit makerchip website at [http://makerchip.com/](http://makerchip.com/) and launch Makerchip IDE
To access a specific example, please follow these steps:
1) **Navigate to the 'Learn' section**
2) **Click on 'Examples'**
3) **Load 'FGPA Multiplier' Example**

![WhatsApp Image 2023-10-18 at 8 44 59 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/8ed04f45-bc20-40f3-9729-c3bc419ec817)


#### Task-3 : Lab - Combitional logic
**A) Inverter**
1) **Click on 'Examples'**
2) **Load Default Template**
3) **Go to editor and make changes(On line 16,in place of `//...` type `$out = ! $in;`)**
4) **Compile(Ctrl+E)**

![WhatsApp Image 2023-10-18 at 8 17 36 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/b2353d68-1ba7-4558-be46-b42d03cf2302)


**B) XOR Gate**
1) **Click on 'Examples'**
2) **Load Default Template**
3) **Go to editor and make changes**
```
$out = ! $in;
$out1 = ($in1 ^ $in2);
```
4) **Compile(Ctrl+E)**
![WhatsApp Image 2023-10-18 at 8 17 45 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/f36e1a79-25e2-4d7c-8abc-01b2fd1852a9)


**C) Vectors**
1) **Click on 'Examples'**
2) **Load Default Template**
3) **Go to editor and make changes**
```
$out[4:0] = $in1[3:0] + $in2[3:0];
```
4) **Compile(Ctrl+E)**
![WhatsApp Image 2023-10-18 at 8 17 45 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/db1768f5-1517-4fe3-aebe-43f4f290467b)


**D) Mux without vector & with vectors**
1) **Click on 'Examples'**
2) **Load Default Template**
   
3a) **Go to editor and make changes**
```
$out = $sel ? $in1 : $in2;
```
4a) **Compile(Ctrl+E)**
![WhatsApp Image 2023-10-18 at 8 17 39 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/c31f8930-d1fb-4b71-94e1-2ffff9057694)


3b) **Go to editor and make changes**
```
$out[7:0] = $sel ? $in1[7:0] : $in2[7:0];
```
4b) **Compile(Ctrl+E)**
![WhatsApp Image 2023-10-18 at 10 34 04 AM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/079c9960-2164-4ad2-a19a-19d2f5df5d68)


**E) Simple Calculator**
1) **Click on 'Examples'**
2) **Load Default Template**   
3) **Go to editor and make changes**
```
$val1[31:0] = $rand1[3:0]; 
$val2[31:0] = $rand2[3:0];
$sum[31:0] = $val1 + $val2;
$diff[31:0] = $val1 - $val2;
$prod[31:0] = $val1 * $val2;
$qut[31:0] = $val1 / $val2;
$out[31:0] = $op[1] ? ($op[0] ? $qut: $prod): ($op [0] ? $diff: $sum);
```
4) **Compile(Ctrl+E)**
![WhatsApp Image 2023-10-18 at 8 17 43 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/a32b48d5-9ecf-4288-920a-16d929c7e961)


#### Task-4 : Sequential logic 

![image](https://github.com/Pavan2280/RISC-V/assets/131603225/0d548af2-e42f-48fd-9a33-fe47df3775fb)

**A) Fibonacci series**
1) **Click on 'Examples'**
2) **Load Default Template**   
3) **Go to editor and make changes**
```
$num[31:0] = $reset ? 1 : (>>1$num + >>2$num); 
```
4) **Compile(Ctrl+E)**
![WhatsApp Image 2023-10-18 at 8 17 43 PM (1)](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/f06107da-5332-498f-b300-6655bc77b80c)


**B) Up-Counter**
1) **Click on 'Examples'**
2) **Load Default Template**   
3) **Go to editor and make changes**
```
$num[2:0] = $reset ? 0 : (>>1$num + 1); 
```
4) **Compile(Ctrl+E)**
![WhatsApp Image 2023-10-18 at 8 17 44 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/d8f32832-b0dc-47c8-a94c-db161982d160)


**C) Sequential Calculator**
1) **Click on 'Examples'**
2) **Load Default Template**   
3) **Go to editor and make changes**
```
$val1[31:0] = (>>1$out); 
$val2[31:0] = $rand2[3:0]; 
$sum[31:0] = $val1 + $val2;
$diff[31:0] = $val1 - $val2;
$prod[31:0] = $val1 * $val2;
$qut[31:0] = $val1 / $val2;
$out[31:0] = $op[1] ? ($op[0] ? $qut: $prod): ($op [0] ? $diff: $sum); 
```
4) **Compile(Ctrl+E)**
![WhatsApp Image 2023-10-18 at 8 17 44 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/5d229da9-353b-429f-84bf-76320a9c7921)


#### Task-5 : Pipelined logic
**A) A simple pipeline through Pythagorean example**
1) **Click on 'Examples'**
2) **Load Default Template**   
3) **Go to editor and make changes**
```
`include "sqrt32.v"
|calc
      @1
         $aa_sq[31:0] = $aa[3:0] * $aa;
         $bb_sq[31:0] = $bb[3:0] * $bb;
      @2
         $cc_sq[31:0] = $aa_sq + $bb_sq;
      @3
         $cc[31:0] = sqrt($cc_sq);
```
4) **Compile(Ctrl+E)**

![WhatsApp Image 2023-10-18 at 8 17 45 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/f7f5aeb0-ae68-4481-8e16-405595b757a0)

**B) Pipeline Implementation**
1) **Click on 'Examples'**
2) **Load Default Template**
3) **Go to editor and make changes**
```
|comp
      @1
         $err1 = $bad_input || $illegal_op;
      @2
         $err2 = $err1 || $over_flow;
      @3
         $err3 = $div_by_zero || $err2;
```
4) **Compile(Ctrl+E)**
![WhatsApp Image 2023-10-18 at 8 17 45 PM (1)](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/3fb19d5e-2e2d-4ca3-b3ab-b2fe158ef9e1)



#### Task-6 : Validity
+ Easier debug
+ Cleaner design
+ Better error checking
+ Automated clock gating

**A) 2 cycle calculator with validity**
1) **Click on 'Examples'**
2) **Load Default Template**
3) **Go to editor and make changes**
```
|calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out [31:0];
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1;
         $valid_or_reset = $valid || $reset;
         
      ?$valid_or_reset
      @1
         $sum [31:0] = $val1 + $val2;
         $diff[31:0] = $val1 - $val2;
         $prod[31:0] = $val1 * $val2;
         $qut [31:0] = $val1 / $val2;
         
      @2
         $out [31:0] = $reset ? 32'b0 :
                      ($op[1:0] == 2'b00) ? $sum :
                      ($op[1:0] == 2'b01) ? $diff :
                      ($op[1:0] == 2'b10) ? $prod :
                                              $qut ;
```
4) **Compile(Ctrl+E)**
![WhatsApp Image 2023-10-17 at 8 03 29 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/9a2fd5e5-f13a-480d-bff0-217be8975d6e)


**B) Distance Calculator**
1) **Click on 'Examples'**
2) **Load Default Template**
3) **Go to editor and make changes**
```
|calc
      @1
         $reset = *reset;
         
      ?$valid
         @1
            $aa_sq[31:0] = $aa[3:0] * $aa;
            $bb_sq[31:0] = $bb[3:0] * $bb;;
         @2
            $cc_sq[31:0] = $aa_sq + $bb_sq;;
         @3
            $cc[31:0] = sqrt($cc_sq);
      @4
         $total_distance[63:0] =
            $reset ? 0 :
            $valid ? >>1$total_distance + $cc :
                     >>1$total_distance;
```
4) **Compile(Ctrl+E)**
![WhatsApp Image 2023-10-17 at 8 00 47 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/a9d7e7ca-47ab-44af-a390-29a0ce2d8f2b)


**A) Calulator Memory**
1) **Click on 'Examples'**
2) **Load Default Template**
3) **Go to editor and make changes**
```
|calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out [31:0];
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1;
         $valid_or_reset = $valid || $reset;
         
      ?$valid_or_reset
      @1
         $sum [31:0] = $val1 + $val2;
         $diff[31:0] = $val1 - $val2;
         $prod[31:0] = $val1 * $val2;
         $qut [31:0] = $val1 / $val2;
         
      @2
         $mem[31:0] = $reset ? 32'b0 :
                      ($op[2:0] == 3'b101) ? $val1 : >>2$mem ;
         
         $out [31:0] = $reset ? 32'b0 :
                      ($op[2:0] == 3'b000) ? $sum :
                      ($op[2:0] == 3'b001) ? $diff :
                      ($op[2:0] == 3'b010) ? $prod :
                      ($op[2:0] == 3'b011) ? $qut  :
                      ($op[2:0] == 3'b100) ? >>2$mem : >>2$out ;
```
4) **Compile(Ctrl+E)**

![WhatsApp Image 2023-10-17 at 7 53 11 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/e580592a-bcd8-4584-9976-65866ce5ff40)


[Back to Top](#top)

</details>

<details>
<summary>DAY 4 : Basic RISC-V CPU Micro Architecture</summary>
<br>

# RISC-V Architecture Block Diagram

![image](https://github.com/Pavan2280/RISC-V/assets/131603225/1695d5f6-eab9-4279-9419-b2817800b002)

## Overview
This RISC-V Architecture Block Diagram illustrates the fundamental components and their interactions within a computer system based on the RISC-V instruction set architecture. RISC-V is a modular and customizable architecture, providing a versatile framework for designing processors tailored to specific application requirements.

## Components
1. **CPU (Central Processing Unit)**
   - *Description*: The CPU serves as the core of the RISC-V processor, responsible for executing instructions. It includes multiple stages:
     - Instruction Fetch (IF): Fetches instructions from memory.
     - Instruction Decode (ID): Decodes the fetched instructions.
     - Execution (EX): Performs arithmetic and logic operations.
     - Memory (MEM): Manages data memory access.
     - Write Back (WB): Writes results back to registers.

2. **Instruction Memory**
   - *Description*: This memory component stores the program's instructions that the CPU fetches and executes. It's essential for the program's proper execution.

3. **Data Memory**
   - *Description*: Data Memory stores data used by the CPU during program execution. It is crucial for data manipulation and storage.

4. **Registers**
   - *Description*: Registers are a set of general-purpose storage units used for temporary data storage and manipulation by the CPU. They play a pivotal role in instruction execution.

5. **Control Unit**
   - *Description*: The Control Unit manages control signals and coordinates the activities of the CPU's components, ensuring the proper execution of instructions.

6. **ALU (Arithmetic Logic Unit)**
   - *Description*: The ALU performs arithmetic and logic operations as directed by the CPU's instructions. It is the computational workhorse of the processor.

7. **Instruction Decoder**
   - *Description*: The Instruction Decoder interprets and decodes instructions fetched from memory. It translates instructions into actions for the CPU to execute.

8. **Cache Memory**
   - *Description*: Cache Memory provides fast access to frequently used instructions and data. It helps improve the system's overall performance by reducing memory access times.

9. **Bus Interface**
   - *Description*: The Bus Interface facilitates data transfer between the CPU, memory, and peripherals. It ensures efficient communication within the system.

10. **Peripherals**
    - *Description*: Peripherals are external devices such as input/output controllers, timers, and more. They connect to the CPU, enhancing the system's functionality by allowing interaction with the outside world.

For the consecutive labs, we will use the "RISC-V lab starting point code" from https://github.com/stevehoover/RISC-V_MYTH_Workshop.

Use the following links : [Link for the starter code](https://myth.makerchip.com/sandbox?code_url=https:%2F%2Fraw.githubusercontent.com%2Fstevehoover%2FRISC-V_MYTH_Workshop%2Fmaster%2Frisc-v_shell.tlv#)


#### Task-1 : Program Counter
```
|cpu
    @0
       $reset = *reset;
       $pc[31:0] = >>1$reset ? 32'b0 :
                               >>1$pc + 32'd4;
```
![WhatsApp Image 2023-10-17 at 8 09 50 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/07c5b7ba-b37e-4165-a1de-ca0cc8749427)


#### Task-2 : Instruction Fetch
```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                                 >>1$pc + 32'd4;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1   
         $instr[31:0] = $imem_rd_data[31:0];
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
```
![WhatsApp Image 2023-10-17 at 8 22 11 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/4fd05a5b-0732-4fa7-9579-d0cddfbb6fdc)


#### Task-3 : Instruction Decode
```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                                 >>1$pc + 32'd4;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1   
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $rs1[4:0] = $instr[19:15];
         $rs2[4:0] = $instr[24:20];
         $rd[4:0] = $instr[11:7];
         $opcode[6:0] = $instr[6:0];
         $funct7[6:0] = $instr[31:25];
         $funct3[2:0] = $instr[14:12];
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
```
![WhatsApp Image 2023-10-17 at 8 19 58 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/d99a232b-729f-402c-95c5-9c4066ef57dd)


#### Task-4 : Instruction Decode with validity
```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                                 >>1$pc + 32'd4;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1   
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
```
![WhatsApp Image 2023-10-17 at 8 24 19 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/b24aba04-6752-4635-8221-d7d8b2aed46a)


#### Task-5 : Individual Instruction decode
```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                                 >>1$pc + 32'd4;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1   
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];         
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
```
![WhatsApp Image 2023-10-17 at 8 27 08 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/3edfa692-7fb5-48a7-96d6-d4aa4eb945a5)


#### Task-6 : Register File Read
```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                                 >>1$pc + 32'd4;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1   
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $rf_wr_en = 1'b0;
         $rf_wr_index[4:0] = 5'b0;
         $rf_wr_data[31:0] = 32'b0;
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @1
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @1
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>1$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>1$value;
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+cpu_viz(@4)    // For visualisation
```
![WhatsApp Image 2023-10-18 at 8 17 46 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/a974dc97-bab4-4db3-b7d4-0b490f5c068e)


#### Task-7 : ALU
```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                                 >>1$pc + 32'd4;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1   
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $rf_wr_en = 1'b0;
         $rf_wr_index[4:0] = 5'b0;
         $rf_wr_data[31:0] = 32'b0;
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @1
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @1
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>1$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>1$value;
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                                    32'bx;
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+cpu_viz(@4)    // For visualisation
```
![WhatsApp Image 2023-10-18 at 8 17 46 PM (1)](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/c0a94657-1bd2-42e0-a799-e7fbc08be18c)


#### Task-8 : Register File Write
```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                                 >>1$pc + 32'd4;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1   
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @1
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @1
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>1$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>1$value;
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                                    32'bx;
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+cpu_viz(@4)    // For visualisation
```
![WhatsApp Image 2023-10-18 at 8 17 45 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/1c7d1cc5-22bc-4e94-8899-a4e672ad71ff)


#### Task-9 : Branch Instructions
```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                         >>1$taken_br ? >>1$br_tgt_pc :
                                 >>1$pc + 32'd4;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1   
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @1
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @1
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>1$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>1$value;
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                                    32'bx;
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                          $is_bne ? ($src1_value != $src2_value) :
                          $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bltu ? ($src1_value < $src2_value) :
                          $is_bgeu ? ($src1_value >= $src2_value) :
                                          1'b0;
         $br_tgt_pc[31:0] = $pc + $imm;
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+cpu_viz(@4)    // For visualisation
```
![WhatsApp Image 2023-10-18 at 8 17 45 PM (1)](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/fd22708d-6df0-4868-a198-cac88aff0357)


#### Task-10 : Testbench to check functionality
```

   |cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                         >>1$taken_br ? >>1$br_tgt_pc :
                                 >>1$pc + 32'd4;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1   
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @1
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @1
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>1$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>1$value;
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                                    32'bx;
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                          $is_bne ? ($src1_value != $src2_value) :
                          $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bltu ? ($src1_value < $src2_value) :
                          $is_bgeu ? ($src1_value >= $src2_value) :
                                          1'b0;
         $br_tgt_pc[31:0] = $pc + $imm;
```
![WhatsApp Image 2023-10-18 at 8 17 45 PM (2)](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/54729577-0f46-4ea0-a684-69cfed18006f)

[Back to Top](#top)

</details>
<details>
<summary>DAY 5 : Complete Pipelined RISC-V Micro Architecture </summary>
<br>

# Complete Pipelined RISC-V CPU micro-architecture
## Pipelining the CPU

#### Introduction To Control Flow Hazard And Read After Write Hazard

- waterfall logic diagram
![image](https://github.com/JBavitha/bavitha_riscv/assets/142578450/1f700718-1d07-4e2e-8553-777458d4488d)


- Control Flow Hazard:
  - Control flow hazards, also known as control hazards, occur when the normal sequential flow of instructions is disrupted due to conditional branch instructions.
  - These hazards can lead to incorrect program execution and reduced performance.
  - There are three main types of control flow hazards:
    - Branch Hazards: These occur when a branch instruction (e.g., a conditional jump) is encountered, and the processor is unsure about the branch target. The processor may need to stall the pipeline or make speculative predictions to mitigate the impact of these hazards.
    - Jump Hazards: Jump instructions that cause a change in the program counter can also create hazards, as they affect the instruction fetch stage and can lead to pipeline bubbles (stalls).
    - Return Hazards: Similar to jump hazards, but related to function calls and returns. These can disrupt the flow of instructions.

- Read-After-Write Hazard (RAW Hazard):
  - Read-after-write hazards, often referred to as data hazards, occur when an instruction depends on the result of a previous instruction that has not yet completed.
  - There are three main types of read-after-write hazards:
    - True Data Dependency: This occurs when an instruction (e.g., a load or ALU operation) depends on the result of a previous instruction. The hazard arises because the dependent instruction cannot proceed until the producing instruction has written its result.
    - Anti-Data Dependency: This occurs when a previous instruction depends on a result produced by a later instruction. This situation can cause issues when instructions are executed out of order.
    - Output Data Dependency (Write-After-Read): This happens when two instructions need to read from the same register, and the second instruction wants to read before the first instruction writes to the register. This can lead to incorrect results if not managed properly.


#### Labs

- 1.Lab to create 3-Cycle Valid Signal

```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                         >>1$taken_br ? >>1$br_tgt_pc :
                                 >>1$pc + 32'd4;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $start = >>1$reset && !$reset;
         $valid = $reset ? 1'b0 : $start ? 1'b1 :
                                          >>3$valid;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1   
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @1
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @1
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>1$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>1$value;
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                                    32'bx;
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                          $is_bne ? ($src1_value != $src2_value) :
                          $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bltu ? ($src1_value < $src2_value) :
                          $is_bgeu ? ($src1_value >= $src2_value) :
                                          1'b0;
         $br_tgt_pc[31:0] = $pc + $imm;
         *passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9);
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+cpu_viz(@4)    // For visualisation

```

![WhatsApp Image 2023-10-18 at 3 11 29 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/d1385a7e-8ad2-4943-8ef0-2cbc83c25fd6)


- 2.To modify 3-cycle RISC-V to Distribute Logic

```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                         >>3$valid_taken_br ? >>3$br_tgt_pc :
                                 >>3$inc_pc;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $start = >>1$reset && !$reset;
         $valid = $reset ? 1'b0 : $start ? 1'b1 :
                                          >>3$valid;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1 
         $inc_pc[31:0] = $pc + 32'd4;
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @3
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @2
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>2$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>2$value;
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         $br_tgt_pc[31:0] = $pc + $imm;
      @3
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                                    32'bx;
         $rf_wr_en = $rd_valid && $rd != 5'b0 && $valid;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                          $is_bne ? ($src1_value != $src2_value) :
                          $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bltu ? ($src1_value < $src2_value) :
                          $is_bgeu ? ($src1_value >= $src2_value) :
                                          1'b0;
         $valid_taken_br = $valid && $taken_br;
         *passed = |cpu/xreg[10]>>3$value == (1+2+3+4+5+6+7+8+9);
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+cpu_viz(@4)    // For visualisation

```

![WhatsApp Image 2023-10-18 at 3 21 13 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/5ef99b25-5d57-4132-ab9d-f6772daa1df3)


## Solutions to Pipeline Hazards

#### 1.Register File Bypass

```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                         >>3$valid_taken_br ? >>3$br_tgt_pc :
                                 >>3$inc_pc;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $start = >>1$reset && !$reset;
         $valid = $reset ? 1'b0 : $start ? 1'b1 :
                                          >>3$valid;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1 
         $inc_pc[31:0] = $pc + 32'd4;
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @3
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @2
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>2$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>2$value;
         $src1_value[31:0] = (>>1$rf_wr_index == $rf_rd_index1) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data1;
         $src2_value[31:0] = (>>1$rf_wr_index == $rf_rd_index2) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data2;
         $br_tgt_pc[31:0] = $pc + $imm;
      @3
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                                    32'bx;
         $rf_wr_en = $rd_valid && $rd != 5'b0 && $valid;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                          $is_bne ? ($src1_value != $src2_value) :
                          $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bltu ? ($src1_value < $src2_value) :
                          $is_bgeu ? ($src1_value >= $src2_value) :
                                          1'b0;
         $valid_taken_br = $valid && $taken_br;
         *passed = |cpu/xreg[10]>>3$value == (1+2+3+4+5+6+7+8+9);
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+cpu_viz(@4)    // For visualisation

```

![WhatsApp Image 2023-10-18 at 8 21 15 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/e78157bf-84e4-42e2-8026-6efc021bc47f)


  
#### 2.To correct Branch Target Path

```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                         >>3$valid_taken_br ? >>3$br_tgt_pc :
                                 >>1$inc_pc;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1 
         $inc_pc[31:0] = $pc + 32'd4;
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @3
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @2
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>2$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>2$value;
         $src1_value[31:0] = (>>1$rf_wr_index == $rf_rd_index1) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data1;
         $src2_value[31:0] = (>>1$rf_wr_index == $rf_rd_index2) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data2;
         $br_tgt_pc[31:0] = $pc + $imm;
      @3
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                                    32'bx;
         $rf_wr_en = $rd_valid && $rd != 5'b0 && $valid;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                          $is_bne ? ($src1_value != $src2_value) :
                          $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bltu ? ($src1_value < $src2_value) :
                          $is_bgeu ? ($src1_value >= $src2_value) :
                                          1'b0;
         $valid_taken_br = $valid && $taken_br;
         $valid = !(>>1$valid_taken_br || >>2$valid_taken_br);
         *passed = |cpu/xreg[10]>>3$value == (1+2+3+4+5+6+7+8+9);
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+cpu_viz(@4)    // For visualisation
```

![WhatsApp Image 2023-10-18 at 8 23 03 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/c46f73c1-1983-4029-81e8-5c929bfb74c5)

  
#### 3.Complete Instruction Decode

```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                         >>3$valid_taken_br ? >>3$br_tgt_pc :
                                 >>1$inc_pc;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1 
         $inc_pc[31:0] = $pc + 32'd4;
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_load = $opcode == 7'b0000011;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_slti = $dec_bits ==? 11'bx_010_0010011;
         $is_slt = $dec_bits ==? 11'b0_010_0110011;
         $is_slli = $dec_bits ==? 11'b0_001_0010011;
         $is_sll = $dec_bits ==? 11'b0_001_0110011;
         $is_sh = $dec_bits ==? 11'bx_001_0100011;
         $is_sb = $dec_bits ==? 11'bx_000_0100011;
         $is_ori = $dec_bits ==? 11'bx_110_0010011;
         $is_or = $dec_bits ==? 11'b0_110_0110011;
         $is_lui = $dec_bits ==? 11'bx_xxx_0110111;
         $is_jalr = $dec_bits ==? 11'bx_000_1100111;
         $is_jal = $dec_bits ==? 11'bx_xxx_1101111;
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_andi = $dec_bits ==? 11'bx_111_0010011;
         $is_and = $dec_bits ==? 11'b0_111_0110011;
         $is_xori = $dec_bits ==? 11'bx_100_0010011;
         $is_xor = $dec_bits ==? 11'b0_100_0110011;
         $is_sw = $dec_bits ==? 11'bx_010_0100011;
         $is_sub = $dec_bits ==? 11'b1_000_0110011;
         $is_srli = $dec_bits ==? 11'b0_101_0010011;
         $is_srl = $dec_bits ==? 11'b0_101_0110011;
         $is_srai = $dec_bits ==? 11'b1_101_0010011;
         $is_sra = $dec_bits ==? 11'b1_101_0110011;
         $is_sltu = $dec_bits ==? 11'b0_011_0110011;
         $is_sltui = $dec_bits ==? 11'bx_011_0010011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @3
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @2
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>2$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>2$value;
         $src1_value[31:0] = (>>1$rf_wr_index == $rf_rd_index1) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data1;
         $src2_value[31:0] = (>>1$rf_wr_index == $rf_rd_index2) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data2;
         $br_tgt_pc[31:0] = $pc + $imm;
      @3
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                                    32'bx;
         $rf_wr_en = $rd_valid && $rd != 5'b0 && $valid;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                          $is_bne ? ($src1_value != $src2_value) :
                          $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bltu ? ($src1_value < $src2_value) :
                          $is_bgeu ? ($src1_value >= $src2_value) :
                                          1'b0;
         $valid_taken_br = $valid && $taken_br;
         $valid = !(>>1$valid_taken_br || >>2$valid_taken_br);
         *passed = |cpu/xreg[10]>>3$value == (1+2+3+4+5+6+7+8+9);
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+cpu_viz(@4)    // For visualisation

```
![WhatsApp Image 2023-10-18 at 8 24 41 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/7c29dc4b-6206-485b-a9f7-75a3a6317041)



#### 4.Complete ALU

```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                         >>3$valid_taken_br ? >>3$br_tgt_pc :
                                 >>1$inc_pc;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1 
         $inc_pc[31:0] = $pc + 32'd4;
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_load = $opcode == 7'b0000011;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_slti = $dec_bits ==? 11'bx_010_0010011;
         $is_slt = $dec_bits ==? 11'b0_010_0110011;
         $is_slli = $dec_bits ==? 11'b0_001_0010011;
         $is_sll = $dec_bits ==? 11'b0_001_0110011;
         $is_sh = $dec_bits ==? 11'bx_001_0100011;
         $is_sb = $dec_bits ==? 11'bx_000_0100011;
         $is_ori = $dec_bits ==? 11'bx_110_0010011;
         $is_or = $dec_bits ==? 11'b0_110_0110011;
         $is_lui = $dec_bits ==? 11'bx_xxx_0110111;
         $is_jalr = $dec_bits ==? 11'bx_000_1100111;
         $is_jal = $dec_bits ==? 11'bx_xxx_1101111;
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_andi = $dec_bits ==? 11'bx_111_0010011;
         $is_and = $dec_bits ==? 11'b0_111_0110011;
         $is_xori = $dec_bits ==? 11'bx_100_0010011;
         $is_xor = $dec_bits ==? 11'b0_100_0110011;
         $is_sw = $dec_bits ==? 11'bx_010_0100011;
         $is_sub = $dec_bits ==? 11'b1_000_0110011;
         $is_srli = $dec_bits ==? 11'b0_101_0010011;
         $is_srl = $dec_bits ==? 11'b0_101_0110011;
         $is_srai = $dec_bits ==? 11'b1_101_0010011;
         $is_sra = $dec_bits ==? 11'b1_101_0110011;
         $is_sltu = $dec_bits ==? 11'b0_011_0110011;
         $is_sltui = $dec_bits ==? 11'bx_011_0010011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @3
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @2
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>2$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>2$value;
         $src1_value[31:0] = (>>1$rf_wr_index == $rf_rd_index1) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data1;
         $src2_value[31:0] = (>>1$rf_wr_index == $rf_rd_index2) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data2;
         $br_tgt_pc[31:0] = $pc + $imm;
      @3
         $sltiu_rslt[31:0] = $src1_value < $imm;
         $sltu_rslt[31:0] = $src1_value < $src2_value;
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                          $is_andi ? $src1_value & $imm :
                          $is_ori ? $src1_value | $imm :
                          $is_xori ? $src1_value ^ $src2_value :
                          $is_slli ? $src1_value << $imm[5:0] :
                          $is_srli ? $src1_value >> $imm[5:0] :
                          $is_and ? $src1_value & $src2_value :
                          $is_or ? $src1_value | $src2_value : 
                          $is_xor ? $src1_value ^ $src2_value :
                          $is_sub ? $src1_value - $src2_value :
                          $is_sll ? $src1_value << $src2_value[4:0] :
                          $is_srl ? $src1_value >> $src2_value[4:0] :
                          $is_srai ? {{32{$src1_value[31]}}, $src1_value} >> $imm[4:0] :
                          $is_slt ? ($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]} :
                          $is_slti ? ($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]} :
                          $is_sra ? {{31{$src1_value[31]}}, $src1_value} >> $src2_value[4:0] :
                          $is_lui ? {$imm[31:12], 12'b0} :
                          $is_auipc ? $pc + $imm :
                          $is_jal ? $pc + 4 :
                          $is_jalr ? $pc + 4 :
                          32'bx;
         $rf_wr_en = $rd_valid && $rd != 5'b0 && $valid;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                          $is_bne ? ($src1_value != $src2_value) :
                          $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bltu ? ($src1_value < $src2_value) :
                          $is_bgeu ? ($src1_value >= $src2_value) :
                                          1'b0;
         $valid_taken_br = $valid && $taken_br;
         $valid = !(>>1$valid_taken_br || >>2$valid_taken_br);
         *passed = |cpu/xreg[10]>>3$value == (1+2+3+4+5+6+7+8+9);
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+cpu_viz(@4)    // For visualisation

```
![WhatsApp Image 2023-10-18 at 8 26 43 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/c73ece7d-e565-45d5-b347-94812a6b603e)



## Load/Store Instructions and Completing RISC-V CPU

#### 1.Lab to Redirect Loads

```
 |cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                         >>3$valid_taken_br ? >>3$br_tgt_pc :
                         >>3$valid_load ? >>3$inc_pc :
                                 >>1$inc_pc;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1 
         $inc_pc[31:0] = $pc + 32'd4;
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_load = $opcode == 7'b0000011;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_slti = $dec_bits ==? 11'bx_010_0010011;
         $is_slt = $dec_bits ==? 11'b0_010_0110011;
         $is_slli = $dec_bits ==? 11'b0_001_0010011;
         $is_sll = $dec_bits ==? 11'b0_001_0110011;
         $is_sh = $dec_bits ==? 11'bx_001_0100011;
         $is_sb = $dec_bits ==? 11'bx_000_0100011;
         $is_ori = $dec_bits ==? 11'bx_110_0010011;
         $is_or = $dec_bits ==? 11'b0_110_0110011;
         $is_lui = $dec_bits ==? 11'bx_xxx_0110111;
         $is_jalr = $dec_bits ==? 11'bx_000_1100111;
         $is_jal = $dec_bits ==? 11'bx_xxx_1101111;
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_andi = $dec_bits ==? 11'bx_111_0010011;
         $is_and = $dec_bits ==? 11'b0_111_0110011;
         $is_xori = $dec_bits ==? 11'bx_100_0010011;
         $is_xor = $dec_bits ==? 11'b0_100_0110011;
         $is_sw = $dec_bits ==? 11'bx_010_0100011;
         $is_sub = $dec_bits ==? 11'b1_000_0110011;
         $is_srli = $dec_bits ==? 11'b0_101_0010011;
         $is_srl = $dec_bits ==? 11'b0_101_0110011;
         $is_srai = $dec_bits ==? 11'b1_101_0010011;
         $is_sra = $dec_bits ==? 11'b1_101_0110011;
         $is_sltu = $dec_bits ==? 11'b0_011_0110011;
         $is_sltui = $dec_bits ==? 11'bx_011_0010011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @3
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @2
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>2$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>2$value;
         $src1_value[31:0] = (>>1$rf_wr_index == $rf_rd_index1) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data1;
         $src2_value[31:0] = (>>1$rf_wr_index == $rf_rd_index2) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data2;
         $br_tgt_pc[31:0] = $pc + $imm;
      @3
         $sltiu_rslt[31:0] = $src1_value < $imm;
         $sltu_rslt[31:0] = $src1_value < $src2_value;
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                          $is_andi ? $src1_value & $imm :
                          $is_ori ? $src1_value | $imm :
                          $is_xori ? $src1_value ^ $src2_value :
                          $is_slli ? $src1_value << $imm[5:0] :
                          $is_srli ? $src1_value >> $imm[5:0] :
                          $is_and ? $src1_value & $src2_value :
                          $is_or ? $src1_value | $src2_value : 
                          $is_xor ? $src1_value ^ $src2_value :
                          $is_sub ? $src1_value - $src2_value :
                          $is_sll ? $src1_value << $src2_value[4:0] :
                          $is_srl ? $src1_value >> $src2_value[4:0] :
                          $is_srai ? {{32{$src1_value[31]}}, $src1_value} >> $imm[4:0] :
                          $is_slt ? ($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]} :
                          $is_slti ? ($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]} :
                          $is_sra ? {{31{$src1_value[31]}}, $src1_value} >> $src2_value[4:0] :
                          $is_lui ? {$imm[31:12], 12'b0} :
                          $is_auipc ? $pc + $imm :
                          $is_jal ? $pc + 4 :
                          $is_jalr ? $pc + 4 :
                          32'bx;
         $rf_wr_en = $rd_valid && $rd != 5'b0 && $valid;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                          $is_bne ? ($src1_value != $src2_value) :
                          $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bltu ? ($src1_value < $src2_value) :
                          $is_bgeu ? ($src1_value >= $src2_value) :
                                          1'b0;
         $valid_taken_br = $valid && $taken_br;
         $valid_load = $valid && $is_load;
         $valid = !(>>1$valid_taken_br || >>2$valid_taken_br || >>1$valid_load || >>2$valid_load);
         *passed = |cpu/xreg[10]>>3$value == (1+2+3+4+5+6+7+8+9);
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+cpu_viz(@4)    // For visualisation
  
```
![WhatsApp Image 2023-10-18 at 8 01 04 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/c3ed5bc1-07b6-4f94-b083-133d4b849949)


#### 2.Lab to Data From Memory to Register File

```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                         >>3$valid_taken_br ? >>3$br_tgt_pc :
                         >>3$valid_load ? >>3$inc_pc :
                                 >>1$inc_pc;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1 
         $inc_pc[31:0] = $pc + 32'd4;
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_load = $opcode == 7'b0000011;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_slti = $dec_bits ==? 11'bx_010_0010011;
         $is_slt = $dec_bits ==? 11'b0_010_0110011;
         $is_slli = $dec_bits ==? 11'b0_001_0010011;
         $is_sll = $dec_bits ==? 11'b0_001_0110011;
         $is_sh = $dec_bits ==? 11'bx_001_0100011;
         $is_sb = $dec_bits ==? 11'bx_000_0100011;
         $is_ori = $dec_bits ==? 11'bx_110_0010011;
         $is_or = $dec_bits ==? 11'b0_110_0110011;
         $is_lui = $dec_bits ==? 11'bx_xxx_0110111;
         $is_jalr = $dec_bits ==? 11'bx_000_1100111;
         $is_jal = $dec_bits ==? 11'bx_xxx_1101111;
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_andi = $dec_bits ==? 11'bx_111_0010011;
         $is_and = $dec_bits ==? 11'b0_111_0110011;
         $is_xori = $dec_bits ==? 11'bx_100_0010011;
         $is_xor = $dec_bits ==? 11'b0_100_0110011;
         $is_sw = $dec_bits ==? 11'bx_010_0100011;
         $is_sub = $dec_bits ==? 11'b1_000_0110011;
         $is_srli = $dec_bits ==? 11'b0_101_0010011;
         $is_srl = $dec_bits ==? 11'b0_101_0110011;
         $is_srai = $dec_bits ==? 11'b1_101_0010011;
         $is_sra = $dec_bits ==? 11'b1_101_0110011;
         $is_sltu = $dec_bits ==? 11'b0_011_0110011;
         $is_sltui = $dec_bits ==? 11'bx_011_0010011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @3
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @2
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>2$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>2$value;
         $src1_value[31:0] = (>>1$rf_wr_index == $rf_rd_index1) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data1;
         $src2_value[31:0] = (>>1$rf_wr_index == $rf_rd_index2) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data2;
         $br_tgt_pc[31:0] = $pc + $imm;
      @3
         $sltiu_rslt[31:0] = $src1_value < $imm;
         $sltu_rslt[31:0] = $src1_value < $src2_value;
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                          $is_andi ? $src1_value & $imm :
                          $is_ori ? $src1_value | $imm :
                          $is_xori ? $src1_value ^ $src2_value :
                          $is_slli ? $src1_value << $imm[5:0] :
                          $is_srli ? $src1_value >> $imm[5:0] :
                          $is_and ? $src1_value & $src2_value :
                          $is_or ? $src1_value | $src2_value : 
                          $is_xor ? $src1_value ^ $src2_value :
                          $is_sub ? $src1_value - $src2_value :
                          $is_sll ? $src1_value << $src2_value[4:0] :
                          $is_srl ? $src1_value >> $src2_value[4:0] :
                          $is_srai ? {{32{$src1_value[31]}}, $src1_value} >> $imm[4:0] :
                          $is_slt ? ($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]} :
                          $is_slti ? ($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]} :
                          $is_sra ? {{31{$src1_value[31]}}, $src1_value} >> $src2_value[4:0] :
                          $is_lui ? {$imm[31:12], 12'b0} :
                          $is_auipc ? $pc + $imm :
                          $is_jal ? $pc + 4 :
                          $is_jalr ? $pc + 4 :
                          32'bx;
         $rf_wr_en = ($rd_valid && $rd != 5'b0 && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = >>2$valid_load ? >>2$rd : $rd;
         $rf_wr_data[31:0] = >>2$valid_load ? >>2$ld_data : $result;
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                          $is_bne ? ($src1_value != $src2_value) :
                          $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bltu ? ($src1_value < $src2_value) :
                          $is_bgeu ? ($src1_value >= $src2_value) :
                                          1'b0;
         $valid_taken_br = $valid && $taken_br;
         $valid_load = $valid && $is_load;
         $valid = !(>>1$valid_taken_br || >>2$valid_taken_br || >>1$valid_load || >>2$valid_load);
         *passed = |cpu/xreg[10]>>3$value == (1+2+3+4+5+6+7+8+9);
      @5
         $ld_data = 'x;
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+cpu_viz(@4)    // For visualisation

```

![WhatsApp Image 2023-10-18 at 8 02 30 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/ba45d2cb-80f3-4c48-8fd9-a6fb9db046ea)



#### 3.Lab to Instantiate Data Memory to CPU

```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                         >>3$valid_taken_br ? >>3$br_tgt_pc :
                         >>3$valid_load ? >>3$inc_pc :
                                 >>1$inc_pc;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1 
         $inc_pc[31:0] = $pc + 32'd4;
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_load = $opcode == 7'b0000011;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_slti = $dec_bits ==? 11'bx_010_0010011;
         $is_slt = $dec_bits ==? 11'b0_010_0110011;
         $is_slli = $dec_bits ==? 11'b0_001_0010011;
         $is_sll = $dec_bits ==? 11'b0_001_0110011;
         $is_sh = $dec_bits ==? 11'bx_001_0100011;
         $is_sb = $dec_bits ==? 11'bx_000_0100011;
         $is_ori = $dec_bits ==? 11'bx_110_0010011;
         $is_or = $dec_bits ==? 11'b0_110_0110011;
         $is_lui = $dec_bits ==? 11'bx_xxx_0110111;
         $is_jalr = $dec_bits ==? 11'bx_000_1100111;
         $is_jal = $dec_bits ==? 11'bx_xxx_1101111;
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_andi = $dec_bits ==? 11'bx_111_0010011;
         $is_and = $dec_bits ==? 11'b0_111_0110011;
         $is_xori = $dec_bits ==? 11'bx_100_0010011;
         $is_xor = $dec_bits ==? 11'b0_100_0110011;
         $is_sw = $dec_bits ==? 11'bx_010_0100011;
         $is_sub = $dec_bits ==? 11'b1_000_0110011;
         $is_srli = $dec_bits ==? 11'b0_101_0010011;
         $is_srl = $dec_bits ==? 11'b0_101_0110011;
         $is_srai = $dec_bits ==? 11'b1_101_0010011;
         $is_sra = $dec_bits ==? 11'b1_101_0110011;
         $is_sltu = $dec_bits ==? 11'b0_011_0110011;
         $is_sltui = $dec_bits ==? 11'bx_011_0010011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @3
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @2
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>2$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>2$value;
         $src1_value[31:0] = (>>1$rf_wr_index == $rf_rd_index1) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data1;
         $src2_value[31:0] = (>>1$rf_wr_index == $rf_rd_index2) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data2;
         $br_tgt_pc[31:0] = $pc + $imm;
      @3
         $sltiu_rslt[31:0] = $src1_value < $imm;
         $sltu_rslt[31:0] = $src1_value < $src2_value;
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                          $is_andi ? $src1_value & $imm :
                          $is_ori ? $src1_value | $imm :
                          $is_xori ? $src1_value ^ $src2_value :
                          $is_slli ? $src1_value << $imm[5:0] :
                          $is_srli ? $src1_value >> $imm[5:0] :
                          $is_and ? $src1_value & $src2_value :
                          $is_or ? $src1_value | $src2_value : 
                          $is_xor ? $src1_value ^ $src2_value :
                          $is_sub ? $src1_value - $src2_value :
                          $is_sll ? $src1_value << $src2_value[4:0] :
                          $is_srl ? $src1_value >> $src2_value[4:0] :
                          $is_srai ? {{32{$src1_value[31]}}, $src1_value} >> $imm[4:0] :
                          $is_slt ? ($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]} :
                          $is_slti ? ($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]} :
                          $is_sra ? {{31{$src1_value[31]}}, $src1_value} >> $src2_value[4:0] :
                          $is_lui ? {$imm[31:12], 12'b0} :
                          $is_auipc ? $pc + $imm :
                          $is_jal ? $pc + 4 :
                          $is_jalr ? $pc + 4 :
                          32'bx;
         $rf_wr_en = ($rd_valid && $rd != 5'b0 && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = >>2$valid_load ? >>2$rd : $rd;
         $rf_wr_data[31:0] = >>2$valid_load ? >>2$ld_data : $result;
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                          $is_bne ? ($src1_value != $src2_value) :
                          $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bltu ? ($src1_value < $src2_value) :
                          $is_bgeu ? ($src1_value >= $src2_value) :
                                          1'b0;
         $valid_taken_br = $valid && $taken_br;
         $valid_load = $valid && $is_load;
         $valid = !(>>1$valid_taken_br || >>2$valid_taken_br || >>1$valid_load || >>2$valid_load);
         *passed = |cpu/xreg[10]>>3$value == (1+2+3+4+5+6+7+8+9);
      /dmem[15:0]
         @4
            $wr = |cpu$dmem_wr_en && (|cpu$dmem_addr == #dmem);
            $value[31:0] = |cpu$reset ? #dmem : $wr ? |cpu$dmem_wr_data :
                                                      $RETAIN;
      @4
         $dmem_wr_data[31:0] = $src2_value;
         $dmem_wr_en = $is_s_instr && $valid;
         $dmem_rd_en = $is_load;
         $dmem_addr[3:0] = $result[5:2];
         ?$dmem_rd_en
            $dmem_rd_data[31:0] = /dmem[$dmem_addr]>>1$value;
      @5
         $ld_data[31:0] = $dmem_rd_data;
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
      m4+cpu_viz(@4)    // For visualisation

```
![WhatsApp Image 2023-10-18 at 8 04 25 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/f105d7a4-78b8-4401-8f21-46cd3c405087)

  
#### 4.Lab for Jump Instructions


```
|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 32'b0 :
                         >>3$valid_taken_br ? >>3$br_tgt_pc :
                         >>3$valid_load ? >>3$inc_pc :
                         >>3$valid_jump && >>3$is_jal ? >>3$br_tgt_pc :
                         >>3$valid_jump && >>3$is_jalr ? >>3$jalr_tgt_pc :
                                 >>1$inc_pc;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
      /imem[7:0]
         @1
            $instr[31:0] = *instrs\[#imem\];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1 
         $inc_pc[31:0] = $pc + 32'd4;
         $instr[31:0] = $imem_rd_data[31:0];
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                          $instr[6:2] ==? 5'b011x0 ||
                          $instr[6:2] ==? 5'b10100;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                            $instr[6:2] ==? 5'b001x0 ||
                            $instr[6:2] ==? 5'b11001;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_load = $opcode == 7'b0000011;
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $funct7_valid = $is_r_instr;
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                         $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                         $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                         $is_u_instr ? {$instr[31:12], 12'b0} :
                         $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                     32'b0;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         $opcode[6:0] = $instr[6:0];
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_slti = $dec_bits ==? 11'bx_010_0010011;
         $is_slt = $dec_bits ==? 11'b0_010_0110011;
         $is_slli = $dec_bits ==? 11'b0_001_0010011;
         $is_sll = $dec_bits ==? 11'b0_001_0110011;
         $is_sh = $dec_bits ==? 11'bx_001_0100011;
         $is_sb = $dec_bits ==? 11'bx_000_0100011;
         $is_ori = $dec_bits ==? 11'bx_110_0010011;
         $is_or = $dec_bits ==? 11'b0_110_0110011;
         $is_lui = $dec_bits ==? 11'bx_xxx_0110111;
         $is_jalr = $dec_bits ==? 11'bx_000_1100111;
         $is_jal = $dec_bits ==? 11'bx_xxx_1101111;
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_andi = $dec_bits ==? 11'bx_111_0010011;
         $is_and = $dec_bits ==? 11'b0_111_0110011;
         $is_xori = $dec_bits ==? 11'bx_100_0010011;
         $is_xor = $dec_bits ==? 11'b0_100_0110011;
         $is_sw = $dec_bits ==? 11'bx_010_0100011;
         $is_sub = $dec_bits ==? 11'b1_000_0110011;
         $is_srli = $dec_bits ==? 11'b0_101_0010011;
         $is_srl = $dec_bits ==? 11'b0_101_0110011;
         $is_srai = $dec_bits ==? 11'b1_101_0010011;
         $is_sra = $dec_bits ==? 11'b1_101_0110011;
         $is_sltu = $dec_bits ==? 11'b0_011_0110011;
         $is_sltui = $dec_bits ==? 11'bx_011_0010011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
      /xreg[31:0]
         @3
            $wr = |cpu$rf_wr_en && (|cpu$rf_wr_index != 5'b0) && (|cpu$rf_wr_index == #xreg);
            $value[31:0] = |cpu$reset ? #xreg : 
                                  $wr ? |cpu$rf_wr_data : $RETAIN;
      @2
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         ?$rf_rd_en1
            $rf_rd_data1[31:0] = /xreg[$rf_rd_index1]>>2$value;
         ?$rf_rd_en2
            $rf_rd_data2[31:0] = /xreg[$rf_rd_index2]>>2$value;
         $src1_value[31:0] = (>>1$rf_wr_index == $rf_rd_index1) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data1;
         $src2_value[31:0] = (>>1$rf_wr_index == $rf_rd_index2) && >>1$rf_wr_en ? >>1$result :
                                                                                  $rf_rd_data2;
         $jalr_tgt_pc[31:0] = $src1_value + $imm;
         $br_tgt_pc[31:0] = $pc + $imm;
      @3
         $sltiu_rslt[31:0] = $src1_value < $imm;
         $sltu_rslt[31:0] = $src1_value < $src2_value;
         $result[31:0] = $is_addi ? $src1_value + $imm :
                          $is_add ? $src1_value + $src2_value :
                          $is_andi ? $src1_value & $imm :
                          $is_ori ? $src1_value | $imm :
                          $is_xori ? $src1_value ^ $src2_value :
                          $is_slli ? $src1_value << $imm[5:0] :
                          $is_srli ? $src1_value >> $imm[5:0] :
                          $is_and ? $src1_value & $src2_value :
                          $is_or ? $src1_value | $src2_value : 
                          $is_xor ? $src1_value ^ $src2_value :
                          $is_sub ? $src1_value - $src2_value :
                          $is_sll ? $src1_value << $src2_value[4:0] :
                          $is_srl ? $src1_value >> $src2_value[4:0] :
                          $is_srai ? {{32{$src1_value[31]}}, $src1_value} >> $imm[4:0] :
                          $is_slt ? ($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]} :
                          $is_slti ? ($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]} :
                          $is_sra ? {{31{$src1_value[31]}}, $src1_value} >> $src2_value[4:0] :
                          $is_lui ? {$imm[31:12], 12'b0} :
                          $is_auipc ? $pc + $imm :
                          $is_jal ? $pc + 4 :
                          $is_jalr ? $pc + 4 :
                          32'bx;
         $rf_wr_en = ($rd_valid && $rd != 5'b0 && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = >>2$valid_load ? >>2$rd : $rd;
         $rf_wr_data[31:0] = >>2$valid_load ? >>2$ld_data : $result;
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                          $is_bne ? ($src1_value != $src2_value) :
                          $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                          $is_bltu ? ($src1_value < $src2_value) :
                          $is_bgeu ? ($src1_value >= $src2_value) :
                                          1'b0;
         $valid_taken_br = $valid && $taken_br;
         $is_jump = $is_jal || $is_jalr;
         $valid_load = $valid && $is_load;
         $valid_jump = $is_jump && $valid;
         $valid = !(>>1$valid_taken_br || >>2$valid_taken_br || >>1$valid_load || >>2$valid_load || >>1$valid_jump || >>2$valid_jump);
         *passed = |cpu/xreg[10]>>3$value == (1+2+3+4+5+6+7+8+9);
      /dmem[15:0]
         @4
            $wr = |cpu$dmem_wr_en && (|cpu$dmem_addr == #dmem);
            $value[31:0] = |cpu$reset ? #dmem : $wr ? |cpu$dmem_wr_data :
                                                      $RETAIN;
      @4
         $dmem_wr_data[31:0] = $src2_value;
         $dmem_wr_en = $is_s_instr && $valid;
         $dmem_rd_en = $is_load;
         $dmem_addr[3:0] = $result[5:2];
         ?$dmem_rd_en
            $dmem_rd_data[31:0] = /dmem[$dmem_addr]>>1$value;
      @5
         $ld_data[31:0] = $dmem_rd_data;
|cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
      m4+cpu_viz(@4)    // For visualisation

```
![WhatsApp Image 2023-10-18 at 8 17 36 PM](https://github.com/Akarsh-Hegde/RISC-V/assets/79705687/0664d318-867e-4b1d-b8ee-5483385ec2ee)


[Back to Top](#top)

</details>
