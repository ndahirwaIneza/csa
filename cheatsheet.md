# 🧠 CSA Master Cheat Sheet
### Computer Systems Architecture — Exam & Lab Reference
*For quick revision, interview prep, and lab assignment help*

---

## 📋 TABLE OF CONTENTS
1. [Number Systems & Conversions](#1-number-systems--conversions)
2. [Binary Arithmetic](#2-binary-arithmetic)
3. [IEEE-754 Floating Point](#3-ieee-754-floating-point)
4. [Character Encoding](#4-character-encoding)
5. [Boolean Algebra](#5-boolean-algebra)
6. [Logic Gate Quick Reference](#6-logic-gate-quick-reference)
7. [Karnaugh Maps (K-Maps)](#7-karnaugh-maps-k-maps)
8. [Digital Circuits](#8-digital-circuits)
9. [MASM32 Assembly Reference](#9-masm32-assembly-reference)
10. [Memory Hierarchy](#10-memory-hierarchy)
11. [Cache Memory](#11-cache-memory)
12. [Virtual Memory](#12-virtual-memory)
13. [Processor Concepts](#13-processor-concepts)
14. [Interface Quick Reference](#14-interface-quick-reference)

---

## 1. NUMBER SYSTEMS & CONVERSIONS

### Base Reference Table
| Base | Name        | Digits Used         | Prefix |
|------|-------------|---------------------|--------|
| 2    | Binary      | 0, 1                | 0b     |
| 8    | Octal       | 0–7                 | 0o     |
| 10   | Decimal     | 0–9                 | (none) |
| 16   | Hexadecimal | 0–9, A–F            | 0x     |

### Hex Digit Reference
| Dec | Hex | Bin  | Dec | Hex | Bin  |
|-----|-----|------|-----|-----|------|
| 0   | 0   | 0000 | 8   | 8   | 1000 |
| 1   | 1   | 0001 | 9   | 9   | 1001 |
| 2   | 2   | 0010 | 10  | A   | 1010 |
| 3   | 3   | 0011 | 11  | B   | 1011 |
| 4   | 4   | 0100 | 12  | C   | 1100 |
| 5   | 5   | 0101 | 13  | D   | 1101 |
| 6   | 6   | 0110 | 14  | E   | 1110 |
| 7   | 7   | 0111 | 15  | F   | 1111 |

---

### ✅ CONVERSION PROCEDURES

#### Decimal Integer → Binary
**Method: Repeated Division by 2, read remainders BOTTOM to TOP**
```
Example: 45 → Binary

45 ÷ 2 = 22  R 1  ← last (MSB)
22 ÷ 2 = 11  R 0
11 ÷ 2 =  5  R 1
 5 ÷ 2 =  2  R 1
 2 ÷ 2 =  1  R 0
 1 ÷ 2 =  0  R 1  ← first (LSB)

Read remainders BOTTOM→TOP: 101101
Answer: 45₁₀ = 101101₂
```

#### Decimal Fraction → Binary
**Method: Repeated Multiplication by 2, read integer parts TOP to BOTTOM**
```
Example: 0.625 → Binary

0.625 × 2 = 1.25  → take 1
0.25  × 2 = 0.50  → take 0
0.50  × 2 = 1.00  → take 1  (stop when .0)

Read TOP→BOTTOM: .101
Answer: 0.625₁₀ = 0.101₂
```

> ⚠️ Stop when fractional part = 0.0, or after required precision bits.

#### Binary → Hexadecimal
**Method: Group binary into groups of 4 from RIGHT, convert each group**
```
Example: 10110111₂ → Hex

  1011  0111
    B     7

Answer: 0xB7
```

#### Binary → Octal
**Method: Group binary into groups of 3 from RIGHT**
```
Example: 10110111₂ → Octal

  10   110   111
   2     6     7

Answer: 267₈
```

#### Hexadecimal → Decimal
**Method: Positional value — multiply each digit by 16ⁿ**
```
Example: 0x2AF → Decimal

2 × 16² + A(10) × 16¹ + F(15) × 16⁰
= 2 × 256 + 10 × 16 + 15 × 1
= 512 + 160 + 15
= 687

Answer: 0x2AF = 687₁₀
```

#### Decimal → Hexadecimal
**Method: Repeated Division by 16, read remainders BOTTOM to TOP**
```
Example: 255 → Hex

255 ÷ 16 = 15  R 15 (F)  ← last
 15 ÷ 16 =  0  R 15 (F)  ← first

Answer: 0xFF
```

---

## 2. BINARY ARITHMETIC

### Addition Rules
```
0 + 0 = 0
0 + 1 = 1
1 + 0 = 1
1 + 1 = 10  (0, carry 1)
1 + 1 + 1 = 11  (1, carry 1)
```

### 2's Complement (Signed Negative Numbers)
**To negate a number:**
```
Step 1: Flip all bits (1's complement)
Step 2: Add 1

Example: -13 in 8-bit 2's complement
  13 = 00001101
  Flip: 11110010
  +1:   11110011
  -13 = 11110011₂
```

**Range of n-bit 2's complement:** `−2^(n−1)` to `+2^(n−1) − 1`
- 8-bit: −128 to +127
- 16-bit: −32768 to +32767

### Binary Subtraction (using 2's complement)
```
A − B = A + (2's complement of B)

Example: 15 − 6
  15 = 00001111
  −6 = 11111010  (2's complement of 6)
  Sum: 100001001
  Drop carry: 00001001 = 9 ✓
```

### Overflow Detection
- **Signed overflow:** occurs if two numbers with same sign produce a result with opposite sign
- Check: `Carry into MSB ≠ Carry out of MSB`

---

## 3. IEEE-754 FLOATING POINT

### Single Precision (32-bit) Layout
```
| S |  Exponent (8 bits)  |     Mantissa (23 bits)      |
|31 | 30 ............. 23 | 22 ........................ 0|
```

### Double Precision (64-bit) Layout
```
| S |  Exponent (11 bits)  |       Mantissa (52 bits)       |
|63 | 62 ........... 52    | 51 .......................... 0|
```

### Key Formulas
| Item            | Single Precision      | Double Precision      |
|-----------------|-----------------------|-----------------------|
| Sign bits       | 1                     | 1                     |
| Exponent bits   | 8                     | 11                    |
| Mantissa bits   | 23                    | 52                    |
| Bias            | **127**               | **1023**              |
| Stored exponent | E_real + Bias         | E_real + Bias         |

### Conversion: Decimal → IEEE-754 (Single)
```
Example: Convert -6.75 to IEEE-754 single precision

Step 1: Sign bit → negative = 1

Step 2: Convert 6.75 to binary
  6   = 110
  .75 = .11
  6.75 = 110.11

Step 3: Normalize → 1.1011 × 2²
  (move decimal until 1.something)

Step 4: Exponent = 2 + 127 (bias) = 129 = 10000001

Step 5: Mantissa = 1011 000...0 (23 bits, drop leading 1)

Result: 1 | 10000001 | 10110000000000000000000
```

### Special Values (Single Precision)
| Value     | Sign | Exponent   | Mantissa |
|-----------|------|------------|----------|
| +0        | 0    | 00000000   | all 0s   |
| −0        | 1    | 00000000   | all 0s   |
| +∞        | 0    | 11111111   | all 0s   |
| −∞        | 1    | 11111111   | all 0s   |
| NaN       | any  | 11111111   | ≠ 0      |
| Denormal  | any  | 00000000   | ≠ 0      |

---

## 4. CHARACTER ENCODING

### ASCII Key Reference
| Char  | Decimal | Hex  | Char | Decimal | Hex  |
|-------|---------|------|------|---------|------|
| NUL   | 0       | 0x00 | A    | 65      | 0x41 |
| Space | 32      | 0x20 | Z    | 90      | 0x5A |
| 0     | 48      | 0x30 | a    | 97      | 0x61 |
| 9     | 57      | 0x39 | z    | 122     | 0x7A |

> 💡 Memory trick: '0' = 48, 'A' = 65, 'a' = 97. Add offset for other chars.

- **ASCII**: 7-bit, 128 characters (extended: 8-bit, 256 chars)
- **EBCDIC**: IBM mainframe encoding, 8-bit, different layout from ASCII

---

## 5. BOOLEAN ALGEBRA

### Basic Operations
| Operation | Symbol  | Meaning        | Example        |
|-----------|---------|----------------|----------------|
| AND       | · or ∧  | both must be 1 | A · B          |
| OR        | + or ∨  | at least one 1 | A + B          |
| NOT       | ‾ or ¬  | flip the bit   | Ā              |
| XOR       | ⊕       | different = 1  | A ⊕ B          |
| NAND      | ↑       | NOT AND        | A · B with bar |
| NOR       | ↓       | NOT OR         | A + B with bar |

### Fundamental Axioms
```
Identity:       A + 0 = A        A · 1 = A
Null:           A + 1 = 1        A · 0 = 0
Idempotent:     A + A = A        A · A = A
Complement:     A + Ā = 1        A · Ā = 0
Involution:     Ā̄ = A
```

### Key Theorems
```
Commutative:    A + B = B + A       A · B = B · A
Associative:    (A+B)+C = A+(B+C)   (AB)C = A(BC)
Distributive:   A(B+C) = AB + AC    A+BC = (A+B)(A+C)
Absorption:     A + AB = A          A(A+B) = A
```

### De Morgan's Laws ⭐
```
¬(A · B) = ¬A + ¬B    "NAND = break bar, flip dot to plus"
¬(A + B) = ¬A · ¬B    "NOR  = break bar, flip plus to dot"
```
> 💡 Memory trick: **"Break the bar, change the operation"**

---

## 6. LOGIC GATE QUICK REFERENCE

### Truth Tables
```
AND           OR            NOT           XOR
A B | Out     A B | Out     A | Out       A B | Out
0 0 |  0      0 0 |  0      0 |  1        0 0 |  0
0 1 |  0      0 1 |  1      1 |  0        0 1 |  1
1 0 |  0      1 0 |  1                    1 0 |  1
1 1 |  1      1 1 |  1                    1 1 |  0

NAND          NOR
A B | Out     A B | Out
0 0 |  1      0 0 |  1
0 1 |  1      0 1 |  0
1 0 |  1      1 0 |  0
1 1 |  0      1 1 |  0
```

---

## 7. KARNAUGH MAPS (K-MAPS)

### 2-Variable K-Map Layout
```
     B=0   B=1
A=0 |  0  |  1  |   ← minterms 0, 1
A=1 |  2  |  3  |   ← minterms 2, 3
```

### 3-Variable K-Map Layout
```
        AB
   | 00 | 01 | 11 | 10 |
C=0| m0 | m1 | m3 | m2 |
C=1| m4 | m5 | m7 | m6 |
```
> ⚠️ Columns follow **Gray code order**: 00, 01, 11, 10 (NOT binary order!)

### 4-Variable K-Map Layout
```
        CD
AB  | 00 | 01 | 11 | 10 |
 00 | m0 | m1 | m3 | m2 |
 01 | m4 | m5 | m7 | m6 |
 11 |m12 |m13 |m15 |m14 |
 10 | m8 | m9 |m11 |m10 |
```

### K-Map Grouping Rules
1. Group **1s only** (for SOP) — group **0s** for POS
2. Groups must be **powers of 2**: 1, 2, 4, 8, 16
3. Groups can **wrap around** edges (map is a torus)
4. **Larger groups = simpler expression**
5. Each group eliminates one variable per doubling in size
6. Every 1 must be covered; groups may overlap

### SOP (Sum of Products) Steps
```
1. Fill in truth table
2. Place 1s in K-map at correct positions
3. Circle largest possible groups of 1s
4. For each group, identify variables that DON'T change
5. Write AND term for each group (using stable variables)
6. OR all terms together
```

---

## 8. DIGITAL CIRCUITS

### Flip-Flops Summary
| Type  | Inputs      | Behavior                         |
|-------|-------------|----------------------------------|
| SR    | S, R        | Set/Reset; undefined if S=R=1    |
| D     | D           | Output = D at clock edge         |
| JK    | J, K        | Toggle when J=K=1; no undefined  |
| T     | T           | Toggles output when T=1          |

### Multiplexer (MUX)
- **N selection lines → 2ᴺ inputs → 1 output**
- Acts like a data selector switch
- Formula: `Y = S·B + S̄·A` (2:1 MUX)

### Demultiplexer (DEMUX)
- **1 input → 2ᴺ outputs** (controlled by N selection lines)
- Opposite of MUX

### Register
- N flip-flops storing N bits
- Types: parallel-in/parallel-out (PIPO), serial-in/serial-out (SISO), shift register

### Counter
- Counts clock pulses
- **n-bit counter**: counts 0 to 2ⁿ−1
- Types: up counter, down counter, modulo-N counter

### Tri-State Gate
- Three states: 0, 1, **High-impedance (Z)**
- Used on buses so multiple devices can share lines without conflict

---

## 9. MASM32 ASSEMBLY REFERENCE

### General-Purpose Registers (32-bit)
| Register | Full Name            | Common Use                          |
|----------|----------------------|-------------------------------------|
| EAX      | Accumulator          | Arithmetic, return values           |
| EBX      | Base                 | Base address for memory             |
| ECX      | Counter              | Loop counter                        |
| EDX      | Data                 | I/O, overflow in mul/div            |
| ESI      | Source Index         | Source in string operations         |
| EDI      | Destination Index    | Destination in string operations    |
| ESP      | Stack Pointer        | Points to top of stack              |
| EBP      | Base Pointer         | Stack frame base                    |

> 💡 EAX sub-registers: EAX (32-bit) → AX (16-bit) → AH/AL (8-bit each)

### Flags Register (EFLAGS)
| Flag | Name           | Set When...                          |
|------|----------------|--------------------------------------|
| ZF   | Zero Flag      | Result = 0                           |
| SF   | Sign Flag      | Result is negative (MSB = 1)         |
| CF   | Carry Flag     | Unsigned overflow / borrow           |
| OF   | Overflow Flag  | Signed overflow                      |
| PF   | Parity Flag    | Result has even number of 1 bits     |

### Common Instructions
```asm
; DATA MOVEMENT
MOV  dst, src      ; dst = src
PUSH src           ; push to stack (ESP -= 4)
POP  dst           ; pop from stack (ESP += 4)
LEA  dst, [addr]   ; load effective address

; ARITHMETIC
ADD  dst, src      ; dst = dst + src
SUB  dst, src      ; dst = dst - src
INC  dst           ; dst = dst + 1
DEC  dst           ; dst = dst - 1
MUL  src           ; EDX:EAX = EAX * src (unsigned)
IMUL src           ; signed multiply
DIV  src           ; EAX = EDX:EAX / src, EDX = remainder
NEG  dst           ; dst = -dst (2's complement)

; LOGICAL
AND  dst, src      ; bitwise AND
OR   dst, src      ; bitwise OR
XOR  dst, src      ; bitwise XOR
NOT  dst           ; bitwise NOT
SHL  dst, n        ; shift left by n (multiply by 2ⁿ)
SHR  dst, n        ; shift right by n (divide by 2ⁿ)

; COMPARISON & JUMPS
CMP  a, b          ; sets flags based on a - b (doesn't store)
JMP  label         ; unconditional jump
JE   label         ; jump if equal (ZF=1)
JNE  label         ; jump if not equal (ZF=0)
JL   label         ; jump if less (signed)
JG   label         ; jump if greater (signed)
JB   label         ; jump if below (unsigned)
JA   label         ; jump if above (unsigned)

; PROCEDURES
CALL proc          ; call procedure (push return addr)
RET                ; return from procedure
INT  0x80          ; software interrupt (system call)
```

### Skeleton Program
```asm
.386                        ; target 80386 processor
.model flat, stdcall        ; flat memory model
.stack 4096                 ; 4KB stack

.data                       ; initialized data segment
  msg DB "Hello!", 0        ; null-terminated string
  num DD 42                 ; 32-bit integer

.data?                      ; uninitialized data
  buffer DB 256 DUP(?)      ; 256-byte buffer

.code                       ; code segment
main PROC
    ; your code here
    mov eax, 0              ; exit code 0
    ret
main ENDP
END main
```

### I/O with Win32 API (MASM32)
```asm
; Print string using StdOut
invoke StdOut, addr msg

; Read input using StdIn
invoke StdIn, addr buffer, 256

; Exit program
invoke ExitProcess, 0
```

---

## 10. MEMORY HIERARCHY

### Hierarchy (Fastest → Slowest, Smallest → Largest)
```
CPU Registers      → ~1 cycle,   bytes
L1 Cache           → 1–4 cycles,  32–64 KB
L2 Cache           → 10–20 cycles, 256 KB–4 MB
L3 Cache           → 30–60 cycles, 4–64 MB
RAM (DRAM)         → 100+ cycles, GB range
SSD Storage        → microseconds, TB range
HDD Storage        → milliseconds, TB range
```

### Memory Types
| Type  | Volatile? | Writable? | Use                            |
|-------|-----------|-----------|--------------------------------|
| SRAM  | Yes       | Yes       | Cache (fast, expensive)        |
| DRAM  | Yes       | Yes       | Main RAM (slow, cheap)         |
| ROM   | No        | No        | Firmware (permanent)           |
| PROM  | No        | Once      | Programmable once              |
| EPROM | No        | UV erase  | Re-programmable via UV light   |
| EEPROM| No        | Yes       | Flash memory, BIOS             |

### DMA (Direct Memory Access)
- Allows devices to transfer data to/from RAM **without CPU involvement**
- Steps: `Device requests DMA → CPU grants bus → DMA transfers → CPU gets interrupt`
- Frees CPU for other tasks during large data transfers

---

## 11. CACHE MEMORY

### Cache Mapping Types
| Type           | How it works                                | Pro           | Con                |
|----------------|---------------------------------------------|---------------|--------------------|
| Direct-mapped  | Each RAM block → exactly 1 cache line       | Simple, fast  | High collision rate|
| Fully associative| Any RAM block → any cache line            | Flexible      | Slow, expensive    |
| Set-associative| N-way: block maps to 1 of N lines in a set  | Balance both  | Moderate complexity|

### Cache Formulas
```
Hit rate (h) = Cache hits / Total accesses

Effective Access Time (EAT) = h × T_cache + (1-h) × T_main

Cache Size = Number of lines × Line size (bytes)

Number of Index bits = log₂(number of sets)
Number of Offset bits = log₂(block size in bytes)
Number of Tag bits = Address bits - Index bits - Offset bits
```

### Write Policies
| Policy       | On Cache Hit         | On Cache Miss              |
|--------------|----------------------|----------------------------|
| Write-through| Write cache + RAM    | Write to RAM directly      |
| Write-back   | Write cache only     | Write to RAM when evicted  |

---

## 12. VIRTUAL MEMORY

### Key Concepts
- **Virtual address**: address used by program (can exceed physical RAM)
- **Physical address**: actual location in RAM
- **Page**: fixed-size block of virtual memory (typically 4 KB)
- **Frame**: fixed-size block of physical memory (same size as page)
- **Page Table**: maps virtual pages → physical frames

### Address Translation Formula
```
Physical Address = Frame Number × Page Size + Offset

Virtual Address → split into:
  [Page Number | Offset]
  - Page Number: upper bits (index into page table)
  - Offset: lower bits (position within page)

Number of Offset bits = log₂(page size)
Number of Page bits = virtual address bits - offset bits
```

### Segmentation vs Paging
| Feature        | Paging                | Segmentation              |
|----------------|-----------------------|---------------------------|
| Division       | Fixed size (pages)    | Variable size (segments)  |
| Fragmentation  | Internal (within page)| External (between segments)|
| Managed by     | OS + hardware (MMU)   | OS + hardware (MMU)       |

### Fragmentation Types
- **Internal fragmentation**: wasted space *inside* an allocated block (paging)
- **External fragmentation**: wasted space *between* allocated blocks (segmentation)

---

## 13. PROCESSOR CONCEPTS

### Pipeline Stages (Classic 5-stage)
```
IF → ID → EX → MEM → WB
Fetch | Decode | Execute | Memory | Write Back
```
- Pipeline allows **N instructions in flight simultaneously**
- Throughput: 1 instruction per cycle (ideal)
- Hazards: data hazard, control hazard, structural hazard

### CISC vs RISC vs VLIW
| Feature         | CISC                  | RISC                  | VLIW                  |
|-----------------|-----------------------|-----------------------|-----------------------|
| Instructions    | Many, complex         | Few, simple           | Long, parallel        |
| Example         | Intel x86             | ARM, MIPS             | Itanium               |
| Cycles/instr    | Variable              | 1 (mostly)            | Variable              |
| Registers       | Few                   | Many                  | Many                  |
| Complexity      | Hardware              | Software (compiler)   | Compiler              |

### Amdahl's Law (Parallel Speedup)
```
Speedup = 1 / ((1 - P) + P/N)

Where:
  P = fraction of program that can be parallelized
  N = number of processors
  (1-P) = serial portion that cannot be parallelized
```

---

## 14. INTERFACE QUICK REFERENCE

| Interface  | Type    | Speed               | Use Case                     |
|------------|---------|---------------------|------------------------------|
| RS-232C    | Serial  | ~115 kbps           | Legacy serial comms          |
| RS-485     | Serial  | Up to 10 Mbps       | Industrial, multi-drop       |
| USB 3.0    | Serial  | 5 Gbps              | Peripherals                  |
| SATA III   | Serial  | 6 Gbps              | Hard drives/SSDs             |
| PCI-Express| Serial  | ~16 GB/s (×16)      | GPUs, NVMe SSDs              |
| Ethernet   | Serial  | 1 Gbps / 10 Gbps    | Networking                   |
| I2C        | Serial  | 100–400 kbps        | Sensors, microcontrollers    |
| SCSI       | Parallel| Up to 640 MB/s      | Server storage (legacy)      |

---

## ⚡ EXAM STRATEGY QUICK TIPS

1. **Number conversions**: Always show your work step-by-step — partial credit exists
2. **IEEE-754**: Memorize bias=127 (single) and bias=1023 (double); always normalize to 1.xxx
3. **De Morgan's**: "Break the bar, swap the operator" — works every time
4. **K-maps**: Gray code order! 00→01→11→10. Wrap-around groups are valid.
5. **2's complement**: Flip + Add 1. Range: −2^(n-1) to +2^(n-1)−1
6. **Cache EAT**: Always use the formula; don't guess
7. **Assembly flags**: CMP doesn't store result — it just sets flags for jumps
8. **MUX**: 2ⁿ inputs needs n select lines — count the select lines!
9. **Pipeline**: 5 stages; speedup limited by the slowest stage
10. **Virtual Memory**: Offset bits = log₂(page size) — always start here

---

*Last updated: 2025 | Version 1.0 | CSA Master Cheat Sheet*