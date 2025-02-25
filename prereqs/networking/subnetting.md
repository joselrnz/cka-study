# Mastering Subnetting: A Detailed Guide

## Table of Contents
- [Mastering Subnetting: A Detailed Guide](#mastering-subnetting-a-detailed-guide)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Subnetting Refresher](#subnetting-refresher)
    - [IP Address Basics](#ip-address-basics)
    - [Network vs. Host Bits](#network-vs-host-bits)
    - [CIDR Notation](#cidr-notation)
  - [Four Pillars of Subnetting](#four-pillars-of-subnetting)
    - [1. Identify the Requirements](#1-identify-the-requirements)
    - [2. Decide How Many Bits to Borrow](#2-decide-how-many-bits-to-borrow)
    - [3. Calculate Subnet Mask \& Block Size](#3-calculate-subnet-mask--block-size)
    - [4. Determine Subnets and Ranges](#4-determine-subnets-and-ranges)
  - [Tricks to Remember Subnetting Forever](#tricks-to-remember-subnetting-forever)
    - [Mnemonic: "256 - Mask = Block Size"](#mnemonic-256---mask--block-size)
    - [Binary Quick Reference](#binary-quick-reference)
    - [Practice, Practice, Practice](#practice-practice-practice)
  - [Conclusion](#conclusion)

---

## Introduction
Subnetting can seem complex at first, but it’s simply a method of dividing a larger network into **smaller, more manageable** subnetworks. Once you grasp the principles, it becomes second nature.

By the end of this guide, you'll:
- Gain a **solid understanding** of subnetting.
- Learn **simple formulas** and **tricks** to remember subnetting calculations.
- Have a **step-by-step method** to apply in any IPv4 subnetting scenario.

---

## Subnetting Refresher

### IP Address Basics
An IPv4 address is **32 bits** long, typically displayed as four 8-bit decimal numbers (octets), e.g.:
```
192.168.10.100
```
In binary representation:
```
192       .168      .10       .100
11000000  .10101000 .00001010 .01100100
```

### Network vs. Host Bits
- **Network bits** (N) define the network segment.
- **Host bits** (H) identify individual devices within that network.

Subnetting involves **borrowing** bits from the **host portion** and using them as **network bits**, thus creating multiple subnetworks.

### CIDR Notation
CIDR (Classless Inter-Domain Routing) notation defines how many bits are used for the network portion:
- A `/x` means the first **x bits** of the IP address are **network bits**.
- For example, `/24` means **24 network bits** and **8 host bits**.

| CIDR  | Network Bits | Host Bits | Total IPs | Usable Hosts |
|-------|-------------:|----------:|----------:|-------------:|
| /24   | 24           | 8         | 256       | 254          |
| /25   | 25           | 7         | 128       | 126          |
| /26   | 26           | 6         | 64        | 62           |
| /27   | 27           | 5         | 32        | 30           |
| /28   | 28           | 4         | 16        | 14           |
| /29   | 29           | 3         | 8         | 6            |
| /30   | 30           | 2         | 4         | 2            |
| /31   | 31           | 1         | 2         | 2*           |
| /32   | 32           | 0         | 1         | 1*           |

> **Note:** `/31` and `/32` are special cases where both IPs are usable for specific network implementations.

---

## Four Pillars of Subnetting

### 1. Identify the Requirements
Ask:
- **How many subnets** do I need?
- **How many hosts** per subnet do I need?

### 2. Decide How Many Bits to Borrow
Use the formula:
```
Number of subnets = 2^(bits borrowed)
```
For hosts:
```
Usable hosts/subnet = 2^(host bits remaining) - 2
```
(The **-2** accounts for the **network** and **broadcast** addresses.)

### 3. Calculate Subnet Mask & Block Size
After deciding how many bits to borrow, determine:
- The **new subnet mask** in both CIDR and dotted decimal.
- The **block size** (increment), showing subnet boundaries.

### 4. Determine Subnets and Ranges
List:
- **Network addresses**
- **Broadcast addresses**
- **Usable host ranges** for each subnet

---

## Tricks to Remember Subnetting Forever

### Mnemonic: "256 - Mask = Block Size"
Quickly determine subnet ranges:
1. Look at the **last octet** (if subnet mask changes there).
2. Compute: `256 - (last octet) = block size`.
3. List subnets in increments of the block size.

For example:
- `255.255.255.192` → last octet `192` → `256 - 192 = 64`.
- Subnets: `.0`, `.64`, `.128`, `.192`.

### Binary Quick Reference
- **128 in binary** = `10000000`
- **192 in binary** = `11000000`
- **224 in binary** = `11100000`
- **240 in binary** = `11110000`
- **248 in binary** = `11111000`
- **252 in binary** = `11111100`
- **254 in binary** = `11111110`
- **255 in binary** = `11111111`

Memorizing these common subnet mask endings simplifies calculations.

### Practice, Practice, Practice
Real-world subnetting scenarios:
- Subnet your home network.
- Solve subnetting problems.
- Use subnet calculators to verify answers.

---

## Conclusion
Mastering subnetting means understanding:
1. **How many bits** to borrow.
2. **How subnet block sizes** work.
3. **How many hosts** each subnet can accommodate.

With practice, subnetting will become an intuitive skill. Keep the **block-size trick** and **binary patterns** in mind. Keep practicing, and soon, subnetting will be second nature.
