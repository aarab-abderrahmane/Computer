

# Number Systems: Binary, Octal, Decimal, Hex

A complete beginner-friendly guide with multiple conversion methods

---

## Table of Contents

1. [Introduction](#introduction)
2. [What is a "Base"?](#what-is-a-base)
3. [Number Systems](#number-systems)

   * [Binary (Base-2)](#binary-base-2)
   * [Octal (Base-8)](#octal-base-8)
   * [Decimal (Base-10)](#decimal-base-10)
   * [Hexadecimal (Base-16)](#hexadecimal-base-16)
4. [Conversions](#conversions)

   * [Decimal → Binary](#decimal--binary)
   * [Decimal → Octal](#decimal--octal)
   * [Decimal → Hex](#decimal--hex)
   * [Binary → Decimal](#binary--decimal)
   * [Binary ↔ Octal & Hex Tricks](#binary--octal--hex-tricks)

---

## Introduction

Computers store and process everything using **bits** (0s and 1s).
To make data readable, humans use different number systems: binary, octal, decimal, and hex.
Understanding how to convert between them is essential in programming, hardware, algorithms, and debugging.

---

## What is a Base?

A **base** is the number of unique digits a number system uses.

Examples:

| System      | Base | Digits Used |
| ----------- | ---- | ----------- |
| Binary      | 2    | 0–1         |
| Octal       | 8    | 0–7         |
| Decimal     | 10   | 0–9         |
| Hexadecimal | 16   | 0–9 + A–F   |

Base determines how each digit's value increases by powers of that base.

---

# Number Systems

---

## Binary (Base-2)

* Digits: `0, 1`
* Each position is a power of 2.

Example:
`10110₂` =
1×2⁴ + 0×2³ + 1×2² + 1×2¹ + 0×2⁰
= 16 + 0 + 4 + 2 + 0
= **22**

---

## Octal (Base-8)

* Digits: `0–7`
* Each position is a power of 8.

Example:
`157₈` =
1×8² + 5×8¹ + 7×8⁰
= 64 + 40 + 7
= **111**

---

## Decimal (Base-10)

* Digits: `0–9`
* Standard system used daily.

Example:
`472₁₀` =
4×10² + 7×10¹ + 2×10⁰
= 400 + 70 + 2

---

## Hexadecimal (Base-16)

* Digits: `0–9` + `A–F` (A=10, …, F=15)
* Popular for memory addresses, colors (#FFAA22), machine code.

Example:
`3AF₁₆` =
3×16² + 10×16¹ + 15×16⁰
= 768 + 160 + 15
= **943**

---

# Conversions

---

# Decimal → Binary

## Method 1: **Descending Powers of Two and Subtraction**

**Concept:**
Find the largest power of 2 ≤ number, subtract it, repeat.

Example: Convert **74**

| Power of 2 | Value | Fits? | Add to binary | New number |
| ---------- | ----- | ----- | ------------- | ---------- |
| 2⁶         | 64    | Yes   | 1             | 74−64 = 10 |
| 2⁵         | 32    | No    | 0             | 10         |
| 2⁴         | 16    | No    | 0             | 10         |
| 2³         | 8     | Yes   | 1             | 10−8 = 2   |
| 2²         | 4     | No    | 0             | 2          |
| 2¹         | 2     | Yes   | 1             | 2−2 = 0    |
| 2⁰         | 1     | No    | 0             | 0          |

Result: **74 → 1001010₂**

---

## Method 2: **Division by 2 (Repeated Remainders)**

Divide by 2 until zero. Read remainders bottom-up.

Example: 74

```
74 / 2 = 37 R0
37 / 2 = 18 R1
18 / 2 = 9  R0
9  / 2 = 4  R1
4  / 2 = 2  R0
2  / 2 = 1  R0
1  / 2 = 0  R1
```

Binary (bottom→top):
**1001010**

---

## Method 3: **Bit-Place Filling (Visual Grid Method)**

Create slots for powers of two (64, 32, 16, 8, 4, 2, 1).
Mark 1 if ≤ remainder, else 0.

Example 74:

```
64 | 32 | 16 | 8 | 4 | 2 | 1
 1 |  0 |  0 | 1 | 0 | 1 | 0
```

Result: **1001010₂**

---

---

# Decimal → Octal

## Method 1: **Descending Powers of 8 and Subtraction (detailed)**

Same logic as binary, but using powers of 8.

Convert **345**

Powers: 8²=64, 8³=512 (too big) → start at 64.

```
345 ÷ 64 = 5 remainder 345 - (5×64) = 25
25 ÷ 8 = 3 remainder 1
1 ÷ 1 = 1 remainder 0
```

Result digits:
5 → 3 → 1 → **531₈**

---

## Method 2: **Division by 8 (standard)**

Example: 345

```
345 / 8 = 43 R1
43  / 8 = 5  R3
5   / 8 = 0  R5
```

Read bottom→top: **531₈**

---

## Method 3: **Binary Grouping Trick**

Convert decimal → binary → group bits.

Each octal digit = 3 bits.

Example: 74

Binary: `1001010`
Pad left: `001 001 010`
Groups → octal digits:

* 001 = 1
* 001 = 1
* 010 = 2

Result: **112₈**

---

---

# Decimal → Hex

## Method 1: **Descending Powers of 16 and Subtraction**

Convert **450**

Powers of 16:
16²=256 fits → 450−256=194
16¹=16 → 194/16=12 remainder 2
12 = C in hex

Digits:
256 → 12 → 2
= **C2₂₁₆** → **1C2₁₆** (corrected grouping)

Let's do properly:

```
450 / 256 = 1 remainder 194
194 / 16  = 12 (C) remainder 2
2 / 1     = 2
```

So: **1C2₁₆**

---

## Method 2: **Division by 16**

Example: 450

```
450 / 16 = 28 R2
28  / 16 = 1  RC (12)
1   / 16 = 0  R1
```

Result bottom→top: **1C2**

---

## Method 3: **Binary Grouping Trick**

Hex groups = 4 bits.

Example: 74

Binary: `1001010`
Pad left: `0100 1010`
Groups:

* 0100 = 4
* 1010 = A

Hex: **4A**

---

---

# Binary → Decimal

Use powers of two.

Example: `110101`

```
1×32 + 1×16 + 0×8 + 1×4 + 0×2 + 1×1
= 32 + 16 + 0 + 4 + 0 + 1
= 53
```

---

# Binary ↔ Octal & Hex Tricks

## Binary → Octal

Group bits by 3.

Example: `110101111`
Pad: `110 101 111`
→ 6 5 7
= **657₈**

## Binary → Hex

Group bits by 4.

Example: `110101111`
Pad: `0011 0101 1111`
→ 3 5 F
= **35F₁₆**

