# Computer Systems Architecture — Complete Coursebook
### Bachelor of Arts in Computer Science | Exam-Ready Edition

> **How to use this book:** Each chapter is self-contained. Read the Hook, skim What You'll Learn, then work through Core Concepts. Before any lab, jump to the Lab Prep Exercises. Before any exam, use Appendix B. Every chapter can be absorbed in 30–45 minutes.

---

## Changelog

| Version | Date | Change |
|---------|------|--------|
| 1.0 | 2025 | Initial generation — all 11 chapters + appendices |

---

## Quick-Reference Index

- [Chapter 1 — History & Architecture Foundations](#chapter-1-history--architecture-foundations)
- [Chapter 2 — Number Systems](#chapter-2-number-systems)
- [Chapter 3 — Floating Point Numbers](#chapter-3-floating-point-numbers)
- [Chapter 4 — Character Encoding](#chapter-4-character-encoding)
- [Chapter 5 — Boolean Algebra & Logic Gates](#chapter-5-boolean-algebra--logic-gates)
- [Chapter 6 — Digital Circuits](#chapter-6-digital-circuits)
- [Chapter 7 — MASM32 Assembly Programming](#chapter-7-masm32-assembly-programming)
- [Chapter 8 — Memory Systems](#chapter-8-memory-systems)
- [Chapter 9 — Cache & Virtual Memory](#chapter-9-cache--virtual-memory)
- [Chapter 10 — Processors & Parallel Computing](#chapter-10-processors--parallel-computing)
- [Chapter 11 — Interfaces & Communication](#chapter-11-interfaces--communication)
- [Appendix A — Master Formula Sheet](#appendix-a-master-formula-sheet)
- [Appendix B — Common Exam Question Patterns](#appendix-b-common-exam-question-patterns)
- [Appendix C — Lab Quick-Reference](#appendix-c-lab-quick-reference)

---

<!-- VERSION: 1.0 | Chapter 1 | 2025 -->

## Chapter 1: History & Architecture Foundations

### 🚀 The Hook

The computer you're using right now — whether a laptop or phone — follows a design blueprint sketched in 1945 by a mathematician named John von Neumann. Nearly 80 years later, the same fundamental architecture is running aboard the James Webb Space Telescope, 1.5 million km from Earth. Understanding *why* this design won is the foundation of everything in this course.

---

### 📌 What You'll Learn

- Describe the Von Neumann model and identify each of its components
- Explain what the Fetch-Decode-Execute cycle does and trace it step by step
- Compare early computers (ENIAC, IAS) to modern architectures
- Identify the key bottleneck in Von Neumann systems and why it still matters
- Place major CPU families on a historical timeline

---

### 🧠 Core Concepts

#### The Von Neumann Model

**Von Neumann Architecture** — A computer design where programs and data share the same memory, connected to a CPU via a single bus.

*Analogy:* Imagine a chef (CPU) working in a restaurant kitchen. The fridge (Memory) stores both the ingredients (data) AND the recipe book (program). The chef reads one instruction at a time, fetches what's needed, and produces output. The kitchen counter (registers) is where the chef actually works.

```
+-------------------+
|      Memory       |  ← stores BOTH data & instructions
+-------------------+
         |
    [System Bus]
         |
+-------------------+
|       CPU         |
|  +-----------+    |
|  |    ALU    |    |  ← does the maths
|  +-----------+    |
|  |  Control  |    |  ← runs the show
|  |   Unit    |    |
|  +-----------+    |
|  | Registers |    |  ← tiny super-fast memory inside CPU
+-------------------+
         |
+-------------------+
|   I/O Devices     |  ← keyboard, screen, disk
+-------------------+
```

**The five components you must know:**

| Component | What It Does |
|-----------|-------------|
| Memory (RAM) | Stores program instructions AND data |
| ALU (Arithmetic Logic Unit) | Performs arithmetic and logical operations |
| Control Unit (CU) | Decodes instructions, coordinates all other parts |
| Registers | Ultra-fast temporary storage inside the CPU |
| I/O Devices | Communicate with the outside world |

> ⚠️ **Warning:** The *Von Neumann bottleneck* is the limited bandwidth between CPU and memory. The CPU can compute far faster than it can fetch data. This single idea drives almost everything in Chapters 8–10 (cache, memory hierarchy, pipelines).

---

#### The Fetch-Decode-Execute Cycle

This is the heartbeat of every computer. **Every single instruction** your CPU ever runs goes through this loop.

```
   +----------+
   |  FETCH   |  ← Read next instruction from memory into CPU
   +----------+
        |
   +----------+
   |  DECODE  |  ← Figure out what the instruction means
   +----------+
        |
   +----------+
   | EXECUTE  |  ← Carry out the instruction
   +----------+
        |
   (repeat forever)
```

**Key registers involved:**

| Register | Name | Role |
|----------|------|------|
| PC | Program Counter | Holds address of NEXT instruction |
| IR | Instruction Register | Holds CURRENT instruction being decoded |
| MAR | Memory Address Register | Holds address to read/write from memory |
| MDR | Memory Data Register | Holds data read from / to be written to memory |
| ACC | Accumulator | Holds results of ALU operations |

> 💡 **Tip:** In exams, trace through the FDE cycle with a specific instruction. Always start: "PC contains address X → fetch instruction at address X into IR → decode opcode → execute."

---

#### ENIAC (1945) and IAS (1952)

**ENIAC** — Electronic Numerical Integrator and Computer. The world's first general-purpose electronic computer.

- Weighed 30 tonnes, filled a room, used 18,000 vacuum tubes
- *Programmed by physically rewiring cables and setting switches* — no stored program
- Could do 5,000 additions per second (your phone does billions)

**IAS Machine** — Designed by von Neumann at the Institute for Advanced Study.

- First computer to use a *stored program* — instructions live in memory alongside data
- 40-bit word size, memory of 1024 words
- This design is the direct ancestor of every modern CPU

> 🚀 **Real World:** The transition from ENIAC (rewire to reprogram) to IAS (software in memory) is the same conceptual leap as moving from hardwired phone circuits to apps on a smartphone. The hardware stays the same; the behavior changes via software.

---

#### Evolution of Computing Generations

| Generation | Era | Technology | Speed |
|------------|-----|------------|-------|
| 1st | 1940s–50s | Vacuum tubes | Thousands of ops/sec |
| 2nd | 1950s–60s | Transistors | Millions of ops/sec |
| 3rd | 1960s–70s | Integrated circuits (ICs) | Hundreds of millions/sec |
| 4th | 1970s–now | Microprocessors (VLSI) | Billions of ops/sec |
| 5th | Now+ | AI chips, quantum | Trillions of ops/sec |

> ⚠️ **Warning:** Don't confuse *generation* with *architecture*. Von Neumann architecture spans generations 1 through 4+. Generation describes the *technology*; architecture describes the *design*.

---

### 🔢 Step-by-Step Procedures

#### Tracing the FDE Cycle

```
Given: Instruction at address 100 says "ADD 200" (add value at address 200 to accumulator)

Step 1: FETCH
  - MAR ← PC (MAR = 100)
  - MDR ← Memory[MAR] (MDR = "ADD 200")
  - IR ← MDR (IR = "ADD 200")
  - PC ← PC + 1 (PC = 101)

Step 2: DECODE
  - CU reads IR, identifies opcode = ADD, operand = 200

Step 3: EXECUTE
  - MAR ← 200
  - MDR ← Memory[200] (fetch the value to add)
  - ACC ← ACC + MDR

Result: Accumulator now contains old_ACC + value_at_address_200
```

---

### ⚡ Why This Matters

Every performance optimization you will ever encounter — caches, pipelines, multi-core processors — exists specifically to fight the Von Neumann bottleneck. Understanding the original model makes all those solutions make sense instinctively. In astronomy, the flight computers on spacecraft like Voyager still follow FDE cycles; they're just radiation-hardened variants of the same design.

---

### 🧪 Lab Prep Exercises

**Exercise 1.1** — Label all five components of the Von Neumann architecture and write one sentence describing each.

<details>
<summary>Show Solution</summary>

1. **Memory** — Stores both program instructions and data in a single addressable space.
2. **ALU** — Performs all arithmetic (add, subtract) and logical (AND, OR) operations.
3. **Control Unit** — Fetches, decodes, and coordinates execution of every instruction.
4. **Registers** — Small, extremely fast storage locations inside the CPU (PC, IR, ACC, etc.).
5. **I/O Devices** — Allow the computer to receive input (keyboard) and produce output (screen, disk).

</details>

---

**Exercise 1.2** — Trace the Fetch-Decode-Execute cycle for the instruction `LOAD 50` (load value from address 50 into accumulator), assuming PC = 10.

<details>
<summary>Show Solution</summary>

```
FETCH:
  MAR ← 10 (address of instruction)
  MDR ← Memory[10] = "LOAD 50"
  IR  ← "LOAD 50"
  PC  ← 11

DECODE:
  CU identifies opcode = LOAD, operand = 50

EXECUTE:
  MAR ← 50
  MDR ← Memory[50]
  ACC ← MDR
```
Result: Accumulator contains the value stored at memory address 50.

</details>

---

**Exercise 1.3** — List two differences between ENIAC and the IAS machine.

<details>
<summary>Show Solution</summary>

1. **Stored program:** IAS stored instructions in memory alongside data (Von Neumann design). ENIAC required physical rewiring to change programs — no stored program concept.
2. **Programmability:** IAS could be reprogrammed by changing memory contents. ENIAC had to be manually reconfigured with cables and switches.

(Bonus: ENIAC used vacuum tubes and was much larger; IAS was a more compact design.)

</details>

---

### 📝 Exam Quick-Fire

**Q1.** Which component of the Von Neumann architecture is responsible for decoding instructions?

- A) ALU
- B) Accumulator
- C) Control Unit
- D) Memory

**Q2.** What is the Von Neumann bottleneck?

- A) The CPU cannot perform floating-point arithmetic
- B) Memory and CPU share the same bus, limiting data transfer speed
- C) There are too few registers in the CPU
- D) I/O devices are slower than the CPU

**Q3.** In the FDE cycle, which register holds the address of the NEXT instruction to be fetched?

- A) IR
- B) MAR
- C) MDR
- D) PC

**Q4.** ENIAC differed from the IAS machine primarily because:

- A) ENIAC had no ALU
- B) ENIAC could not do arithmetic
- C) ENIAC did not use a stored-program model
- D) ENIAC was built with transistors

**Q5.** Which computing generation introduced microprocessors?

- A) 1st
- B) 2nd
- C) 3rd
- D) 4th

<details>
<summary>Show Answers</summary>

1. **C** — The Control Unit decodes instructions (the ALU executes them).
2. **B** — The shared bus between CPU and memory creates a data transfer bottleneck.
3. **D** — The Program Counter (PC) always points to the next instruction.
4. **C** — ENIAC required physical rewiring; the IAS machine introduced the stored-program concept.
5. **D** — The 4th generation (1970s onward) introduced VLSI microprocessors.

</details>

---

### 🔁 Chapter Summary

- The **Von Neumann model** has 5 parts: Memory, ALU, Control Unit, Registers, I/O
- Programs **and** data share the same memory — this is the defining feature
- The **FDE cycle** (Fetch → Decode → Execute) is the CPU's heartbeat
- Key registers: **PC** (next instruction), **IR** (current instruction), **ACC** (result)
- The **Von Neumann bottleneck**: CPU is faster than the memory bus
- **ENIAC** (1945): first electronic computer, no stored program
- **IAS** (1952): first stored-program computer — direct ancestor of all modern CPUs
- Computing generations track *technology* (tubes → transistors → ICs → microprocessors)

---

### 🔗 Connections to Other Chapters

This chapter provides the *mental model* for the entire course. Every subsequent chapter is about one component of this architecture in detail. **Chapter 7** (Assembly) shows you what instructions look like inside the CPU. **Chapters 8–9** (Memory & Cache) explain how to fight the bottleneck identified here. **Chapter 10** (Processors) explores how the FDE cycle evolved into pipelines and multi-core.

---

<!-- VERSION: 1.0 | Chapter 2 | 2025 -->

## Chapter 2: Number Systems

### 🚀 The Hook

The Mars Climate Orbiter was lost in 1999 — a $327 million spacecraft — because one team used metric units and another used imperial units in the same data stream. In computers, the equivalent disaster happens with number systems. Every piece of data in your computer — images, music, your code — is ultimately a number in binary. Master this chapter and you'll never be confused by hex dumps, memory addresses, or bitwise operations again.

---

### 📌 What You'll Learn

- Convert any decimal integer to binary, octal, and hexadecimal in under 2 minutes
- Convert binary fractions to decimal and vice versa
- Perform binary addition and subtraction using 2's complement
- Read a hex memory dump without a calculator
- Spot common conversion errors before they cost you marks

---

### 🧠 Core Concepts

#### Why Different Bases?

**Number base (radix)** — The number of unique digits a system uses before it "carries over."

*Analogy:* Decimal (base 10) is what you count on fingers — 0 to 9, then carry. Binary (base 2) is like a light switch — only 0 or 1. Hexadecimal (base 16) is a shorthand for binary that fits exactly 4 bits per digit.

| System | Base | Digits Used | Used For |
|--------|------|-------------|---------|
| Binary | 2 | 0, 1 | Internal CPU/memory storage |
| Octal | 8 | 0–7 | Legacy Unix permissions, some embedded |
| Decimal | 10 | 0–9 | Human interface |
| Hexadecimal | 16 | 0–9, A–F | Memory addresses, color codes, debugging |

---

#### Positional Value

Every digit's value depends on its **position** (its power of the base).

```
Decimal:  3 4 5  =  3×10² + 4×10¹ + 5×10⁰  =  300 + 40 + 5  =  345

Binary:   1 0 1  =  1×2²  + 0×2¹  + 1×2⁰  =  4   + 0  + 1  =  5

Hex:      2 A    =  2×16¹ + 10×16⁰ =  32  + 10      =  42
```

> 💡 **Tip:** In exams, always write out the positional values explicitly. It takes 30 seconds and prevents errors.

---

#### Hex Cheat Grid

| Decimal | Hex | Binary |
|---------|-----|--------|
| 0 | 0 | 0000 |
| 1 | 1 | 0001 |
| 2 | 2 | 0010 |
| 3 | 3 | 0011 |
| 4 | 4 | 0100 |
| 5 | 5 | 0101 |
| 6 | 6 | 0110 |
| 7 | 7 | 0111 |
| 8 | 8 | 1000 |
| 9 | 9 | 1001 |
| 10 | A | 1010 |
| 11 | B | 1011 |
| 12 | C | 1100 |
| 13 | D | 1101 |
| 14 | E | 1110 |
| 15 | F | 1111 |

> 💡 **Memory trick for A–F:** "**A**ll **B**ig **C**ats **D**ine **E**very **F**riday" = 10, 11, 12, 13, 14, 15

---

#### Full Conversion Table: Decimal 0–31 (excerpt; full 0–255 in Appendix C)

| Dec | Bin | Oct | Hex |
|-----|-----|-----|-----|
| 0 | 00000000 | 000 | 00 |
| 1 | 00000001 | 001 | 01 |
| 2 | 00000010 | 002 | 02 |
| 3 | 00000011 | 003 | 03 |
| 4 | 00000100 | 004 | 04 |
| 5 | 00000101 | 005 | 05 |
| 6 | 00000110 | 006 | 06 |
| 7 | 00000111 | 007 | 07 |
| 8 | 00001000 | 010 | 08 |
| 9 | 00001001 | 011 | 09 |
| 10 | 00001010 | 012 | 0A |
| 15 | 00001111 | 017 | 0F |
| 16 | 00010000 | 020 | 10 |
| 31 | 00011111 | 037 | 1F |
| 32 | 00100000 | 040 | 20 |
| 64 | 01000000 | 100 | 40 |
| 127 | 01111111 | 177 | 7F |
| 128 | 10000000 | 200 | 80 |
| 255 | 11111111 | 377 | FF |

---

#### 2's Complement (Negative Numbers in Binary)

*Analogy:* An odometer that rolls backward. If you have an 8-bit number and subtract 1 from 00000000, you wrap around to 11111111 — and that's exactly how 2's complement works. It lets us represent negative numbers without a separate "sign switch."

**Rule:** To negate a number in 2's complement:
1. Flip all bits (invert: 0→1, 1→0) — this is the 1's complement
2. Add 1

```
Example: -5 in 8-bit 2's complement

+5 in binary:  00000101
Step 1 — flip: 11111010
Step 2 — +1:   11111011  ← this is -5
```

**Range for n-bit 2's complement:**
```
Minimum: -2^(n-1)
Maximum: +2^(n-1) - 1

8-bit: -128 to +127
16-bit: -32768 to +32767
```

> ⚠️ **Warning:** If the MSB (leftmost bit) is 1, the number is negative in 2's complement. Never interpret a 2's complement number as unsigned without knowing which representation is intended.

---

#### Binary Arithmetic

**Addition rules:**
```
0 + 0 = 0
0 + 1 = 1
1 + 0 = 1
1 + 1 = 0, carry 1
1 + 1 + 1 (carry) = 1, carry 1
```

**Subtraction using 2's complement:**
Instead of A − B, compute A + (−B) using 2's complement of B.

---

### 🔢 Step-by-Step Procedures

#### Decimal Integer → Binary (Division Method)

```
Convert 45 to binary:

Step 1: Divide by 2, record remainder
  45 ÷ 2 = 22 remainder 1
  22 ÷ 2 = 11 remainder 0
  11 ÷ 2 =  5 remainder 1
   5 ÷ 2 =  2 remainder 1
   2 ÷ 2 =  1 remainder 0
   1 ÷ 2 =  0 remainder 1

Step 2: Read remainders BOTTOM to TOP
Result: 45₁₀ = 101101₂
```

#### Decimal Fraction → Binary (Multiplication Method)

```
Convert 0.625 to binary:

Step 1: Multiply by 2, record integer part
  0.625 × 2 = 1.25  → bit = 1, keep 0.25
  0.25  × 2 = 0.50  → bit = 0, keep 0.50
  0.50  × 2 = 1.00  → bit = 1, keep 0.00 (stop — fraction = 0)

Step 2: Read bits TOP to BOTTOM
Result: 0.625₁₀ = 0.101₂
```

#### Binary → Hexadecimal (Grouping Method)

```
Convert 11010111₂ to hex:

Step 1: Group into 4-bit chunks from RIGHT
  1101 | 0111

Step 2: Convert each group using the Hex Cheat Grid
  1101 = D
  0111 = 7

Result: 11010111₂ = D7₁₆
```

#### Hex → Decimal

```
Convert B4₁₆ to decimal:

Step 1: B = 11, 4 = 4
Step 2: 11 × 16¹ + 4 × 16⁰
      = 11 × 16 + 4 × 1
      = 176 + 4
Result: B4₁₆ = 180₁₀
```

#### Binary Subtraction (using 2's complement)

```
Calculate 12 - 5 in 8-bit binary:

Step 1: Write 12 in binary: 00001100
Step 2: Write 5 in binary:  00000101
Step 3: Negate 5 (find 2's complement of 00000101):
  Flip:  11111010
  Add 1: 11111011  ← this is -5

Step 4: Add 12 + (-5):
    00001100
  + 11111011
  ----------
    00000111  ← ignore overflow carry bit

Result: 00000111₂ = 7₁₀  ✓ (12 - 5 = 7)
```

---

### ⚡ Why This Matters

Every memory address you see in a debugger, every color value in CSS (`#FF5733`), every IP address subnet mask — they're all hex or binary. Assembly programmers read hex dumps daily. When you write `0xFF` in C, you're using hex. When you set Unix file permissions with `chmod 755`, you're using octal. This is the alphabet of computing.

---

### 🧪 Lab Prep Exercises

**Exercise 2.1** — Convert 178₁₀ to binary, octal, and hexadecimal.

<details>
<summary>Show Solution</summary>

**Binary (division by 2):**
```
178 ÷ 2 = 89 r 0
 89 ÷ 2 = 44 r 1
 44 ÷ 2 = 22 r 0
 22 ÷ 2 = 11 r 0
 11 ÷ 2 =  5 r 1
  5 ÷ 2 =  2 r 1
  2 ÷ 2 =  1 r 0
  1 ÷ 2 =  0 r 1

Read bottom to top: 10110010₂
```

**Hex (group binary into 4s):**
```
1011 | 0010
  B  |  2
= B2₁₆
```

**Octal (group binary into 3s from right):**
```
010 | 110 | 010
 2  |  6  |  2
= 262₈
```

Verify: 2×64 + 6×8 + 2 = 128 + 48 + 2 = 178 ✓

</details>

---

**Exercise 2.2** — Represent -37 in 8-bit 2's complement.

<details>
<summary>Show Solution</summary>

```
Step 1: +37 in binary:   00100101
Step 2: Flip all bits:   11011010
Step 3: Add 1:           11011011

Result: -37 = 11011011₂ (in 8-bit 2's complement)

Verify: 128+64+16+8+2+1 = 219; 219 - 256 = -37 ✓
```

</details>

---

**Exercise 2.3** — Compute 0.3125₁₀ in binary.

<details>
<summary>Show Solution</summary>

```
0.3125 × 2 = 0.625 → bit 0, carry 0.625
0.625  × 2 = 1.25  → bit 1, carry 0.25
0.25   × 2 = 0.5   → bit 0, carry 0.5
0.5    × 2 = 1.0   → bit 1, carry 0.0 (stop)

Result: 0.3125₁₀ = 0.0101₂
```

</details>

---

**Exercise 2.4** — Add 01101010₂ + 00110101₂. Show all carries.

<details>
<summary>Show Solution</summary>

```
  01101010
+ 00110101
----------
Carries:
  01100000 (carry row)

  01101010
+ 00110101
----------
  10011111

Result: 10011111₂ = 159₁₀
Check: 106 + 53 = 159 ✓
```

</details>

---

### 📝 Exam Quick-Fire

**Q1.** What is 0xAC in decimal?

- A) 162
- B) 172
- C) 174
- D) 176

**Q2.** The binary number 10110110₂ is represented in hexadecimal as:

- A) B5
- B) B6
- C) C6
- D) D6

**Q3.** Which method is used to convert a decimal FRACTION to binary?

- A) Repeated division by 2
- B) Repeated multiplication by 2
- C) Repeated subtraction
- D) Grouping into 4-bit nibbles

**Q4.** In 8-bit 2's complement, what is the decimal value of 11110000?

- A) -15
- B) -16
- C) 240
- D) -240

**Q5.** To convert binary to hexadecimal, you group bits in chunks of:

- A) 2
- B) 3
- C) 4
- D) 8

<details>
<summary>Show Answers</summary>

1. **B** — A=10, C=12. 10×16 + 12 = 160 + 12 = 172.
2. **B** — 1011=B, 0110=6 → B6.
3. **B** — Fractions use repeated multiplication by 2 (read integer parts top to bottom).
4. **B** — Flip: 00001111, add 1: 00010000 = 16. So original = -16.
5. **C** — Each hex digit represents exactly 4 bits (a nibble).

</details>

---

### 🔁 Chapter Summary

- **Binary** (base 2): digits 0–1; each position is a power of 2
- **Hex** (base 16): digits 0–9, A–F; each hex digit = exactly 4 binary bits
- **Octal** (base 8): digits 0–7; each octal digit = exactly 3 binary bits
- Convert **decimal → binary**: repeated division by 2, read remainders bottom-up
- Convert **fraction → binary**: repeated multiplication by 2, read integer parts top-down
- Convert **binary → hex**: group into 4s from right, use cheat grid
- **2's complement**: flip all bits + 1 = negation; MSB=1 means negative
- **Binary addition**: carry when 1+1; subtraction = add the 2's complement

---

### 🔗 Connections to Other Chapters

Number systems are the language of this entire course. **Chapter 3** extends this to floating-point (fractional binary in a specific format). **Chapter 4** uses ASCII codes (decimal/hex values for characters). **Chapter 5** works with binary 1s and 0s as logic states. **Chapter 7** (Assembly) uses hex constants everywhere.

---

<!-- VERSION: 1.0 | Chapter 3 | 2025 -->

## Chapter 3: Floating Point Numbers

### 🚀 The Hook

In 1996, the Ariane 5 rocket exploded 37 seconds after launch. The cause? A 64-bit floating point number was converted to a 16-bit integer — and the number was too large to fit. The software worked perfectly on the older Ariane 4, which flew at lower speeds. One floating point overflow: €370 million lost. Understanding IEEE-754 isn't just an exam topic — it's flight safety.

---

### 📌 What You'll Learn

- Draw and label the IEEE-754 single and double precision bit layout from memory
- Encode any decimal number into IEEE-754 single precision
- Decode an IEEE-754 binary pattern back to decimal
- Explain the bias exponent and calculate it correctly
- Identify special values: zero, infinity, NaN

---

### 🧠 Core Concepts

#### Scientific Notation for Binary

*Analogy:* Just like decimal scientific notation writes 123,000 as 1.23 × 10⁵, binary floating point writes numbers as 1.fraction × 2^exponent. The "1." at the front is always there (for normalized numbers), so we don't store it — it's a hidden free bit.

**IEEE-754 Single Precision (32 bits):**

```
Bit:  31  | 30    23 | 22          0
      +-+-+----------+--------------+
      |S|  Exponent  |   Mantissa   |
      +-+-+----------+--------------+
       1      8           23
      bit    bits         bits
```

**IEEE-754 Double Precision (64 bits):**

```
Bit:  63  | 62    52 | 51          0
      +-+-+----------+--------------+
      |S|  Exponent  |   Mantissa   |
      +-+-+----------+--------------+
       1      11          52
      bit    bits         bits
```

| Field | Single | Double | Meaning |
|-------|--------|--------|---------|
| Sign (S) | 1 bit | 1 bit | 0 = positive, 1 = negative |
| Exponent | 8 bits | 11 bits | Biased exponent |
| Mantissa | 23 bits | 52 bits | Fractional part (hidden leading 1) |

---

#### The Bias Exponent

*Why a bias?* We need to represent both positive and negative exponents (e.g., 2⁻³ and 2⁵). Instead of using 2's complement, IEEE uses a **bias** so the stored exponent is always non-negative.

**Bias formula:**
```
Bias = 2^(n-1) - 1

Single precision (8-bit exponent): Bias = 2^7 - 1 = 127
Double precision (11-bit exponent): Bias = 2^10 - 1 = 1023

Stored exponent = Actual exponent + Bias
Actual exponent = Stored exponent - Bias
```

---

#### Special Values (Single Precision)

| Stored Exponent | Mantissa | Value |
|-----------------|----------|-------|
| 00000000 (0) | 00...0 | Zero (±0) |
| 00000000 (0) | non-zero | Denormalized number |
| 11111111 (255) | 00...0 | ±Infinity |
| 11111111 (255) | non-zero | NaN (Not a Number) |
| 1–254 | anything | Normal number |

> ⚠️ **Warning:** NaN is the result of invalid operations like 0/0 or √(-1). It *propagates* — any operation involving NaN returns NaN. This is why unchecked NaN values silently corrupt calculations.

---

#### The Hidden Bit

For normalized numbers, the mantissa always starts with `1.` in binary. Since this leading 1 is **always** there, it's **not stored** — this gives an extra free bit of precision.

```
The actual mantissa = 1.stored_bits
Example: stored mantissa = 01000000000000000000000
         actual mantissa = 1.01000000000000000000000 = 1.25
```

---

### 🔢 Step-by-Step Procedures

#### Decimal → IEEE-754 Single Precision

```
Convert -12.375 to IEEE-754 single precision

Step 1: Determine sign bit
  Negative → Sign = 1

Step 2: Convert integer part to binary
  12 → 1100

Step 3: Convert fractional part to binary
  0.375 × 2 = 0.75 → 0
  0.75  × 2 = 1.5  → 1
  0.5   × 2 = 1.0  → 1
  0.375 → .011

Step 4: Combine: 12.375 = 1100.011₂

Step 5: Normalize (move point to after first 1)
  1100.011 = 1.100011 × 2³
  Actual exponent = 3

Step 6: Calculate stored exponent
  Stored exponent = 3 + 127 = 130 = 10000010₂

Step 7: Write mantissa (bits after the "1.")
  100011 followed by zeros to fill 23 bits:
  10001100000000000000000

Step 8: Assemble
  Sign: 1
  Exponent: 10000010
  Mantissa: 10001100000000000000000

  1 10000010 10001100000000000000000

  = C1460000₁₆ (as hex)
```

#### IEEE-754 Single Precision → Decimal

```
Decode: 0 10000001 01000000000000000000000

Step 1: Sign bit = 0 → positive

Step 2: Stored exponent = 10000001₂ = 129₁₀
  Actual exponent = 129 - 127 = 2

Step 3: Mantissa = 01000000000000000000000
  With hidden 1: 1.01 (rest are zeros)
  = 1 + 0.25 = 1.25

Step 4: Value = 1.25 × 2² = 1.25 × 4 = 5.0

Result: 5.0
```

---

### ⚡ Why This Matters

Floating point is how your CPU handles all real-number math — graphics, physics engines, scientific simulations, GPS coordinates. Every language (C `float`, Python `float`, JavaScript `Number`) uses IEEE-754 underneath. The quirks of floating point (0.1 + 0.2 ≠ 0.3 in most languages) come directly from the precision limits you're learning here.

---

### 🧪 Lab Prep Exercises

**Exercise 3.1** — Encode 0.1 in IEEE-754 single precision (show why it's not exact).

<details>
<summary>Show Solution</summary>

```
Sign: 0 (positive)

Convert 0.1 to binary:
0.1 × 2 = 0.2 → 0
0.2 × 2 = 0.4 → 0
0.4 × 2 = 0.8 → 0
0.8 × 2 = 1.6 → 1
0.6 × 2 = 1.2 → 1
0.2 × 2 = 0.4 → 0  ← repeats!
0.4 × 2 = 0.8 → 0
...
0.1₁₀ = 0.0001100110011...₂ (infinite repeating pattern)

Normalized: 1.100110011001100... × 2⁻⁴

Stored exponent: -4 + 127 = 123 = 01111011₂
Mantissa (23 bits): 10011001100110011001101 (rounded)

Final: 0 01111011 10011001100110011001101

Since the binary representation repeats forever, it CANNOT be stored exactly in 23 bits.
This is why 0.1 + 0.2 ≠ 0.3 in most programming languages.
```

</details>

---

**Exercise 3.2** — What decimal value does this IEEE-754 pattern represent?
`0 10000100 10100000000000000000000`

<details>
<summary>Show Solution</summary>

```
Sign: 0 → positive
Exponent: 10000100₂ = 132₁₀ → Actual = 132 - 127 = 5
Mantissa: 10100000000000000000000 → 1.101₂ = 1 + 0.5 + 0.125 = 1.625

Value = 1.625 × 2⁵ = 1.625 × 32 = 52.0
```

</details>

---

**Exercise 3.3** — Encode -0.75 in IEEE-754 single precision.

<details>
<summary>Show Solution</summary>

```
Sign: 1 (negative)
0.75 = 0.11₂ = 1.1 × 2⁻¹
Actual exponent: -1
Stored exponent: -1 + 127 = 126 = 01111110₂
Mantissa: 10000000000000000000000

Result: 1 01111110 10000000000000000000000
```

</details>

---

### 📝 Exam Quick-Fire

**Q1.** In IEEE-754 single precision, the exponent bias is:

- A) 126
- B) 127
- C) 128
- D) 255

**Q2.** An IEEE-754 value with all exponent bits = 1 and mantissa ≠ 0 represents:

- A) Zero
- B) Infinity
- C) NaN
- D) The maximum possible value

**Q3.** The "hidden bit" in IEEE-754 refers to:

- A) The sign bit
- B) The least significant mantissa bit
- C) The implicit leading 1 in normalized numbers
- D) The overflow bit

**Q4.** Double precision IEEE-754 uses how many bits for the exponent?

- A) 8
- B) 10
- C) 11
- D) 12

**Q5.** To find the actual exponent from the stored exponent in single precision:

- A) Add 127
- B) Subtract 127
- C) Multiply by 2
- D) Apply 2's complement

<details>
<summary>Show Answers</summary>

1. **B** — Bias = 2^(8-1) - 1 = 127 for single precision.
2. **C** — Exponent all 1s + non-zero mantissa = NaN.
3. **C** — Normalized numbers always start with 1. before the point; this isn't stored, saving one bit.
4. **C** — Double precision: 1 sign + 11 exponent + 52 mantissa = 64 bits.
5. **B** — Actual exponent = Stored exponent − 127.

</details>

---

### 🔁 Chapter Summary

- IEEE-754 uses **sign, biased exponent, and mantissa** to represent real numbers
- **Single precision**: 1+8+23 = 32 bits; **Double**: 1+11+52 = 64 bits
- **Bias** = 127 (single) or 1023 (double); stored = actual + bias
- The **hidden bit**: leading 1 in mantissa is implicit (not stored)
- **Special values**: 0 (all zeros), ±∞ (exp=255, mantissa=0), NaN (exp=255, mantissa≠0)
- Many decimal fractions (like 0.1) **cannot be exactly represented** — infinite binary fractions are rounded
- Floating point precision errors are *inherent* to the format, not bugs

---

### 🔗 Connections to Other Chapters

Floating point builds directly on **Chapter 2** (binary fractions and binary representation). It connects to **Chapter 7** (Assembly), where FPU instructions like `FADD` operate on IEEE-754 values. In **Chapter 10** (Processors), the floating point unit (FPU) is a dedicated execution unit inside the CPU.

---

<!-- VERSION: 1.0 | Chapter 4 | 2025 -->

## Chapter 4: Character Encoding

### 🚀 The Hook

When the Mars Pathfinder mission sent back data in 1997, every character in the telemetry stream was encoded as a number. The letter 'A' was the number 65. A space was 32. Without a shared encoding standard between Earth's computers and the spacecraft's transmitter, the data would be gibberish. Character encoding is the unsung hero of all digital communication.

---

### 📌 What You'll Learn

- Look up any ASCII character code from memory or a table
- Explain the difference between ASCII and EBCDIC
- Convert text strings to their hexadecimal ASCII representations
- Explain why ASCII was extended and what problems that caused
- Understand where Unicode fits into the picture

---

### 🧠 Core Concepts

#### ASCII — American Standard Code for Information Interchange

**ASCII** — A 7-bit encoding standard mapping 128 characters (0–127) to integers.

*Analogy:* A universal phone book for characters. Computers don't speak letters; they speak numbers. ASCII is the agreement: "Everyone agrees that 65 means 'A'." As long as all computers use the same book, they can exchange text.

**Key ASCII Values to Memorize:**

| Character | Decimal | Hex | Binary |
|-----------|---------|-----|--------|
| NULL | 0 | 00 | 0000000 |
| Space | 32 | 20 | 0100000 |
| `0` | 48 | 30 | 0110000 |
| `9` | 57 | 39 | 0111001 |
| `A` | 65 | 41 | 1000001 |
| `Z` | 90 | 5A | 1011010 |
| `a` | 97 | 61 | 1100001 |
| `z` | 122 | 7A | 1111010 |
| DEL | 127 | 7F | 1111111 |

> 💡 **Key pattern:** `A` = 65, `a` = 97. The difference is **32** (= 0x20). Uppercase and lowercase differ by exactly one bit (bit 5). This is why `tolower()` in C is just `OR 0x20`.

**Control Characters (0–31):** Non-printable. Include TAB (9), newline LF (10), carriage return CR (13).

---

#### Extended ASCII (8-bit)

Original ASCII is 7-bit (128 characters). **Extended ASCII** uses the 8th bit for characters 128–255 — accented letters, symbols, box-drawing characters.

> ⚠️ **Warning:** Extended ASCII is *not a single standard*. There were dozens of incompatible "code pages" (Windows-1252, ISO-8859-1, etc.). This is exactly the problem Unicode was designed to solve.

---

#### EBCDIC — Extended Binary Coded Decimal Interchange Code

**EBCDIC** — IBM's 8-bit character encoding, used on mainframe computers.

*Analogy:* Imagine if America used one alphabet layout and Europe used a different one for the same keyboard. EBCDIC and ASCII represent the same characters but with completely different numbering.

| Character | ASCII (dec) | EBCDIC (dec) |
|-----------|------------|--------------|
| `A` | 65 | 193 |
| `a` | 97 | 129 |
| `0` | 48 | 240 |
| Space | 32 | 64 |

> 💡 **Exam tip:** You won't need to memorize EBCDIC codes, but you DO need to know: EBCDIC is used by IBM mainframes; ASCII is used by virtually everything else; they are INCOMPATIBLE byte-for-byte.

---

#### Unicode — The Modern Solution

**Unicode** — An international standard encoding over 140,000 characters from every writing system.

- **UTF-8**: Variable-length (1–4 bytes). ASCII-compatible (first 128 code points are identical). The dominant encoding on the web.
- **UTF-16**: Fixed 2 bytes for most characters (used internally by Windows, Java).
- **UTF-32**: Fixed 4 bytes per character (simple but wastes space).

> 🚀 **Real World:** When you see `\u0041` in a Python string, that's Unicode code point for 'A'. When you scrape web data from international sites and see garbled characters, you're almost always looking at an encoding mismatch.

---

### 🔢 Step-by-Step Procedures

#### Text String → ASCII Hex

```
Convert "Hi" to ASCII hex:

Step 1: Look up each character
  H = 72 decimal = 48 hex
  i = 105 decimal = 69 hex

Step 2: Write as hex bytes
Result: 48 69

In a hex dump: 48 69 (optionally with 0x prefix: 0x48 0x69)
```

#### ASCII Hex → Text

```
Decode: 57 6F 72 6C 64

Step 1: Convert each hex to decimal
  57 = 87, 6F = 111, 72 = 114, 6C = 108, 64 = 100

Step 2: Look up ASCII table
  87=W, 111=o, 114=r, 108=l, 100=d

Result: "World"
```

---

### ⚡ Why This Matters

Every text file, web page, email, and database stores characters using an encoding. SQL injection attacks, encoding bugs in international apps, and data corruption during file transfers are all rooted in encoding mismatches. Web developers who don't understand UTF-8 vs. Latin-1 cause real security vulnerabilities.

---

### 🧪 Lab Prep Exercises

**Exercise 4.1** — Write the ASCII hex representation of "CSA".

<details>
<summary>Show Solution</summary>

C = 67 = 43₁₆
S = 83 = 53₁₆
A = 65 = 41₁₆

Result: 43 53 41

</details>

---

**Exercise 4.2** — What string is encoded by: `48 65 6C 6C 6F`?

<details>
<summary>Show Solution</summary>

0x48=72=H, 0x65=101=e, 0x6C=108=l, 0x6C=108=l, 0x6F=111=o

Result: "Hello"

</details>

---

**Exercise 4.3** — What is the ASCII decimal value of 'z'? What is 'Z'? What is the difference and why?

<details>
<summary>Show Solution</summary>

'z' = 122, 'Z' = 90. Difference = 32 = 0x20 = 0b00100000.

The difference is exactly bit 5 (the 6th bit from the right). Lowercase letters have bit 5 set to 1; uppercase have it set to 0. This allows simple bitwise operations to change case: OR with 0x20 = lowercase; AND with 0xDF = uppercase.

</details>

---

### 📝 Exam Quick-Fire

**Q1.** How many bits does standard ASCII use?

- A) 6
- B) 7
- C) 8
- D) 16

**Q2.** The ASCII decimal value of the character '0' (zero digit) is:

- A) 0
- B) 32
- C) 48
- D) 64

**Q3.** EBCDIC is primarily used by:

- A) Unix/Linux systems
- B) IBM mainframes
- C) Apple Macintosh computers
- D) Internet web servers

**Q4.** The ASCII value of 'A' is 65. What is the ASCII value of 'a'?

- A) 66
- B) 91
- C) 97
- D) 129

**Q5.** UTF-8 encoding is notable because:

- A) It always uses exactly 2 bytes per character
- B) It's incompatible with ASCII
- C) It's variable length and ASCII-compatible
- D) It only supports the English alphabet

<details>
<summary>Show Answers</summary>

1. **B** — Standard ASCII is 7-bit (128 characters, 0–127).
2. **C** — '0' through '9' are decimal 48–57 in ASCII.
3. **B** — EBCDIC is IBM's encoding used on mainframes.
4. **C** — 65 + 32 = 97. Lowercase = uppercase + 32.
5. **C** — UTF-8 is variable-length (1–4 bytes) and the first 128 code points match ASCII exactly.

</details>

---

### 🔁 Chapter Summary

- **ASCII**: 7-bit, 128 characters; standard for English text in computers
- **Key values**: Space=32, '0'=48, 'A'=65, 'a'=97; lowercase = uppercase + 32
- **Extended ASCII**: 8-bit, adds 128 more characters; many incompatible variants
- **EBCDIC**: IBM mainframe encoding; same characters, completely different codes to ASCII
- **Unicode**: modern universal standard; 140,000+ characters; several encodings (UTF-8, UTF-16, UTF-32)
- **UTF-8**: most common web encoding; variable-length; ASCII-compatible
- Encoding mismatches cause garbled text — always know what encoding a file uses
- Control characters (0–31) are non-printable: LF=10, CR=13, TAB=9

---

### 🔗 Connections to Other Chapters

Character encoding builds on **Chapter 2** (numbers and hex representation). In **Chapter 7** (Assembly), string operations work directly with ASCII byte values. In **Chapter 8** (Memory), character arrays are stored as consecutive ASCII bytes in RAM.

---

<!-- VERSION: 1.0 | Chapter 5 | 2025 -->

## Chapter 5: Boolean Algebra & Logic Gates

### 🚀 The Hook

Every decision your CPU makes — every `if` statement, every comparison, every branch — comes down to Boolean logic at the transistor level. When NASA's Curiosity rover decides whether to fire its retrorockets, that decision flows through billions of logic gates running Boolean expressions. George Boole invented this algebra in 1854, with no idea it would one day land robots on Mars.

---

### 📌 What You'll Learn

- Evaluate any Boolean expression using truth tables
- Apply De Morgan's Law to simplify logic expressions
- Minimize a 4-variable Boolean function using a Karnaugh Map
- Derive Sum of Products (SOP) expressions from truth tables
- Identify NAND, NOR, XOR gates and explain why NAND is called "universal"

---

### 🧠 Core Concepts

#### Boolean Variables and Basic Operations

**Boolean variable** — A variable that can only be 0 (FALSE) or 1 (TRUE).

Three fundamental operations:

| Operation | Symbol | Meaning |
|-----------|--------|---------|
| AND | A · B or AB | Both must be true |
| OR | A + B | At least one must be true |
| NOT | Ā or A' | Invert (flip) |

---

#### Truth Tables for All Basic Gates

**AND Gate:**

| A | B | A·B |
|---|---|-----|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

**OR Gate:**

| A | B | A+B |
|---|---|-----|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

**NOT Gate:**

| A | Ā |
|---|---|
| 0 | 1 |
| 1 | 0 |

**NAND Gate (NOT AND):**

| A | B | A·B̄ |
|---|---|------|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

**NOR Gate (NOT OR):**

| A | B | A+B̄ |
|---|---|------|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

**XOR Gate (Exclusive OR):**

| A | B | A⊕B |
|---|---|-----|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

> 💡 **XOR memory trick:** "One or the other, but NOT both." XOR outputs 1 only when inputs *differ*.

> 💡 **NAND is universal:** Any logic function can be built from NAND gates alone. Chip manufacturers love this — one gate type, infinite possibilities.

---

#### Boolean Algebra Axioms

```
Identity:      A + 0 = A        A · 1 = A
Null:          A + 1 = 1        A · 0 = 0
Idempotent:    A + A = A        A · A = A
Complement:    A + Ā = 1        A · Ā = 0
Double negation: Ā̄ = A
Commutative:   A+B = B+A        AB = BA
Associative:   (A+B)+C = A+(B+C)
Distributive:  A(B+C) = AB+AC   A+BC = (A+B)(A+C)
```

---

#### De Morgan's Law

*Analogy:* "Neither Alice nor Bob is coming" means the same as "Alice isn't coming AND Bob isn't coming." De Morgan translates between OR-with-NOT and AND-with-NOT.

```
De Morgan's Law:
  ̄(A + B) = Ā · B̄     "NOT(A OR B)  = (NOT A) AND (NOT B)"
  ̄(A · B) = Ā + B̄     "NOT(A AND B) = (NOT A) OR  (NOT B)"
```

**Memory trick:** "Break the bar, change the sign."
- Break the overbar over the whole expression
- Each variable gets its own bar
- AND ↔ OR (the operator flips)

```
Example: ̄(AB) = Ā + B̄
  Proof: check with truth table A=1, B=1:
    LHS: NOT(1·1) = NOT(1) = 0
    RHS: NOT(1) + NOT(1) = 0 + 0 = 0  ✓
```

---

#### Karnaugh Maps (K-maps)

**K-map** — A grid-based visual method for minimizing Boolean expressions by grouping adjacent 1s.

*Analogy:* Imagine circling areas on a grid to simplify a complicated city bus route map. Each circle you draw eliminates a variable from the expression.

**Rules for K-map grouping:**
1. Groups must contain 1, 2, 4, 8, or 16 cells (powers of 2)
2. Groups should be as large as possible
3. Groups can wrap around the edges
4. Every 1 must be in at least one group
5. 0s are never grouped

**2-Variable K-map layout:**

```
        B=0   B=1
A=0  |  00  |  01  |
A=1  |  10  |  11  |
```

**4-Variable K-map layout (most common in exams):**

```
          CD
AB    00  01  11  10
00  |  0 | 1 | 3 | 2 |
01  |  4 | 5 | 7 | 6 |
11  | 12 |13 |15 |14 |
10  |  8 | 9 |11 |10 |
```

> ⚠️ **Warning:** The column order is **00, 01, 11, 10** — NOT 00, 01, 10, 11. This is **Gray code** order. Adjacent cells differ by exactly 1 bit. Getting this wrong invalidates your entire K-map.

---

### 🔢 Step-by-Step Procedures

#### SOP (Sum of Products) Minimization via K-map

```
Problem: Minimize F(A,B,C,D) = Σm(0,1,2,3,8,9,10,11)
(Σm means sum of minterms — the positions where F=1)

Step 1: Fill in the K-map (put 1s in positions 0,1,2,3,8,9,10,11)

          CD
AB    00  01  11  10
00  |  1 | 1 | 1 | 1 |   ← row AB=00: all 1s
01  |  0 | 0 | 0 | 0 |
11  |  0 | 0 | 0 | 0 |
10  |  1 | 1 | 1 | 1 |   ← row AB=10: all 1s

Step 2: Find largest possible groups of 1s
  Group 1: All 8 cells in rows AB=00 and AB=10
  These rows both have A=0 (row 00) and A=1 (row 10)... wait:
  AB=00 means A=0,B=0; AB=10 means A=1,B=0
  Both have B=0 → This group is where B=0
  The group covers all CD values → CD cancels
  Remaining variable: B=0 → term = B̄

Step 3: Write minimized expression
  F = B̄
```

---

### ⚡ Why This Matters

K-maps and Boolean minimization directly impact hardware efficiency. Fewer gates = less power, less heat, less chip area. In embedded systems (like those on satellites), minimizing logic can be the difference between a circuit that fits on a chip and one that doesn't. Every digital circuit starts with Boolean expressions and ends with minimized gate arrays.

---

### 🧪 Lab Prep Exercises

**Exercise 5.1** — Create a truth table for F = AB + ĀC

<details>
<summary>Show Solution</summary>

| A | B | C | Ā | AB | ĀC | F = AB + ĀC |
|---|---|---|---|----|----|------------|
| 0 | 0 | 0 | 1 | 0  | 0  | 0 |
| 0 | 0 | 1 | 1 | 0  | 1  | 1 |
| 0 | 1 | 0 | 1 | 0  | 0  | 0 |
| 0 | 1 | 1 | 1 | 0  | 1  | 1 |
| 1 | 0 | 0 | 0 | 0  | 0  | 0 |
| 1 | 0 | 1 | 0 | 0  | 0  | 0 |
| 1 | 1 | 0 | 0 | 1  | 0  | 1 |
| 1 | 1 | 1 | 0 | 1  | 0  | 1 |

F = 1 at rows: 1, 3, 6, 7 (0-indexed)

</details>

---

**Exercise 5.2** — Apply De Morgan's Law to simplify: F = ̄(A + B̄)

<details>
<summary>Show Solution</summary>

```
F = ̄(A + B̄)

Apply De Morgan: ̄(X + Y) = X̄ · Ȳ
  X = A,  Y = B̄
  X̄ = Ā, Ȳ = ̄(B̄) = B (double negation)

F = Ā · B
```

</details>

---

**Exercise 5.3** — Minimize using K-map: F(A,B,C) = Σm(1,3,5,7)

<details>
<summary>Show Solution</summary>

```
3-variable K-map:

         BC
A    00  01  11  10
0  |  0 | 1 | 1 | 0 |
1  |  0 | 1 | 1 | 0 |

Groups:
- Column BC=01: both 1s → A varies, B=0, C=1 → term: B̄C
  Wait — let me re-examine. BC=01 means B=0,C=1.
- Column BC=11: both 1s → A varies, B=1, C=1 → term: BC

Actually these two columns (01 and 11) can be grouped together:
- Columns 01 and 11 cover all 4 cells where C=1
- A varies (0 and 1), B varies (0 and 1), C=1

Group: all 4 cells where C=1

Minimized: F = C
```

</details>

---

### 📝 Exam Quick-Fire

**Q1.** De Morgan's Law states that ̄(A·B) equals:

- A) Ā · B̄
- B) Ā + B̄
- C) A + B
- D) A · B

**Q2.** The XOR gate outputs 1 when:

- A) Both inputs are 1
- B) Both inputs are 0
- C) The inputs are different
- D) At least one input is 1

**Q3.** In a Karnaugh map, groups must contain how many cells?

- A) Any number
- B) An even number
- C) A power of 2 (1, 2, 4, 8...)
- D) A prime number

**Q4.** Which gate is considered "universal" (can implement any Boolean function)?

- A) AND
- B) OR
- C) XOR
- D) NAND

**Q5.** The Boolean expression A + A equals:

- A) A²
- B) 2A
- C) A
- D) 1

<details>
<summary>Show Answers</summary>

1. **B** — De Morgan: break the bar, change the sign. ̄(AB) = Ā + B̄.
2. **C** — XOR = "exclusive or" = 1 only when inputs differ.
3. **C** — K-map groups must be powers of 2 (1, 2, 4, 8, 16) for proper variable elimination.
4. **D** — NAND is universal: all other gates can be constructed from NAND gates alone.
5. **C** — Idempotent law: A + A = A (not 2A — Boolean algebra has no "2").

</details>

---

### 🔁 Chapter Summary

- **AND**: output 1 only if all inputs are 1; **OR**: output 1 if any input is 1; **NOT**: flips input
- **NAND** = NOT AND; **NOR** = NOT OR; **XOR** = "different inputs"
- **NAND is universal**: any circuit can be built from NAND gates alone
- **De Morgan's Law**: ̄(A+B) = Ā·B̄ and ̄(A·B) = Ā+B̄; "break bar, change sign"
- **K-map groups** must be powers of 2; use Gray code ordering (00,01,11,10)
- **SOP** (Sum of Products) = OR of AND terms; derived from positions where output = 1
- Larger K-map groups = simpler (fewer variables in) the resulting term
- Boolean minimization reduces gate count → saves power, chip area, cost

---

### 🔗 Connections to Other Chapters

Boolean algebra is the mathematical foundation of **Chapter 6** (Digital Circuits) — every multiplexer, flip-flop, and counter is built from these gates. It connects back to **Chapter 2** (binary 0s and 1s are the inputs), and forward to **Chapter 7** (Assembly logical operations like AND, OR, XOR work on bits following these exact rules).

---

<!-- VERSION: 1.0 | Chapter 6 | 2025 -->

## Chapter 6: Digital Circuits

### 🚀 The Hook

The entire memory system of your computer — every byte of RAM, every register in the CPU — is made of flip-flops: tiny circuits that can "remember" a single bit. Chain enough of them together with some logic gates and you have a processor. This chapter is where Boolean algebra leaves the textbook and becomes actual hardware.

---

### 📌 What You'll Learn

- Explain how a multiplexer routes data and design a 4-to-1 MUX
- Describe the difference between a latch and a flip-flop
- Trace the state sequence of a 3-bit binary counter
- Explain the role of tri-state gates in a shared data bus
- Connect flip-flops to form a shift register

---

### 🧠 Core Concepts

#### Multiplexers (MUX)

**Multiplexer** — A circuit that selects ONE of many inputs and routes it to a single output, based on select lines.

*Analogy:* A TV remote control. Many channels (inputs) exist, but the selection buttons (select lines) determine which one appears on screen (output).

```
4-to-1 MUX:
Inputs: I0, I1, I2, I3
Select: S1, S0 (2 bits → 4 combinations)
Output: Y

S1 S0 → Y
 0  0  → I0
 0  1  → I1
 1  0  → I2
 1  1  → I3

Boolean: Y = (S̄1·S̄0·I0) + (S̄1·S0·I1) + (S1·S̄0·I2) + (S1·S0·I3)
```

**Demultiplexer (DEMUX)** — The reverse: one input routes to one of many outputs. Like a post office sorting mail to different boxes.

---

#### Flip-Flops — The Memory of Digital Logic

**Latch** — A level-triggered bistable circuit. Changes output while enable signal is active.
**Flip-flop** — An edge-triggered bistable circuit. Changes output only on a clock edge (rising or falling).

*Analogy:* A latch is like a door that's open while you hold the handle. A flip-flop is like a door that only opens when you knock (clock pulse) — no matter how long you hold the handle, it only responds at the knock.

**D Flip-Flop (most important for exams):**

| Clock Edge | D | Q (next) |
|------------|---|----------|
| ↑ (rising) | 0 | 0 |
| ↑ (rising) | 1 | 1 |
| No edge | X | Q (unchanged) |

The D flip-flop simply *captures* the value of D on the rising clock edge. This is the building block of registers.

**SR Latch:**

| S | R | Q | Q̄ |
|---|---|---|---|
| 0 | 0 | Q | Q̄ (hold) |
| 0 | 1 | 0 | 1 (reset) |
| 1 | 0 | 1 | 0 (set) |
| 1 | 1 | ? | ? (forbidden!) |

> ⚠️ **Warning:** S=1, R=1 is the **forbidden state** for an SR latch — it produces an undefined, race-condition output. This is why D and JK flip-flops were invented.

---

#### Registers

**Register** — A group of flip-flops storing multiple bits simultaneously. An 8-bit register = 8 D flip-flops sharing the same clock.

*Analogy:* One flip-flop is a single parking space; a register is a car park with multiple spaces, all under the same security camera (clock).

```
8-bit Register:
D7 D6 D5 D4 D3 D2 D1 D0  ← Data inputs
↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓
[FF][FF][FF][FF][FF][FF][FF][FF]  ← 8 D flip-flops
↑  ↑  ↑  ↑  ↑  ↑  ↑  ↑
     Clock (shared by all)
Q7 Q6 Q5 Q4 Q3 Q2 Q1 Q0  ← Stored output
```

**Shift Register** — Data shifts left or right on each clock pulse. Used for serial-to-parallel conversion, multiplication/division by 2.

---

#### Counters

**Binary Counter** — A register whose value increments (or decrements) on each clock pulse.

**3-bit binary counter sequence:**

```
State: 000 → 001 → 010 → 011 → 100 → 101 → 110 → 111 → 000 (wraps)
Dec:    0  →  1  →  2  →  3  →  4  →  5  →  6  →  7  →  0
```

*Analogy:* An odometer. Counts up, and when it hits the maximum, wraps back to zero.

---

#### Tri-State Gates and Data Buses

**Tri-state gate** — A gate with three output states: 0, 1, and **High-Impedance (Z)** (effectively disconnected).

*Analogy:* A light switch with an extra "disconnected" position. When in high-Z, the output doesn't load the bus at all — as if the wire isn't there.

**Why this matters:** Multiple devices share a single data bus. Without tri-state logic, two devices driving the bus simultaneously would cause a short circuit. Tri-state gates ensure only ONE device drives the bus at any time.

```
Device A output: --- [Tri-state] ---+--- [Data Bus] --- CPU
Device B output: --- [Tri-state] --/

Only one tri-state enabled at a time!
```

---

### 🔢 Step-by-Step Procedures

#### Designing a 2-to-1 MUX from Gates

```
2-to-1 MUX: Y = S̄·I0 + S·I1

Step 1: When S=0: Y = I0  (pass I0 through)
Step 2: When S=1: Y = I1  (pass I1 through)

Implementation:
  Gate 1: NOT(S) = S̄
  Gate 2: AND(S̄, I0) = S̄·I0
  Gate 3: AND(S,  I1) = S·I1
  Gate 4: OR(Gate2, Gate3) = Y
```

#### Tracing a D Flip-Flop Sequence

```
Initial state: Q = 0
Clock pulses and D inputs:

Clock 1: D=1 → on ↑ edge: Q becomes 1
Clock 2: D=1 → on ↑ edge: Q becomes 1 (no change)
Clock 3: D=0 → on ↑ edge: Q becomes 0
Clock 4: D=1 → on ↑ edge: Q becomes 1

Sequence: 0, 1, 1, 0, 1
```

---

### ⚡ Why This Matters

Every RAM chip, CPU register, and cache line is made of flip-flops. Multiplexers route data inside ALUs. Shift registers are used in serial communication protocols like I2C and SPI (Chapter 11). Counters appear in timer circuits, memory addressing, and clock dividers in every microcontroller you'll ever program.

---

### 🧪 Lab Prep Exercises

**Exercise 6.1** — Complete the truth table for a 2-to-1 MUX with inputs I0=1, I1=0, for both values of S.

<details>
<summary>Show Solution</summary>

| S | I0 | I1 | Y |
|---|----|----|---|
| 0 | 1  | 0  | 1 |  (S=0 → pass I0 = 1)
| 1 | 1  | 0  | 0 |  (S=1 → pass I1 = 0)

</details>

---

**Exercise 6.2** — An SR latch has inputs S=0, R=0 with current Q=1. What is Q after the inputs are applied?

<details>
<summary>Show Solution</summary>

S=0, R=0 is the HOLD state. Q remains unchanged = 1.

</details>

---

**Exercise 6.3** — Trace a 3-bit binary up-counter for 10 clock pulses starting from 000.

<details>
<summary>Show Solution</summary>

```
Clock:  0    1    2    3    4    5    6    7    8    9   10
State: 000  001  010  011  100  101  110  111  000  001  010
Dec:    0    1    2    3    4    5    6    7    0    1    2
```
Note the wrap-around at clock 8 (back to 000).

</details>

---

### 📝 Exam Quick-Fire

**Q1.** A 4-to-1 multiplexer requires how many select lines?

- A) 1
- B) 2
- C) 3
- D) 4

**Q2.** The forbidden state in an SR latch occurs when:

- A) S=0, R=0
- B) S=0, R=1
- C) S=1, R=0
- D) S=1, R=1

**Q3.** A D flip-flop changes its output:

- A) Whenever D changes
- B) Only on a clock edge
- C) Only when the enable is high
- D) Continuously

**Q4.** Tri-state gates are used primarily to:

- A) Implement XOR functions
- B) Allow multiple devices to share a single bus
- C) Double the clock speed
- D) Create binary counters

**Q5.** A 4-bit binary counter that wraps around has how many distinct states?

- A) 4
- B) 8
- C) 16
- D) 32

<details>
<summary>Show Answers</summary>

1. **B** — 2 select lines: 2² = 4 possible inputs. n inputs requires log₂(n) select lines.
2. **D** — S=R=1 is forbidden in SR latch (both outputs try to be 1 and 0 simultaneously).
3. **B** — Flip-flops are edge-triggered: they only sample D on a clock edge (rising or falling).
4. **B** — Tri-state outputs can be "disconnected" (high-Z), allowing bus sharing without contention.
5. **C** — 4 bits = 2⁴ = 16 states (0000 through 1111).

</details>

---

### 🔁 Chapter Summary

- **MUX**: selects one of n inputs based on select lines; n inputs → log₂(n) selects
- **DEMUX**: routes one input to one of n outputs
- **SR Latch**: level-triggered; S=R=1 is forbidden
- **D Flip-Flop**: captures D on clock edge; the universal memory element
- **Register**: group of D flip-flops sharing a clock; stores multiple bits
- **Shift Register**: shifts bits left/right each clock; used for serial data conversion
- **Binary Counter**: increments each clock pulse; n bits → 2ⁿ states
- **Tri-state gate**: adds high-Z output; essential for bus-sharing between multiple devices

---

### 🔗 Connections to Other Chapters

Digital circuits implement the processor architecture described in **Chapter 1** and the memory hierarchy in **Chapter 8**. The data bus using tri-state gates connects to **Chapter 11** (Interfaces). Registers here are exactly the CPU registers you'll program in **Chapter 7** (Assembly).

---

<!-- VERSION: 1.0 | Chapter 7 | 2025 -->

## Chapter 7: MASM32 Assembly Programming

### 🚀 The Hook

When security researchers reverse-engineer malware, or when game developers squeeze the last drop of performance from a CPU, they work in assembly. Assembly is the closest you can get to the metal — one instruction, one CPU operation. Every high-level language (C, Python, Java) compiles down to instructions almost exactly like these. Reading assembly is like reading the CPU's diary.

---

### 📌 What You'll Learn

- Write and annotate a complete MASM32 skeleton program
- Use MOV, ADD, SUB, MUL, DIV for arithmetic in registers
- Apply AND, OR, XOR, NOT for bit manipulation
- Use CMP with conditional jumps (JE, JNE, JL, JG) to implement `if` logic
- Read the flag register and explain when each flag is set

---

### 🧠 Core Concepts

#### The x86 Register Set

*Analogy:* Registers are the chef's hands — the only place where actual cooking (computation) happens. You can't add two numbers that are still in the fridge (memory); you must bring them to your hands (registers) first.

**General-Purpose Registers (32-bit):**

| Register | Full Name | Primary Use | Sub-registers |
|----------|-----------|-------------|---------------|
| EAX | Accumulator | Arithmetic, return values | AX, AH, AL |
| EBX | Base | Memory base addressing | BX, BH, BL |
| ECX | Counter | Loop counter | CX, CH, CL |
| EDX | Data | I/O, MUL/DIV overflow | DX, DH, DL |
| ESI | Source Index | Source pointer for string ops | SI |
| EDI | Destination Index | Destination pointer | DI |
| ESP | Stack Pointer | Points to top of stack | SP |
| EBP | Base Pointer | Stack frame reference | BP |

**Register Size Breakdown (EAX example):**
```
|       EAX (32-bit)          |
|   high    |      AX (16-bit)|
|           |  AH   |   AL    |
|           | 8-bit | 8-bit   |
```

**Special Registers:**

| Register | Name | Purpose |
|----------|------|---------|
| EIP | Instruction Pointer | Address of next instruction (= PC) |
| EFLAGS | Flags Register | Condition codes after operations |

---

#### The Flag Register (EFLAGS)

**Flag** — A single bit in EFLAGS that records something that happened during the last operation.

| Flag | Symbol | Set When... |
|------|--------|------------|
| Zero Flag | ZF | Result = 0 |
| Sign Flag | SF | Result is negative (MSB = 1) |
| Carry Flag | CF | Unsigned overflow / borrow |
| Overflow Flag | OF | Signed overflow |
| Parity Flag | PF | Result has even number of 1 bits |

> 💡 **Exam tip:** CMP subtracts without storing the result — it ONLY sets flags. Then a conditional jump reads those flags.

---

#### Instruction Reference Table

| Instruction | Syntax | What It Does |
|-------------|--------|-------------|
| MOV | `MOV dest, src` | Copy src into dest |
| ADD | `ADD dest, src` | dest = dest + src |
| SUB | `SUB dest, src` | dest = dest − src |
| MUL | `MUL src` | EAX = EAX × src (unsigned) |
| DIV | `DIV src` | EAX = EAX / src; EDX = remainder |
| AND | `AND dest, src` | dest = dest AND src (bitwise) |
| OR | `OR dest, src` | dest = dest OR src (bitwise) |
| XOR | `XOR dest, src` | dest = dest XOR src |
| NOT | `NOT dest` | dest = bitwise NOT of dest |
| CMP | `CMP a, b` | Compute a − b, set flags only |
| JMP | `JMP label` | Unconditional jump |
| JE / JZ | `JE label` | Jump if ZF=1 (equal / zero) |
| JNE / JNZ | `JNE label` | Jump if ZF=0 (not equal) |
| JL / JNGE | `JL label` | Jump if less than (signed) |
| JG / JNLE | `JG label` | Jump if greater than (signed) |
| PUSH | `PUSH src` | Push src onto stack; ESP -= 4 |
| POP | `POP dest` | Pop stack top into dest; ESP += 4 |
| CALL | `CALL proc` | Push EIP, jump to proc |
| RET | `RET` | Pop return address into EIP |
| INT | `INT n` | Software interrupt (e.g., INT 21h for DOS) |

---

#### Annotated Skeleton Program

```asm
; ============================================================
; MASM32 Skeleton Program
; Description: Basic template — computes 5 + 3 and stores result
; ============================================================

.386                        ; Target 80386 or higher processor
.model flat, stdcall        ; Flat memory model; stdcall calling convention
.stack 4096                 ; Reserve 4KB for the stack

; ----- DATA SEGMENT -----------------------------------------
.data
    num1   DWORD 5          ; First number (32-bit variable)
    num2   DWORD 3          ; Second number
    result DWORD 0          ; Will store the sum

; ----- CODE SEGMENT -----------------------------------------
.code

main PROC                   ; Start of main procedure
    ; --- Load values into registers ---
    MOV EAX, num1           ; EAX = 5
    MOV EBX, num2           ; EBX = 3

    ; --- Perform addition ---
    ADD EAX, EBX            ; EAX = EAX + EBX = 8

    ; --- Store result ---
    MOV result, EAX         ; Save result to memory variable

    ; --- Exit program ---
    INVOKE ExitProcess, 0   ; Call Windows API to exit cleanly

main ENDP                   ; End of main procedure
END main                    ; Entry point = main
```

---

#### A Conditional Branch Example

```asm
; If EAX > 10, jump to "big"; otherwise, continue

MOV EAX, 15
CMP EAX, 10         ; Compute 15 - 10, set flags
JG  big             ; Jump if EAX > 10 (Greater, signed)
; ... code for EAX <= 10 ...
JMP done

big:
; ... code for EAX > 10 ...

done:
```

---

#### XOR Trick — Zero a Register

```asm
XOR EAX, EAX    ; EAX = 0 (any value XOR itself = 0)
                ; Faster and smaller than MOV EAX, 0
```

> 💡 **Tip:** `XOR EAX, EAX` is the idiomatic way to zero a register in x86 assembly. You'll see this in disassembled code constantly.

---

### 🔢 Step-by-Step Procedures

#### Writing an IF-ELSE in Assembly

```
High-level code:
  if (EAX == 5) {
      EBX = 1;
  } else {
      EBX = 0;
  }

Assembly translation:
Step 1: Compare EAX to 5
  CMP EAX, 5

Step 2: Jump to "else" if NOT equal
  JNE else_branch

Step 3: Then-branch (EAX == 5)
  MOV EBX, 1
  JMP end_if          ; Skip else

Step 4: else_branch label
else_branch:
  MOV EBX, 0

end_if:               ; Continue here
```

#### Multiply Two Numbers

```
Compute EAX = 6 × 7

Step 1: Load first number
  MOV EAX, 6

Step 2: Load second number into a register/memory
  MOV ECX, 7

Step 3: MUL uses implicit EAX
  MUL ECX             ; EAX = EAX × ECX = 42
                      ; (for large results, high 32 bits go into EDX)
Result: EAX = 42
```

---

### ⚡ Why This Matters

Assembly is the language of operating system kernels, device drivers, bootloaders, and exploit development. Embedded systems in rockets, medical devices, and industrial equipment are often written in assembly for predictable timing. Even if you never write assembly professionally, reading disassembled code is a core skill in cybersecurity and performance optimization.

---

### 🧪 Lab Prep Exercises

**Exercise 7.1** — Write MASM32 code to compute (A + B) × C where A=4, B=6, C=3. Store the result in EDX.

<details>
<summary>Show Solution</summary>

```asm
MOV EAX, 4      ; A = 4
MOV EBX, 6      ; B = 6
ADD EAX, EBX    ; EAX = A + B = 10
MOV ECX, 3      ; C = 3
MUL ECX         ; EAX = EAX × ECX = 30
MOV EDX, EAX    ; Store result in EDX
; Result: EDX = 30
```

</details>

---

**Exercise 7.2** — What flags are set after `CMP EAX, EAX` (comparing a register to itself)?

<details>
<summary>Show Solution</summary>

CMP subtracts: EAX - EAX = 0.

- **ZF = 1** (result is zero)
- **CF = 0** (no borrow)
- **OF = 0** (no signed overflow)
- **SF = 0** (result is not negative)

This is why `JE` (Jump if Equal) checks ZF=1.

</details>

---

**Exercise 7.3** — Write MASM32 code to clear (zero) bits 3 and 4 of EAX without affecting other bits.

<details>
<summary>Show Solution</summary>

```asm
; Bit 3 = 00001000 = 0x08
; Bit 4 = 00010000 = 0x10
; To clear specific bits: AND with a mask where those bits are 0

AND EAX, 0xFFFFFFE7    ; 0xE7 = 11100111, clears bits 3 and 4

; Alternative using NOT:
; Mask = 0x18 = 00011000
; NOT mask = 11100111
AND EAX, NOT 18h       ; Same result
```

</details>

---

### 📝 Exam Quick-Fire

**Q1.** Which register implicitly holds the first operand in a MUL instruction?

- A) EBX
- B) ECX
- C) EAX
- D) EDX

**Q2.** The instruction `XOR EAX, EAX` is commonly used to:

- A) Toggle all bits in EAX
- B) Set EAX to -1
- C) Set EAX to 0
- D) Copy EAX to memory

**Q3.** The Zero Flag (ZF) is set when:

- A) The result is positive
- B) The result is zero
- C) There is a carry
- D) There is a signed overflow

**Q4.** What does `CMP EAX, EBX` do?

- A) Stores EAX − EBX in EAX
- B) Stores EBX − EAX in EBX
- C) Computes EAX − EBX and sets flags only (no storage)
- D) Swaps EAX and EBX

**Q5.** In x86 assembly, the instruction pointer register is:

- A) ESP
- B) EBP
- C) EIP
- D) EAX

<details>
<summary>Show Answers</summary>

1. **C** — MUL always uses EAX as the implicit first operand. Result goes to EDX:EAX.
2. **C** — XOR reg,reg = 0. Any value XOR itself = 0. Faster than MOV reg, 0.
3. **B** — ZF=1 when arithmetic result is exactly zero.
4. **C** — CMP = subtract and set flags, no result stored. Used before conditional jumps.
5. **C** — EIP = Extended Instruction Pointer = the PC in x86.

</details>

---

### 🔁 Chapter Summary

- **EAX** = accumulator (arithmetic + return values); **ECX** = counter; **ESP** = stack pointer
- **MOV** copies data; **ADD/SUB** arithmetic; **MUL/DIV** use EAX implicitly
- **AND/OR/XOR/NOT** = bitwise logical ops; XOR reg,reg = fastest way to zero a register
- **CMP** = subtract without storing — only sets flags; always followed by a conditional jump
- **ZF**: result=0; **CF**: unsigned carry/borrow; **SF**: negative result; **OF**: signed overflow
- **JMP** unconditional; **JE/JNE/JL/JG** test flags set by CMP
- **PUSH/POP** manage the stack; **CALL/RET** implement procedure calls
- The MASM32 skeleton needs: `.386`, `.model flat,stdcall`, `.data`, `.code`, `main PROC/ENDP`, `END main`

---

### 🔗 Connections to Other Chapters

Assembly directly implements the FDE cycle from **Chapter 1** — each instruction is one fetch-decode-execute iteration. The registers here are the CPU registers from **Chapter 6** (digital flip-flop arrays). The flag register uses Boolean logic from **Chapter 5**. Memory access (`MOV EAX, [address]`) connects to the memory hierarchy of **Chapter 8**.

---

<!-- VERSION: 1.0 | Chapter 8 | 2025 -->

## Chapter 8: Memory Systems

### 🚀 The Hook

The difference between a fast computer and a slow one often has nothing to do with the CPU — it's the memory. The CPU in a modern laptop can process billions of operations per second, but DRAM only delivers data at a fraction of that rate. Computer architects have spent 60 years designing elaborate memory hierarchies to hide this gap. Understanding memory is understanding performance.

---

### 📌 What You'll Learn

- Draw and explain the memory hierarchy pyramid with correct speed/cost/size tradeoffs
- Distinguish between DRAM, SRAM, ROM, EPROM, and Flash memory
- Explain how DMA (Direct Memory Access) frees the CPU from data transfers
- Describe the interrupt mechanism and trace an interrupt service routine
- Calculate memory capacity from address bus width

---

### 🧠 Core Concepts

#### The Memory Hierarchy

*Analogy:* Think of memory as a city's supply chain. Registers are the tools in a carpenter's hands (fast, tiny, expensive). Cache is the toolbox on the workbench (fast, small). RAM is the hardware store down the street (slower, much bigger). Disk is the warehouse across town (very slow, massive, cheap).

```
        /\
       /  \   Registers (< 1ns, bytes)
      /----\
     /      \  Cache L1/L2/L3 (1–10ns, KB–MB)
    /--------\
   /          \ DRAM / RAM (50–100ns, GB)
  /------------\
 /              \ SSD / HDD (μs–ms, TB)
/________________\
```

**Key principle:** The closer to the CPU, the faster and more expensive per byte, but the smaller the capacity.

---

#### RAM Types

**DRAM (Dynamic RAM)** — The main system memory.

- Stores bits as charge in capacitors
- Must be *refreshed* thousands of times per second (capacitors leak)
- Slower but very cheap — used for main memory (GBs)

**SRAM (Static RAM)** — Used for cache memory.

- Stores bits using flip-flop circuits
- No refresh needed
- 4–6 transistors per bit (vs. 1 for DRAM) → more expensive, smaller, faster

| Property | DRAM | SRAM |
|----------|------|------|
| Speed | Slow (50–100 ns) | Fast (1–10 ns) |
| Cost | Low | High |
| Density | High (GB chips) | Low (MB chips) |
| Refresh | Yes (required) | No |
| Use | Main RAM | Cache |

---

#### ROM Types

**ROM (Read-Only Memory)** — Non-volatile; retains data without power.

| Type | Programmable? | Erasable? | Use Case |
|------|--------------|-----------|---------|
| ROM | No (factory) | No | Fixed firmware (ancient) |
| PROM | Once (field) | No | One-time programming |
| EPROM | Yes | UV light | Reusable development firmware |
| EEPROM | Yes (byte) | Electrically | Config storage |
| Flash | Yes (block) | Electrically | SSDs, USB drives, BIOS |

> 🚀 **Real World:** The BIOS/UEFI firmware in your computer is stored in Flash memory. So is every smartphone's OS. The Mars rovers use EEPROM/Flash for non-volatile mission-critical parameter storage.

---

#### Memory Addressing

**Memory capacity formula:**
```
Number of addressable locations = 2^(number of address lines)
Each location holds N bits (the data bus width)

Total memory = 2^(address lines) × data_bus_width

Example: 16 address lines, 8-bit data bus
  Locations = 2^16 = 65,536 = 64K
  Total bits = 64K × 8 = 512 Kbits = 64 KB
```

---

#### Direct Memory Access (DMA)

*Analogy:* Without DMA, the CPU is like a manager who personally photocopies every document — slow and wasteful. With DMA, the manager delegates to an assistant (DMA controller) who handles all the copying while the manager does more important work.

**DMA process:**
1. CPU programs the DMA controller with: source address, destination address, byte count
2. CPU releases the bus to the DMA controller (CPU may halt or do other work)
3. DMA controller transfers data directly between memory and I/O device
4. DMA controller signals CPU via **interrupt** when transfer is complete
5. CPU resumes normal operation

---

#### Interrupts

**Interrupt** — A signal that stops the CPU's current work to handle an urgent event.

*Analogy:* You're cooking (running a program). The doorbell rings (interrupt). You pause cooking, go to the door, handle whatever it is (Interrupt Service Routine), then return to cooking (resume program) from exactly where you left off.

**Interrupt sequence:**
```
Step 1: Device raises interrupt signal
Step 2: CPU finishes current instruction
Step 3: CPU saves its state (pushes registers/flags to stack)
Step 4: CPU looks up ISR address in Interrupt Vector Table
Step 5: CPU jumps to ISR (Interrupt Service Routine)
Step 6: ISR runs (handles the device)
Step 7: IRET instruction: CPU restores state from stack
Step 8: CPU resumes original program
```

**Types of interrupts:**

| Type | Cause | Example |
|------|-------|---------|
| Hardware | External device signal | Keyboard press, DMA complete |
| Software (Trap) | INT instruction | INT 21h (DOS), system calls |
| Exception | CPU error | Division by zero, page fault |

---

### 🔢 Step-by-Step Procedures

#### Calculating Memory Size from Bus Width

```
Problem: A system has 20 address lines and a 16-bit data bus.
What is the total memory capacity in KB?

Step 1: Number of locations = 2^20 = 1,048,576 = 1M locations
Step 2: Each location = 16 bits = 2 bytes
Step 3: Total = 1M × 2 bytes = 2 MB = 2048 KB

Answer: 2 MB
```

---

### ⚡ Why This Matters

Memory hierarchy design is why your computer doesn't feel like it runs at DRAM speed. Cache memory (Chapter 9) bridges the gap. DMA is how your SSD, network card, and GPU transfer data without constantly bothering the CPU. Interrupt handling is the core mechanism behind every OS event loop — mouse clicks, keystrokes, timer events. This is the plumbing of computing.

---

### 🧪 Lab Prep Exercises

**Exercise 8.1** — A system has 24 address lines and an 8-bit data bus. What is the total memory capacity in megabytes?

<details>
<summary>Show Solution</summary>

```
Locations = 2^24 = 16,777,216 = 16M locations
Data width = 8 bits = 1 byte per location
Total = 16M × 1 = 16 MB
```

</details>

---

**Exercise 8.2** — List the memory hierarchy from fastest to slowest, with a typical access time for each level.

<details>
<summary>Show Solution</summary>

1. Registers: < 1 ns (inside CPU)
2. L1 Cache: ~1–4 ns (on-chip)
3. L2 Cache: ~4–10 ns (on-chip or near-chip)
4. L3 Cache: ~20–40 ns (shared on-chip)
5. DRAM (Main Memory): ~50–100 ns
6. SSD: ~50–200 μs
7. HDD: ~5–20 ms

</details>

---

**Exercise 8.3** — Describe what happens in the processor when a keyboard interrupt occurs during execution of a loop.

<details>
<summary>Show Solution</summary>

1. Keyboard controller raises an IRQ signal on the interrupt line.
2. CPU completes its current instruction (doesn't stop mid-instruction).
3. CPU saves the current Program Counter (EIP) and EFLAGS register to the stack.
4. CPU reads the interrupt vector table to find the address of the keyboard ISR.
5. CPU jumps to the keyboard ISR.
6. ISR reads the keycode from the keyboard controller's data port.
7. ISR uses IRET to restore EIP and EFLAGS from the stack.
8. CPU continues executing the loop from exactly where it paused.

</details>

---

### 📝 Exam Quick-Fire

**Q1.** Which type of RAM requires periodic refreshing?

- A) SRAM
- B) DRAM
- C) ROM
- D) Flash

**Q2.** DMA (Direct Memory Access) is used primarily to:

- A) Speed up CPU arithmetic
- B) Transfer data between memory and I/O without constant CPU involvement
- C) Increase the size of the cache
- D) Replace the need for interrupts

**Q3.** A system with 16 address lines can address how many unique memory locations?

- A) 16
- B) 256
- C) 65,536
- D) 1,048,576

**Q4.** During an interrupt, what is the first thing the CPU saves before jumping to the ISR?

- A) The contents of the data bus
- B) Its current state (registers, PC, flags)
- C) The memory address being accessed
- D) The interrupt vector table

**Q5.** Which memory type is used for a computer's BIOS/UEFI firmware today?

- A) DRAM
- B) SRAM
- C) Flash
- D) PROM

<details>
<summary>Show Answers</summary>

1. **B** — DRAM stores bits as capacitor charge which leaks; must be refreshed every ~64ms.
2. **B** — DMA handles bulk transfers (disk, network, audio), freeing the CPU for computation.
3. **C** — 2^16 = 65,536 = 64K locations.
4. **B** — CPU must save its complete state (PC, flags, key registers) so it can restore them with IRET.
5. **C** — Flash memory is non-volatile, electrically erasable, and rewritable — perfect for BIOS.

</details>

---

### 🔁 Chapter Summary

- **Memory hierarchy**: Registers → Cache → RAM → Disk (fast/expensive/small → slow/cheap/large)
- **DRAM**: main RAM; slow, cheap, high density; needs refresh; used for GBs of system memory
- **SRAM**: cache memory; fast, expensive, no refresh; 4–6 transistors per bit
- **ROM types**: ROM, PROM, EPROM (UV erase), EEPROM (byte erase), Flash (block erase)
- **Flash**: used for SSDs, BIOS, USB drives, phone storage
- **Memory capacity**: 2^(address lines) locations × data bus width
- **DMA**: CPU delegates bulk transfers to DMA controller; CPU freed for other work
- **Interrupt sequence**: signal → save state → jump to ISR → execute → IRET → resume

---

### 🔗 Connections to Other Chapters

Memory types and hierarchy set up **Chapter 9** (Cache & Virtual Memory) — which exists purely to bridge the speed gap identified here. DMA connects to **Chapter 11** (I/O Interfaces). Interrupts are used in **Chapter 7** (Assembly INT instructions) and underpin every OS scheduler.

---

<!-- VERSION: 1.0 | Chapter 9 | 2025 -->

## Chapter 9: Cache & Virtual Memory

### 🚀 The Hook

In 2020, a major cloud provider's outage took down dozens of major websites. The root cause? A cache invalidation bug. Cache memory is so deeply woven into how computers function that a bug in its management can cascade to failure across thousands of servers. This chapter explains the tricks that make your CPU feel 10× faster than its DRAM speed would suggest.

---

### 📌 What You'll Learn

- Explain direct-mapped, fully associative, and set-associative cache mapping
- Calculate cache hit rate and effective memory access time
- Describe write-through vs. write-back cache policies
- Explain virtual memory, paging, and the role of the TLB
- Distinguish internal vs. external fragmentation

---

### 🧠 Core Concepts

#### Why Cache Exists — Locality of Reference

*Analogy:* A chef doing a recipe will use the same spices repeatedly in a short session. Instead of walking to the pantry (RAM) every time, they keep a small tray (cache) of frequently used items nearby.

Cache exploits two principles:
- **Temporal locality**: recently accessed data will likely be accessed again soon
- **Spatial locality**: data near a recently accessed address will likely be accessed soon

---

#### Cache Mapping Types

**Direct-Mapped Cache**

Each memory block maps to exactly ONE cache line. Simple but can cause conflicts.

```
Cache line = Memory_block_address MOD Cache_size

Memory block 0  → Cache line 0
Memory block 4  → Cache line 0  (conflict!)
Memory block 8  → Cache line 0  (conflict!)
```

**Fully Associative Cache**

Any memory block can go into ANY cache line. Best hit rate, but expensive to search.

**N-Way Set Associative Cache** (most common)

Cache is divided into sets; each set has N lines. A block maps to a specific set, and can go into any of the N lines in that set. Balances between the two extremes.

```
Set = Memory_block_address MOD Number_of_sets
(within that set, any of N ways can hold the block)
```

| Type | Where block goes | Hit rate | Cost |
|------|-----------------|----------|------|
| Direct-mapped | 1 specific line | Low | Low |
| N-way Set Assoc. | 1 specific set, N choices | Medium | Medium |
| Fully Associative | Anywhere | High | High |

---

#### Cache Effectiveness — Hit Rate

**Cache hit**: data found in cache — fast!
**Cache miss**: data not in cache — must fetch from RAM.

```
Hit rate (h) = Cache_hits / Total_memory_accesses

Effective Access Time (EAT):
EAT = h × T_cache + (1 - h) × T_RAM

Example:
  T_cache = 2ns, T_RAM = 100ns, h = 0.95 (95% hit rate)
  EAT = 0.95 × 2 + 0.05 × 100 = 1.9 + 5 = 6.9 ns
```

> 💡 **Insight:** Even a 95% hit rate gives EAT = 6.9ns vs. 100ns for pure RAM — a 14× speedup. This is why cache is transformative.

---

#### Write Policies

**Write-Through**: On every write, update BOTH cache AND RAM simultaneously.
- Simpler, always consistent
- Every write goes to slow RAM → slower writes

**Write-Back**: Write only to cache; update RAM only when the cache line is evicted.
- Faster writes (most writes stay in cache)
- More complex; risk of data loss on crash
- Uses a "dirty bit" to mark modified lines

---

#### Virtual Memory

*Analogy:* A messy desk with limited space (RAM). Virtual memory is like using sticky notes to tell you which drawer (disk) holds which papers — you can have far more "papers" than fit on the desk at once.

**Virtual memory** allows processes to use more memory than physically installed, by swapping pages to disk.

**Key concepts:**

| Concept | Meaning |
|---------|---------|
| Virtual Address | Address used by the program |
| Physical Address | Actual RAM location |
| Page | Fixed-size block of virtual memory (e.g., 4KB) |
| Frame | Fixed-size block of physical RAM (same size as page) |
| Page Table | Maps virtual pages to physical frames |
| TLB | Translation Lookaside Buffer — cache for page table entries |

---

#### Address Translation (Paging)

```
Virtual address split into:
  | Virtual Page Number | Page Offset |

Step 1: VPN looked up in Page Table (or TLB for speed)
Step 2: VPN → Physical Frame Number
Step 3: Physical address = Frame Number + Page Offset
```

**TLB (Translation Lookaside Buffer)** — A small, fast cache for page table entries. Without TLB, every memory access requires two accesses (one for page table, one for data).

---

#### Fragmentation

**Internal Fragmentation**: Wasted space *inside* an allocated block. Occurs when allocated size > needed size (e.g., a 4KB page holds only 1KB of data — 3KB wasted).

**External Fragmentation**: Wasted space *between* allocated blocks. Free memory exists but in scattered, unusable pieces.

*Analogy:* Internal fragmentation is like putting a small photo in a large picture frame — the frame is yours but mostly empty. External fragmentation is like a parking lot where every other space is empty but all are too narrow for your car.

**Segmentation** divides memory into variable-size logical segments (code, data, stack). More natural but susceptible to external fragmentation.

---

### 🔢 Step-by-Step Procedures

#### Effective Access Time Calculation

```
Given: Cache hit time = 5ns, RAM access time = 200ns, hit rate = 90%

EAT = h × T_cache + (1-h) × (T_cache + T_RAM)  [miss: check cache, then RAM]
    = 0.9 × 5 + 0.1 × (5 + 200)
    = 4.5 + 0.1 × 205
    = 4.5 + 20.5
    = 25 ns

(Some exam formulations omit the T_cache from the miss penalty;
 always check which formula your exam uses)
```

#### Virtual → Physical Address Translation

```
Page size = 4KB = 2^12 bytes → 12-bit offset
Virtual address = 0x00003A7F

Step 1: Page offset = lower 12 bits = 0xA7F
Step 2: Virtual page number = upper bits = 0x3
Step 3: Look up VPN 3 in page table → Physical frame = 7 (example)
Step 4: Physical address = 7 × 4096 + 0xA7F = 0x007A7F
```

---

### ⚡ Why This Matters

Cache misses are the #1 cause of CPU stalls in real programs. Performance-sensitive code (games, databases, scientific computing) is often written with cache in mind — keeping data in contiguous arrays, avoiding pointer chasing. Virtual memory enabled modern operating systems where processes can't accidentally overwrite each other's memory. TLB shootdowns in multi-core systems are still a hot research topic.

---

### 🧪 Lab Prep Exercises

**Exercise 9.1** — A cache has a hit rate of 92%. Cache access time is 3ns, RAM access time is 120ns. Calculate the Effective Access Time.

<details>
<summary>Show Solution</summary>

```
EAT = 0.92 × 3 + 0.08 × 120
    = 2.76 + 9.6
    = 12.36 ns
```

</details>

---

**Exercise 9.2** — Explain the difference between write-through and write-back caching and give one advantage of each.

<details>
<summary>Show Solution</summary>

**Write-through**: Every write updates both cache AND RAM simultaneously.
- Advantage: RAM is always consistent with cache; simpler recovery from crashes.

**Write-back**: Writes only update cache; RAM is updated only when the cache line is evicted ("dirty bit" = 1).
- Advantage: Writes are much faster (most writes stay in fast cache, RAM isn't written every time).

</details>

---

**Exercise 9.3** — A virtual memory system uses 4KB pages and 32-bit virtual addresses. How many bits are used for the page offset? For the virtual page number?

<details>
<summary>Show Solution</summary>

```
Page size = 4KB = 2^12 bytes → Page offset = 12 bits
VPN = 32 - 12 = 20 bits
Number of virtual pages = 2^20 = ~1 million pages
```

</details>

---

### 📝 Exam Quick-Fire

**Q1.** In direct-mapped cache, which formula determines which cache line a memory block maps to?

- A) Block address × cache size
- B) Block address MOD cache size
- C) Block address DIV cache size
- D) Block address + cache offset

**Q2.** A cache with 95% hit rate, 4ns cache time, 200ns RAM time has an EAT of approximately:

- A) 4 ns
- B) 14 ns
- C) 104 ns
- D) 200 ns

**Q3.** The Translation Lookaside Buffer (TLB) is:

- A) A type of main memory
- B) A cache for page table entries
- C) A disk sector buffer
- D) A register in the CPU

**Q4.** Internal fragmentation occurs when:

- A) Free memory exists but is too scattered to use
- B) Allocated memory is larger than needed
- C) The page table is full
- D) Two processes access the same physical frame

**Q5.** Write-back caching uses a "dirty bit" to indicate:

- A) The cache line contains corrupted data
- B) The cache line has been modified and differs from RAM
- C) The cache line was recently evicted
- D) The cache line is reserved for OS use

<details>
<summary>Show Answers</summary>

1. **B** — Direct-mapped: cache_line = block_address MOD number_of_cache_lines.
2. **B** — EAT = 0.95×4 + 0.05×200 = 3.8 + 10 = 13.8 ≈ 14ns.
3. **B** — TLB is a small, fast associative cache that stores recent virtual-to-physical translations.
4. **B** — Internal fragmentation = wasted space inside an allocated block.
5. **B** — Dirty bit = 1 means cache line was written but RAM not yet updated; must write back on eviction.

</details>

---

### 🔁 Chapter Summary

- **Cache** exploits temporal and spatial locality to reduce effective memory access time
- Three mapping types: **direct** (1 location), **set associative** (N choices in a set), **fully associative** (anywhere)
- **EAT** = h × T_cache + (1-h) × T_RAM; even 90% hit rate gives massive speedup
- **Write-through**: consistent but slower; **write-back**: faster but needs dirty bits
- **Virtual memory**: programs use virtual addresses; OS/MMU translates to physical via page table
- **Page** = virtual block; **frame** = physical block; both same size (e.g., 4KB)
- **TLB**: caches page table lookups; without it, every access needs 2 memory reads
- **Internal fragmentation**: waste inside allocations; **external**: waste between allocations

---

### 🔗 Connections to Other Chapters

Cache directly solves the Von Neumann bottleneck from **Chapter 1** and relies on SRAM from **Chapter 8**. Virtual memory's page faults trigger interrupts (Chapter 8). The TLB is another cache hierarchy level — layered on top of the memory hierarchy from Chapter 8. This all feeds into **Chapter 10** (Processors), where pipelines and cache work together.

---

<!-- VERSION: 1.0 | Chapter 10 | 2025 -->

## Chapter 10: Processors & Parallel Computing

### 🚀 The Hook

For decades, CPU clock speeds doubled every 18 months — Moore's Law in action. Then around 2005, the clock stopped scaling: we hit a power wall. You can't run a CPU at 10 GHz without it melting. The solution? More cores. The modern era of computing is defined by parallelism, and understanding processor architecture is understanding why your 4-core laptop feels different from a 64-core server running neural networks.

---

### 📌 What You'll Learn

- Explain pipelining and calculate the speedup it provides
- Compare CISC, RISC, and VLIW architectures and give examples of each
- Name key processors in the Intel 80x86 family and their main improvements
- Classify computer architectures using Flynn's Taxonomy (SISD, SIMD, MISD, MIMD)
- Explain the challenges of multi-core computing (cache coherence, parallelism)

---

### 🧠 Core Concepts

#### Pipelining

*Analogy:* A car wash. Without pipelining, each car waits for the previous car to completely finish before starting. With pipelining, when Car 1 is being dried, Car 2 is being rinsed, Car 3 is being soaped — three cars being processed simultaneously, one stage each.

**Classic 5-stage pipeline:**
```
Stage 1: IF  — Instruction Fetch
Stage 2: ID  — Instruction Decode / Register Read
Stage 3: EX  — Execute (ALU operation)
Stage 4: MEM — Memory Access
Stage 5: WB  — Write Back (write result to register)

Clock:    1    2    3    4    5    6    7    8    9
Inst 1:  [IF] [ID] [EX] [MEM][WB]
Inst 2:       [IF] [ID] [EX] [MEM][WB]
Inst 3:            [IF] [ID] [EX] [MEM][WB]
Inst 4:                 [IF] [ID] [EX] [MEM][WB]
Inst 5:                      [IF] [ID] [EX] [MEM][WB]
```

**Pipeline speedup (ideal):**
```
Speedup = Number of pipeline stages
          (for a large number of instructions)

5-stage pipeline → up to 5× speedup over no pipeline
```

**Pipeline hazards** (things that break the ideal):
- **Data hazard**: instruction needs result of previous instruction not yet finished
- **Control hazard**: branch instruction; don't know which instruction to fetch next
- **Structural hazard**: two instructions need the same hardware resource

---

#### CISC vs. RISC vs. VLIW

*Analogy:* CISC is a Swiss Army knife — one tool with many functions. RISC is a set of simple, specialized tools. VLIW is a set of tools designed to be used simultaneously by different hands.

| Property | CISC | RISC | VLIW |
|----------|------|------|------|
| Instruction size | Variable | Fixed | Very wide |
| Instructions | Complex, many | Simple, few | Multiple ops/cycle |
| Cycles/instruction | Multiple | ~1 | 1 (parallel) |
| Compiler | Simple | Complex | Very complex |
| Example | x86, x86-64 | ARM, MIPS | Intel Itanium |
| Registers | Few | Many | Many |
| Memory access | Many instruction types | Load/Store only | Load/Store only |

> 💡 **Insight:** Modern x86 processors are physically RISC inside. The complex x86 (CISC) instructions are translated to simple micro-operations (μops) internally. You get CISC compatibility with RISC performance.

---

#### Intel 80x86 Family Timeline

| Processor | Year | Bits | Key Innovation |
|-----------|------|------|---------------|
| 8086 | 1978 | 16-bit | First x86; segmented memory |
| 80286 | 1982 | 16-bit | Protected mode, 16MB addressable |
| 80386 (i386) | 1985 | 32-bit | Full 32-bit, virtual memory support |
| 80486 | 1989 | 32-bit | On-chip cache, FPU integrated |
| Pentium | 1993 | 32-bit | 64-bit data bus, dual pipeline (superscalar) |
| Pentium Pro | 1995 | 32-bit | Out-of-order execution, 3-way superscalar |
| Pentium 4 | 2000 | 32-bit | NetBurst, deep pipeline, HyperThreading |
| Core 2 | 2006 | 64-bit | Efficient multi-core; end of pure clock-speed race |
| Core i-series | 2008+ | 64-bit | Turbo Boost, integrated GPU, many cores |

---

#### Flynn's Taxonomy

Flynn classified computers by how many instruction streams and data streams they process simultaneously.

| Class | Full Name | Description | Example |
|-------|-----------|-------------|---------|
| SISD | Single Instruction, Single Data | Classic von Neumann; one instruction on one data | Single-core CPU |
| SIMD | Single Instruction, Multiple Data | One instruction applied to many data simultaneously | GPU, SSE/AVX CPU extensions |
| MISD | Multiple Instruction, Single Data | Multiple operations on same data | Fault-tolerant/space systems (rare) |
| MIMD | Multiple Instruction, Multiple Data | Multiple cores, each running different code on different data | Multi-core CPUs, clusters |

> 🚀 **Real World:** When your GPU renders a frame, it uses SIMD/SIMT — thousands of cores running the same shader program on different pixels simultaneously. Deep learning uses the same principle (matrix operations on thousands of data points in parallel).

---

#### Multi-Core and Cache Coherence

**Cache coherence problem:** In a multi-core system, each core has its own cache. If Core 1 modifies a value in its cache, Core 2's cache has an outdated copy — a coherence violation.

**MESI Protocol** (one common solution): Each cache line has a state:
- **M**odified: dirty, only valid copy
- **E**xclusive: clean, only copy
- **S**hared: clean, multiple cores have it
- **I**nvalid: stale, needs to be reloaded

> ⚠️ **Warning:** Cache coherence is why writing multi-threaded code is hard. Data races, memory barriers, and the `volatile` keyword in C all trace back to cache coherence.

---

### 🔢 Step-by-Step Procedures

#### Pipeline Speedup Calculation

```
Without pipeline: each instruction takes 5 cycles (5 stages × 1 cycle each)
Execute 100 instructions:
  Time = 100 × 5 = 500 cycles

With 5-stage pipeline:
  Time = (stages - 1) + instructions = 4 + 100 = 104 cycles
  (Pipeline fill time is (stages-1) cycles, then 1 inst/cycle)

Speedup = 500 / 104 = 4.81×

Ideal speedup → approaches 5× as instruction count grows.
```

---

### ⚡ Why This Matters

Understanding pipelines explains why branch mispredictions are so costly (speculation rollback), why out-of-order CPUs exist, and why modern processors have such deep pipelines. RISC vs. CISC is still live: ARM (RISC) now powers most phones and Apple's M-series MacBooks. The shift to multi-core drove the rise of concurrent programming — the defining challenge of modern software development.

---

### 🧪 Lab Prep Exercises

**Exercise 10.1** — A 4-stage pipeline executes 200 instructions. How many clock cycles does it take? What is the speedup over non-pipelined execution?

<details>
<summary>Show Solution</summary>

```
Pipelined: (4-1) + 200 = 203 cycles
Non-pipelined: 200 × 4 = 800 cycles
Speedup = 800 / 203 = 3.94× (approaches 4× for large N)
```

</details>

---

**Exercise 10.2** — Classify each of the following using Flynn's Taxonomy: (a) A single-core CPU; (b) A GPU rendering pixels; (c) A 16-core server.

<details>
<summary>Show Solution</summary>

(a) **SISD** — Single Instruction, Single Data. One instruction stream, one data stream.
(b) **SIMD** — Single Instruction, Multiple Data. Same shader executed on thousands of pixels simultaneously.
(c) **MIMD** — Multiple Instruction, Multiple Data. Each core runs its own code on its own data independently.

</details>

---

**Exercise 10.3** — What is the difference between CISC and RISC in terms of instructions? Give one example CPU of each type.

<details>
<summary>Show Solution</summary>

**CISC** (Complex Instruction Set Computer): variable-length instructions, many complex instructions (e.g., string copy in one instruction), fewer registers. Example: Intel x86.

**RISC** (Reduced Instruction Set Computer): fixed-length simple instructions (~1 cycle each), explicit load/store for memory, many registers, compiler does more work. Example: ARM Cortex, MIPS, RISC-V.

</details>

---

### 📝 Exam Quick-Fire

**Q1.** In a 5-stage pipeline, how many instructions can be "in flight" simultaneously?

- A) 1
- B) 3
- C) 5
- D) 10

**Q2.** Which Flynn's Taxonomy class describes a GPU processing thousands of pixels with the same shader?

- A) SISD
- B) SIMD
- C) MISD
- D) MIMD

**Q3.** RISC architectures typically:

- A) Have many complex variable-length instructions
- B) Use fewer registers than CISC
- C) Have fixed-length simple instructions taking ~1 cycle
- D) Access memory directly in arithmetic instructions

**Q4.** Which Intel processor first introduced 32-bit protected mode addressing?

- A) 8086
- B) 80286
- C) 80386
- D) 80486

**Q5.** A "data hazard" in pipelining occurs when:

- A) The cache runs out of space
- B) A branch instruction is encountered
- C) An instruction depends on a result not yet produced
- D) Two instructions need the same ALU simultaneously

<details>
<summary>Show Answers</summary>

1. **C** — Each stage handles one instruction; 5 stages = 5 instructions in flight simultaneously.
2. **B** — SIMD: one instruction (the shader), many data streams (one per pixel).
3. **C** — RISC: simple, fixed-length instructions, ~1 cycle each, many registers.
4. **C** — The 80386 (1985) was Intel's first full 32-bit processor with virtual memory support.
5. **C** — Data hazard: Instruction N+1 needs a value that Instruction N is still computing (not yet written back).

</details>

---

### 🔁 Chapter Summary

- **Pipelining** overlaps instruction stages: 5-stage pipeline → up to 5× speedup; real speedup lower due to hazards
- **Hazards**: data (dependency), control (branch), structural (resource conflict)
- **CISC**: complex variable instructions (x86); **RISC**: simple fixed instructions (ARM); **VLIW**: multiple ops/instruction (Itanium)
- **Modern x86** internally converts CISC instructions to RISC micro-ops
- **Intel timeline**: 8086(16-bit) → 386(32-bit) → Pentium(superscalar) → Core(multi-core)
- **Flynn's Taxonomy**: SISD (single core), SIMD (GPU/vector), MISD (rare), MIMD (multi-core)
- **Multi-core** solved the power wall but created cache coherence challenges
- **MESI protocol** manages cache coherence in multi-core systems

---

### 🔗 Connections to Other Chapters

Pipelining is the evolution of the FDE cycle from **Chapter 1**. The registers being filled and emptied at each stage are the digital circuits from **Chapter 6**. Cache hazards connect to **Chapter 9**. Assembly language (**Chapter 7**) compilers target specific ISA (CISC/RISC) architectures. The bus interfaces connecting CPU cores to memory and I/O are covered in **Chapter 11**.

---

<!-- VERSION: 1.0 | Chapter 11 | 2025 -->

## Chapter 11: Interfaces & Communication

### 🚀 The Hook

When the International Space Station communicates with Earth, or when a Mars rover downloads a day's worth of scientific data to its on-board storage, every byte travels through serial communication protocols — the same fundamental concepts you'll learn in this chapter. From USB to Ethernet to the I2C bus inside a smartwatch, interfaces are how every component of a computer system talks to every other component.

---

### 📌 What You'll Learn

- Distinguish parallel vs. serial communication and explain when each is preferred
- Describe the role of a system bus and its three constituent buses
- Compare RS-232C, USB, SATA, PCIe, Ethernet, I2C, and SCSI characteristics
- Explain the crossbar switch as an alternative to shared buses
- Calculate data transfer rates from interface specifications

---

### 🧠 Core Concepts

#### The System Bus

*Analogy:* The system bus is the highway system of a computer. Just as cities have roads (address signs tell you where to go, cargo trucks carry goods, traffic lights control flow), computers have address lines, data lines, and control lines.

**Three sub-buses:**

| Sub-bus | What it carries | Direction |
|---------|----------------|-----------|
| Address bus | Memory/device address | CPU → others (unidirectional) |
| Data bus | The actual data | Bidirectional |
| Control bus | Read/Write/Clock signals | Both directions |

**Bus performance:**

```
Data transfer rate = Bus_width × Clock_frequency

Example: 64-bit bus at 100 MHz
Rate = 64 bits × 100M cycles/sec = 6400 Mbits/sec = 800 MB/s
```

---

#### Parallel vs. Serial Communication

| Property | Parallel | Serial |
|----------|---------|--------|
| How many wires | Many (one per bit) | One (or a few) |
| Speed per cycle | Fast (many bits at once) | Slower (one bit at a time) |
| Distance | Short (crosstalk issues) | Long (simple signal) |
| Cost | Expensive (many wires) | Cheap |
| Modern trend | Replaced by serial | Dominant today |
| Examples | Old IDE/PATA, parallel port | USB, SATA, PCIe, Ethernet |

> 💡 **Key insight:** Paradoxically, modern high-speed "serial" interfaces (PCIe, USB 3.x) are faster than old parallel interfaces. Why? At GHz frequencies, parallel wires interfere with each other (crosstalk). Serial interfaces avoid this, enabling much higher per-wire frequencies.

---

#### Interface Standards Reference

| Interface | Type | Typical Speed | Primary Use |
|-----------|------|--------------|-------------|
| **RS-232C** | Serial (async) | Up to 115.2 Kbps | Legacy COM ports, serial terminals |
| **USB 3.2** | Serial | 10–20 Gbps | Universal peripheral connection |
| **SATA III** | Serial | 6 Gbps | Internal storage (HDDs, SSDs) |
| **PCIe 4.0 ×16** | Serial (lanes) | ~64 GB/s | GPU, NVMe SSDs, high-speed cards |
| **Ethernet** | Serial | 1 Gbps – 400 Gbps | Network (LAN, WAN, internet) |
| **I2C** | Serial (sync, 2-wire) | 100 Kbps – 3.4 Mbps | Sensors, embedded devices |
| **SCSI** | Parallel/Serial | Up to 640 MB/s | High-performance storage (servers) |

---

#### RS-232C

**RS-232C** — An older serial communication standard for asynchronous point-to-point communication.

- Voltages: logic 1 = -3V to -15V; logic 0 = +3V to +15V (inverted!)
- Data frame: start bit + 5–8 data bits + optional parity bit + stop bit(s)
- Max distance: ~15 meters at 9600 baud

> ⚠️ **Warning:** RS-232C uses inverted logic (negative voltage = 1). This catches students out. It's not TTL logic.

---

#### USB (Universal Serial Bus)

USB uses a host-controlled, tree topology:

```
HOST (PC)
  |
[Root Hub]
  |      |
[Hub 1] [Device A]
  |      |
[Dev B] [Dev C]
```

- USB is **host-controlled**: the host (PC) initiates ALL transfers; devices never initiate
- USB 2.0: 480 Mbps High Speed
- USB 3.2 Gen 2×2: 20 Gbps
- USB4: up to 40 Gbps

---

#### PCIe (PCI Express)

**PCIe** — A high-speed serial interface using multiple "lanes" (×1, ×4, ×8, ×16).

Each lane is a full-duplex serial link. More lanes = more bandwidth.

```
PCIe 4.0 per lane: 2 GB/s (full-duplex)
×16 slot (GPU): 2 × 16 = 32 GB/s in each direction
```

---

#### I2C (Inter-Integrated Circuit)

**I2C** — A 2-wire synchronous serial protocol for connecting microcontrollers to sensors and peripheral ICs.

- **SDA** (Serial Data) + **SCL** (Serial Clock) = only 2 wires
- Multi-master, multi-slave on the same bus
- Each device has a 7-bit address
- Typical speeds: 100 Kbps (standard), 400 Kbps (fast), 3.4 Mbps (high speed)

> 🚀 **Real World:** I2C is everywhere in embedded systems — temperature sensors, accelerometers, OLED displays, RTC modules. If you ever program an Arduino or Raspberry Pi, you've used I2C.

---

#### Crossbar Switch

*Analogy:* A highway with an interchange at every junction, allowing any source to connect to any destination without waiting for others.

A **crossbar switch** connects N inputs to N outputs, with a switch at every intersection. Any input can connect to any output simultaneously (as long as no two inputs target the same output).

```
         OUT1  OUT2  OUT3  OUT4
IN1  ---[ X ]--[ X ]--[ X ]--[ X ]
IN2  ---[ X ]--[ X ]--[ X ]--[ X ]
IN3  ---[ X ]--[ X ]--[ X ]--[ X ]
IN4  ---[ X ]--[ X ]--[ X ]--[ X ]
```

Crossbar vs. shared bus:
- **Shared bus**: only one transfer at a time; cheap; doesn't scale
- **Crossbar**: multiple simultaneous transfers; expensive (N² switches); used in high-performance systems

---

### 🔢 Step-by-Step Procedures

#### Calculating Bus Transfer Rate

```
Problem: A 32-bit bus runs at 66 MHz. What is the data transfer rate in MB/s?

Step 1: Bits per second = 32 × 66,000,000 = 2,112,000,000 bits/sec
Step 2: Bytes per second = 2,112,000,000 / 8 = 264,000,000 bytes/sec
Step 3: Convert to MB/s = 264,000,000 / 1,048,576 ≈ 251.7 MB/s

Result: ≈ 252 MB/s
```

---

### ⚡ Why This Matters

Every peripheral you ever use communicates through one of these interfaces. USB drives, NVMe SSDs (PCIe), monitors (DisplayPort = PCIe-based), network cards (Ethernet), motherboard sensors (I2C). In embedded systems and IoT — the fastest growing area of tech employment — you will configure UART/RS-232, SPI, and I2C daily. In systems programming, understanding bus bandwidth is critical for performance analysis.

---

### 🧪 Lab Prep Exercises

**Exercise 11.1** — A 64-bit bus operates at 133 MHz. Calculate the maximum theoretical transfer rate in GB/s.

<details>
<summary>Show Solution</summary>

```
Bits/sec = 64 × 133,000,000 = 8,512,000,000 bits/sec
Bytes/sec = 8,512,000,000 / 8 = 1,064,000,000 bytes/sec
GB/s = 1,064,000,000 / 1,073,741,824 ≈ 0.991 GB/s ≈ 1 GB/s
```

</details>

---

**Exercise 11.2** — Compare USB and I2C: What are the main differences in topology, speed, and use case?

<details>
<summary>Show Solution</summary>

| Property | USB | I2C |
|----------|-----|-----|
| Topology | Star (tree via hubs) | Multi-drop bus |
| Wires | 4+ (data differential pair + power) | 2 (SDA + SCL) |
| Speed | Up to 40 Gbps (USB4) | Up to 3.4 Mbps |
| Addressing | Host-controlled | 7-bit device address |
| Use case | External peripherals (PC) | On-board sensors/chips |
| Complexity | High | Low |

USB: external high-speed peripherals. I2C: simple on-board sensor communication.

</details>

---

**Exercise 11.3** — Explain why modern high-speed interfaces are serial rather than parallel.

<details>
<summary>Show Solution</summary>

At high frequencies (GHz range), parallel buses suffer from:
1. **Crosstalk**: adjacent wires induce noise in each other at high frequencies
2. **Skew**: slight differences in wire length cause bits to arrive at different times, limiting sync
3. **Cost and complexity**: more wires, more connectors, larger PCB traces

Serial interfaces avoid these issues: fewer wires, simpler signaling, and differential signaling (two wires per lane carrying equal-and-opposite signals) rejects noise. This allows clock rates in the GHz range, making modern serial interfaces faster in practice than old parallel ones despite sending fewer bits per cycle.

</details>

---

### 📝 Exam Quick-Fire

**Q1.** The address bus in a computer system is typically:

- A) Bidirectional
- B) Unidirectional (CPU to devices)
- C) Unidirectional (devices to CPU)
- D) Not part of the system bus

**Q2.** I2C communication uses how many wires for data and clock?

- A) 1
- B) 2
- C) 4
- D) 8

**Q3.** In USB architecture, which device initiates data transfers?

- A) Any device can initiate
- B) Only the hub
- C) Always the host (PC)
- D) The device with highest priority

**Q4.** PCIe uses a "×16" configuration meaning:

- A) 16 parallel data bits
- B) 16 serial lanes, each carrying data
- C) A 16 GHz clock
- D) 16 volts signaling

**Q5.** Compared to a shared bus, a crossbar switch:

- A) Is cheaper but slower
- B) Supports fewer simultaneous transfers
- C) Allows multiple simultaneous transfers but is more expensive
- D) Uses fewer connections

<details>
<summary>Show Answers</summary>

1. **B** — The address bus is unidirectional: CPU sends addresses to memory/devices; devices don't send addresses to CPU.
2. **B** — I2C uses 2 wires: SDA (data) and SCL (clock).
3. **C** — USB is strictly host-controlled. The host polls/requests; devices only respond.
4. **B** — PCIe ×16 = 16 independent serial lanes operating in parallel, each lane being full-duplex serial.
5. **C** — Crossbar allows N simultaneous transfers (one per input-output pair) but requires N² switches — more expensive than a shared bus.

</details>

---

### 🔁 Chapter Summary

- **System bus** = address bus + data bus + control bus
- **Address bus** is unidirectional (CPU→devices); data bus is bidirectional
- **Parallel → Serial shift**: serial dominates today because it scales to GHz without crosstalk
- **RS-232C**: legacy async serial; inverted logic; low speed; short distance
- **USB**: host-controlled tree topology; up to 40 Gbps (USB4)
- **SATA**: serial storage interface for drives; up to 6 Gbps
- **PCIe**: lane-based (×1, ×4, ×8, ×16); used for GPU, NVMe; high bandwidth
- **I2C**: 2-wire (SDA+SCL); multi-device; used for sensors and embedded chips
- **Crossbar switch**: N×N switching fabric; allows simultaneous transfers; more expensive than bus

---

### 🔗 Connections to Other Chapters

The system bus directly implements the connection between all Von Neumann components (**Chapter 1**). Tri-state gates (**Chapter 6**) make bus-sharing possible. DMA transfers (**Chapter 8**) use the data bus. PCIe lanes are how modern GPUs (SIMD processors, **Chapter 10**) connect to the system. I2C and UART are used to interface with the external sensors and devices that generate interrupts (**Chapter 8**).

---

## Appendix A: Master Formula Sheet

*Your last-minute revision sheet. All formulas from all chapters in one place.*

---

### Chapter 2 — Number Systems

```
Decimal → Binary:        Repeated division by 2; read remainders bottom-up
Decimal fraction → Bin:  Repeated ×2; read integer parts top-down
Binary → Hex:            Group into 4-bit nibbles from right; use cheat grid
Hex → Decimal:           Sum of (digit × 16^position)
Binary → Octal:          Group into 3-bit groups from right

2's complement:          Flip all bits + 1  (negation)
Range (n-bit 2's comp):  −2^(n−1) to +2^(n−1) − 1
8-bit range:             −128 to +127
```

### Chapter 3 — IEEE-754

```
Single precision:  1 sign + 8 exponent + 23 mantissa = 32 bits
Double precision:  1 sign + 11 exponent + 52 mantissa = 64 bits

Bias (single):   127   →  Stored exponent = Actual + 127
Bias (double):   1023  →  Stored exponent = Actual + 1023

Value = (−1)^sign × 1.mantissa × 2^(stored_exp − bias)

Special values (single):
  Exp = 0, Mant = 0   → Zero
  Exp = 255, Mant = 0 → ±Infinity
  Exp = 255, Mant ≠ 0 → NaN
```

### Chapter 4 — Character Encoding

```
ASCII range:  0–127 (7-bit)
'0' = 48 (0x30)     '9' = 57
'A' = 65 (0x41)     'Z' = 90
'a' = 97 (0x61)     'z' = 122
Space = 32;  DEL = 127

Lowercase = Uppercase + 32 (OR with 0x20)
Uppercase = Lowercase − 32 (AND with 0xDF)
```

### Chapter 5 — Boolean Algebra

```
De Morgan's Law:   (A+B)̄ = Ā·B̄    (A·B)̄ = Ā+B̄
XOR:               A⊕B = ĀB + AB̄   (A⊕A = 0;  A⊕0 = A)
SOP:               OR of AND terms from rows where F=1
K-map group size:  Power of 2 only (1,2,4,8,16)
K-map column order: 00, 01, 11, 10 (Gray code)
```

### Chapter 6 — Digital Circuits

```
MUX select lines needed: log₂(number of inputs)
4-to-1 MUX: Y = S̄1·S̄0·I0 + S̄1·S0·I1 + S1·S̄0·I2 + S1·S0·I3
Counter states: 2^n  (n = number of flip-flops)
```

### Chapter 8 — Memory Systems

```
Memory capacity: 2^(address lines) × data_bus_width (bits)
                 ÷ 8 for bytes

Example: 16 addr lines, 8-bit bus → 2^16 × 1 byte = 64 KB
```

### Chapter 9 — Cache & Virtual Memory

```
Effective Access Time:
  EAT = h × T_cache + (1 − h) × T_RAM
  (h = hit rate, 0–1)

Virtual address → Physical:
  Physical = Frame_number × Page_size + Page_offset
  VPN bits = total_address_bits − log₂(page_size)
```

### Chapter 10 — Processors

```
Pipeline execution time: (stages − 1) + instructions  (cycles)
Non-pipeline time:       instructions × stages        (cycles)
Speedup = Non-pipeline / Pipeline

Flynn's Taxonomy:
  SISD: 1 inst, 1 data stream (classic CPU)
  SIMD: 1 inst, many data (GPU, vector)
  MISD: many inst, 1 data (rare)
  MIMD: many inst, many data (multi-core)
```

### Chapter 11 — Interfaces

```
Bus transfer rate = Bus_width_bits × Clock_Hz / 8   (bytes/sec)

PCIe bandwidth per lane: ~2 GB/s (PCIe 4.0, each direction)
×16 = 16 × 2 = 32 GB/s each direction
```

---

## Appendix B: Common Exam Question Patterns

*The 10 most common CSA multiple-choice question types, with strategy.*

---

**Pattern 1: Decimal-to-Binary Conversion**
> "Convert 185 to binary"

Strategy: Use repeated division by 2. Write the work out column by column. Read remainders from bottom to top. Double-check by summing bit-position values. Time: 2 minutes.

---

**Pattern 2: IEEE-754 Encoding/Decoding**
> "What decimal value does 0 10000011 01000000000000000000000 represent?"

Strategy: Split into S / Exp / Mantissa. Compute Actual = Stored − 127. Rebuild the number: (−1)^S × 1.Mantissa × 2^Actual. Watch out for the hidden bit — always add the implicit leading 1.

---

**Pattern 3: 2's Complement / Negative Binary**
> "What is −45 in 8-bit 2's complement?"

Strategy: Convert +45 to binary → flip all bits → add 1. Verify by adding result to +45 — should get 00000000 (with carry ignored).

---

**Pattern 4: Boolean Algebra Simplification**
> "Simplify: AB + AB̄"

Strategy: Factor out A: A(B + B̄) = A × 1 = A. Always check De Morgan's and the complement law (A + Ā = 1) first — they eliminate terms instantly.

---

**Pattern 5: K-map Minimization**
> "Minimize F(A,B,C) = Σm(1,3,5,7)"

Strategy: Fill in 1s in correct positions (use Gray code column order!). Find largest possible groups of 1s (powers of 2). Write one AND term per group, including only variables that don't change within the group.

---

**Pattern 6: Cache Effective Access Time**
> "Hit rate 90%, cache = 5ns, RAM = 100ns. Find EAT."

Strategy: EAT = h × T_cache + (1−h) × T_RAM. Plug in directly. Be alert to whether the exam version includes T_cache in the miss penalty or not.

---

**Pattern 7: Memory Capacity Calculation**
> "How much memory can a system with 20 address lines and a 16-bit data bus address?"

Strategy: Locations = 2^20. Each location = 16 bits = 2 bytes. Total = 2^20 × 2 bytes = 2 MB.

---

**Pattern 8: Assembly Code Tracing**
> "What is the value of EAX after: MOV EAX,10; SUB EAX,3; MUL ECX (ECX=4)"

Strategy: Trace each instruction in order. After MOV: EAX=10. After SUB: EAX=7. MUL ECX: EAX = 7×4 = 28. Don't forget MUL uses EAX implicitly.

---

**Pattern 9: Pipeline Speedup**
> "A 5-stage pipeline executes 100 instructions. Compare time to non-pipelined."

Strategy: Pipeline = (stages−1) + instructions = 4 + 100 = 104 cycles. Non-pipeline = 100 × 5 = 500. Speedup = 500/104 ≈ 4.8×.

---

**Pattern 10: Interface/Protocol Identification**
> "Which interface uses 2 wires and supports multiple devices on the same bus?"

Strategy: I2C = 2 wires (SDA+SCL), multi-device. SPI = 4 wires, faster. UART/RS-232 = 2 wires but point-to-point. USB = tree topology, host-controlled. Memorize the distinguishing feature of each interface.

---

## Appendix C: Lab Quick-Reference

*One-page reference cards for the most common lab task types. Use these in timed assignments.*

---

### Card 1: Number Conversions — Quick Method Selection

| Given → Want | Method | Time |
|--------------|--------|------|
| Dec → Bin | Divide by 2, read up | 1 min |
| Dec → Hex | Dec → Bin → group 4s | 2 min |
| Bin → Hex | Group into 4s, use grid | 30 sec |
| Hex → Bin | Each hex → 4 bits | 30 sec |
| Hex → Dec | Positional value sum | 1 min |
| Dec frac → Bin | Multiply by 2, read down | 1 min |
| Neg Dec → Bin | Convert +, flip, +1 | 1 min |

**Hex cheat (memorize A–F):** A=10, B=11, C=12, D=13, E=14, F=15

---

### Card 2: IEEE-754 Encoding Steps

```
1. Sign bit: negative = 1, positive = 0
2. Convert |number| to binary (integer + fraction parts)
3. Normalize: move point to get 1.xxxxx × 2^n
4. Exponent: store = n + 127 (single) or n + 1023 (double)
5. Mantissa: bits after the "1." in step 3 (zero-pad to 23 bits)
6. Assemble: [Sign][8-bit Exponent][23-bit Mantissa]
```

---

### Card 3: K-map Minimization (4-variable)

```
Column order: AB\CD  00  01  11  10
                00 [ 0][ 1][ 3][ 2]
                01 [ 4][ 5][ 7][ 6]
                11 [12][13][15][14]
                10 [ 8][ 9][11][10]

Rules:
✓ Fill 1s in minterm positions
✓ Groups = 1, 2, 4, 8, or 16 cells
✓ Largest groups first
✓ Wrap around edges
✓ Variable eliminated if it changes within a group
✓ One product term per group → OR them all = SOP
```

---

### Card 4: Assembly Quick-Reference

```
Zero a register:    XOR EAX, EAX
If-then:            CMP EAX, val → JE / JNE / JL / JG → label
Multiply:           MOV EAX, a → MOV ECX, b → MUL ECX → result in EAX
Clear bit n:        AND EAX, NOT (1 << n)
Set bit n:          OR  EAX, (1 << n)
Toggle bit n:       XOR EAX, (1 << n)
Check if zero:      CMP EAX, 0 → JE zero_label
Negate:             NEG EAX  (or XOR + INC)
```

---

### Card 5: Cache Calculation Formula

```
EAT = h × T_cache + (1 − h) × T_RAM

Where:
  h         = hit rate (0.0 to 1.0)
  T_cache   = cache access time (ns)
  T_RAM     = main memory access time (ns)
  EAT       = Effective Access Time (ns)

Example: h=0.95, T_cache=4ns, T_RAM=200ns
EAT = 0.95(4) + 0.05(200) = 3.8 + 10 = 13.8 ns
```

---

### Card 6: Interface Identification Cheat Sheet

| Interface | Wires | Speed | Topology | Remember By |
|-----------|-------|-------|----------|-------------|
| I2C | 2 | Up to 3.4 Mbps | Multi-drop | "2 wires, many devices" |
| SPI | 4 | 10s Mbps | Daisy chain | "4 wires, fast, short" |
| RS-232 | 2 | 115.2 Kbps | Point-to-point | "Legacy serial COM" |
| USB | 4+ | Up to 40 Gbps | Tree | "Universal, host-controlled" |
| SATA | 7 | 6 Gbps | Point-to-point | "Storage only" |
| PCIe | Lanes | 2 GB/s/lane | Point-to-point | "Lanes, GPU, NVMe" |
| Ethernet | 8 (Cat5) | 1G–400G | Various | "Network, packets" |

---

*End of CSA Complete Coursebook v1.0*

<!-- TODO: expand Chapter 6 shift register section with more worked examples -->
<!-- TODO: add more MASM32 sample programs in Chapter 7 -->
<!-- TODO: expand Chapter 9 with TLB miss penalty calculation examples -->