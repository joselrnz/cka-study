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
Subnetting can feel intimidating at first, but it’s really about **splitting a larger network** into **smaller, more manageable** subnetworks. Once you understand the logic, it becomes second nature.

By the end of this guide, you'll have:
- A **solid understanding** of how subnetting works
- **Simple formulas** and **tricks** to remember key details
- A **step-by-step method** you can apply to any IPv4 subnetting scenario

---

## Subnetting Refresher

### IP Address Basics
An IPv4 address is **32 bits** in total, typically shown as four 8-bit decimal numbers (octets), e.g.:
```
192.168.10.100
```
This can be represented in binary as:
```
192       .168      .10       .100
11000000  .10101000 .00001010 .01100100
```

### Network vs. Host Bits
- **Network bits** (N) identify which network the address belongs to.
- **Host bits** (H) identify a particular device (host) on that network.

**Subnetting** is about “borrowing” some bits from the **host** portion and using them as **network** bits, thus creating more (smaller) networks.

### CIDR Notation
- A `/x` means the first **x bits** of the IP address are **network bits**.
- For instance, `/24` means **24 network bits** and **8 host bits**.

| CIDR  | Network Bits | Host Bits |
|-------|-------------:|----------:|
| /24   | 24           | 8         |
| /25   | 25           | 7         |
| /26   | 26           | 6         |
| /27   | 27           | 5         |
| /28   | 28           | 4         |
| /29   | 29           | 3         |
| /30   | 30           | 2         |
| /31   | 31           | 1         |
| /32   | 32           | 0         |

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
Also keep in mind the required **hosts**:
```
Usable hosts/subnet = 2^(host bits remaining) - 2
```
(The “-2” is for the network address and broadcast address.)

### 3. Calculate Subnet Mask & Block Size
After deciding how many bits to borrow, you can find:
- The **new subnet mask** in both `/x` form and dotted decimal.
- The **block size** (increment) which tells you how subnets are spaced in the last octet (or relevant octet).

### 4. Determine Subnets and Ranges
Finally:
- **List** the network addresses.
- **List** the broadcast address for each subnet.
- **Identify** the usable host range in each subnet.

---

## Tricks to Remember Subnetting Forever

### Mnemonic: "256 - Mask = Block Size"
For quick subnet calculations:
1. Look at the **last octet** (if the new subnet mask changes there).
2. **Do**: 256 - (that octet) = block size.
3. **List** your subnets in increments of that block size in the changed octet.

For example, `255.255.255.192` → last octet `192` → `256 - 192 = 64`.  
- So subnets are `.0`, `.64`, `.128`, `.192`.

### Binary Quick Reference
- **128 in binary** = `10000000`
- **192 in binary** = `11000000`
- **224 in binary** = `11100000`
- **240 in binary** = `11110000`
- **248 in binary** = `11111000`
- **252 in binary** = `11111100`
- **254 in binary** = `11111110`
- **255 in binary** = `11111111`

These are the **common subnet mask endings**. Memorizing them makes your job far easier.

### Practice, Practice, Practice
Nothing beats practicing real-world scenarios. Subnet in your home lab, do sample exam questions, or create hypothetical networks on paper. The more you do, the more it sticks.

---

## Conclusion
Subnetting is all about breaking down the 32-bit IPv4 space into **smaller chunks**. Once you grasp:
1. **How many bits** to borrow,
2. **How block size** (increments) work,
3. **How many hosts** you can fit,

…you can tackle **any** subnetting problem quickly. Keep the **block-size trick (256 - mask)** and the **binary patterns** in mind. Practice regularly, and soon **subnetting** will be second nature.



