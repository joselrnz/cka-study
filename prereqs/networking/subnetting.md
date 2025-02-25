
### Network & Host Portions
Each IPv4 address is conceptually split into two parts:
1. **Network portion**: Identifies the specific network.
2. **Host portion**: Identifies a specific host (device) within that network.

### CIDR Notation
- **CIDR** (Classless Inter-Domain Routing) uses “slash” notation: `/x`.
- The `/x` denotes how many bits (starting from the left) make up the **network portion**.

A `/24` means:
- The first 24 bits are the network portion.
- The last 8 bits are for hosts.

In dotted-decimal form, a `/24` mask is `255.255.255.0`.

### Classful vs Classless
- **Classful** addressing was the old system where a "Class C" network by definition had a `255.255.255.0` mask (`/24`).
- **Classless** addressing (CIDR) is more flexible. You can use any subnet mask that suits your design (like `/25`, `/26`, etc.), regardless of traditional “class” boundaries.

---

## Subnetting a /24 Network

### Borrowing Bits
When you “subnet” a `/24`, you **borrow bits** from the host portion to create more networks. Each bit you borrow doubles the number of subnets.

- **Number of subnets** = \(2^{(\text{borrowed bits})}\)

### Subnet Mask Changes
Originally:  
- A `/24` network mask = `255.255.255.0`.

If you borrow 1 bit (making `/25`):
- New subnet mask = `255.255.255.128`.

Borrow 2 bits (making `/26`):
- New subnet mask = `255.255.255.192`, and so on.

### Number of Subnets and Hosts
- **Subnets**: \(2^{\text{borrowed bits}}\)
- **Hosts per subnet**: \(2^{\text{remaining host bits}} - 2\)

  (The “-2” accounts for the **Network Address** and the **Broadcast Address**, neither of which can be assigned to a host.)

---

## Example: 192.168.1.0/24

Let’s say you have the network `192.168.1.0/24`. By default, that’s **one** network with:
- Network portion: `192.168.1`
- Host portion: last 8 bits
- Usable host addresses: 254 (from `.1` to `.254`)

### Subnetting to /26
Suppose you want **4 subnets** in total. Borrow **2 bits** from the host portion:

- Original: `/24` → `255.255.255.0`
- New: `/26` → `255.255.255.192`
- Borrowed 2 bits → \(2^2 = 4\) subnets
- Each subnet can have \(2^{6} - 2 = 64 - 2 = 62\) usable IP addresses.

### Subnet Ranges for /26
Your 4 subnets are each 64 IP addresses long (`.0-.63`, `.64-.127`, `.128-.191`, `.192-.255`):

1. **Subnet 1**:  
   - Network Address: `192.168.1.0`  
   - Usable Host Range: `192.168.1.1` – `192.168.1.62`  
   - Broadcast Address: `192.168.1.63`

2. **Subnet 2**:  
   - Network Address: `192.168.1.64`  
   - Usable Host Range: `192.168.1.65` – `192.168.1.126`  
   - Broadcast Address: `192.168.1.127`

3. **Subnet 3**:  
   - Network Address: `192.168.1.128`  
   - Usable Host Range: `192.168.1.129` – `192.168.1.190`  
   - Broadcast Address: `192.168.1.191`

4. **Subnet 4**:  
   - Network Address: `192.168.1.192`  
   - Usable Host Range: `192.168.1.193` – `192.168.1.254`  
   - Broadcast Address: `192.168.1.255`

---

## Subnetting Table for /24

| Borrowed Bits | New CIDR | Subnet Mask          | Number of Subnets    | Total IPs per Subnet | Usable IPs (Hosts) |
|:-------------:|:--------:|:--------------------:|:--------------------:|:--------------------:|:------------------:|
| 0 (none)      | /24      | 255.255.255.0        | 1                    | 256                  | 254                |
| 1             | /25      | 255.255.255.128      | 2                    | 128                  | 126                |
| 2             | /26      | 255.255.255.192      | 4                    | 64                   | 62                 |
| 3             | /27      | 255.255.255.224      | 8                    | 32                   | 30                 |
| 4             | /28      | 255.255.255.240      | 16                   | 16                   | 14                 |
| 5             | /29      | 255.255.255.248      | 32                   | 8                    | 6                  |
| 6             | /30      | 255.255.255.252      | 64                   | 4                    | 2                  |

> **Note**:  
> - For typical IPv4 subnets, subtracting 2 from the total IPs yields the number of usable host IPs (because of the Network and Broadcast addresses).  
> - `/31` and `/32` are special cases used typically for point-to-point links (/31) and loopback or single IP assignments (/32).

---

## Additional Tips
1. **Remember the Binary**: It’s often helpful to visualize or write out the 8 bits of the last octet when subnetting a `/24`.  
   - Example: For a `/25`, your last octet might be `1_______` in binary, showing you borrowed 1 bit.

2. **Subnet Boundaries**: Each subnet range starts at multiples of the subnet block size in the last octet. For example, a `/26` block size is 64 in decimal (in the last octet).

3. **Plan for Future Growth**: Always consider the potential need for more subnets or more hosts per subnet before choosing how many bits to borrow.

4. **Network Design**: You may want to leave space for future networks (e.g., not all subnets must be used immediately, especially in a private addressing scenario like `192.168.x.x`).

---

## Conclusion
- A single `/24` network provides 254 usable addresses and can be subdivided (“subnetted”) into smaller networks by borrowing bits from the host portion.
- Each borrowed bit **doubles** the number of subnets and **halves** the number of available addresses per subnet.
- Use the table above to quickly determine how many subnets you can create and how many hosts each will support.

