### **Networking Fundamentals**

### **3.1 IP Addressing**

IP (Internet Protocol) addresses are used to identify devices on a network, allowing communication between different systems over the internet or within a private network. Every device connected to a network has a unique IP address.

There are two main versions of IP addresses: **IPv4** and **IPv6**.

---

### **IPv4 vs. IPv6**

#### **1. IPv4 (Internet Protocol version 4)**:
- **Format**: IPv4 uses a 32-bit address system, which is typically represented as four decimal numbers separated by periods (dots). Each decimal number is called an **octet** and can range from 0 to 255.
  - **Example**: `192.168.1.10`
  
- **Address Space**: Since IPv4 uses 32 bits, it can support approximately 4.3 billion unique addresses (`2^32 = 4,294,967,296`).

- **Usage**: IPv4 is still widely used for most of the internet's traffic, although it's running out of available addresses due to the rapid expansion of devices connected to the internet.

#### **2. IPv6 (Internet Protocol version 6)**:
- **Format**: IPv6 uses a 128-bit address system, represented as eight groups of four hexadecimal digits separated by colons. Leading zeros can be omitted, and a series of consecutive zeroes can be compressed using `::`.
  - **Example**: `2001:0db8:85a3:0000:0000:8a2e:0370:7334` or compressed as `2001:db8:85a3::8a2e:370:7334`
  
- **Address Space**: IPv6 provides a much larger address space (`2^128`), supporting approximately 340 undecillion (340 trillion trillion trillion) unique IP addresses.

- **Usage**: IPv6 was designed to address the shortage of IPv4 addresses and improve routing and security features. Many modern systems support both IPv4 and IPv6.

---

### **Public vs. Private IP Addresses**

#### **1. Public IP Addresses**:
- **Definition**: Public IP addresses are globally unique addresses assigned to devices that need to communicate directly over the internet. These addresses are assigned by **ISPs (Internet Service Providers)**.
  
- **Examples**: 
  - **Example IPv4 Public Address**: `203.0.113.5`
  - **Example IPv6 Public Address**: `2001:db8::1`

- **Usage**: Public IP addresses are required for websites, cloud services, and any system that needs to be accessible over the internet.

#### **2. Private IP Addresses**:
- **Definition**: Private IP addresses are used for internal networks, such as homes, businesses, or data centers. They are not routable on the internet and must go through a **Network Address Translation (NAT)** device (like a router) to communicate with public IP addresses.

- **Reserved Private IP Ranges** (IPv4):
  - `10.0.0.0` to `10.255.255.255` (Class A)
  - `172.16.0.0` to `172.31.255.255` (Class B)
  - `192.168.0.0` to `192.168.255.255` (Class C)
  
  **Example**: `192.168.1.100` (commonly used for home networks)

- **Usage**: Private IP addresses are used within local networks (such as homes, offices, or data centers) to allow devices to communicate internally without consuming public IP addresses.

#### **Example:**
- A **home network** might assign private IP addresses such as `192.168.0.2` to a laptop and `192.168.0.3` to a smartphone, while the router uses a single public IP address (e.g., `198.51.100.5`) to connect to the internet.

---

### **CIDR Notation (Classless Inter-Domain Routing)**

#### **1. What is CIDR?**
- **CIDR** is a method of IP address allocation and routing that allows for more efficient use of IP addresses compared to the old **classful addressing** system. CIDR represents IP addresses and their associated routing prefix using a **slash (/) notation**.
  
- **Format**: `IP Address / Prefix Length`
  - The **prefix length** defines how many bits of the address are dedicated to the network portion (leftmost bits), while the remaining bits are available for hosts.

#### **2. Example of CIDR Notation**:

- **IPv4 CIDR Notation**:
  - **192.168.1.0/24**: This indicates that the first 24 bits (or the first three octets) are reserved for the network, and the remaining 8 bits are for host addresses within that network.
    - This means the network address is `192.168.1.0`, and the host IP range is `192.168.1.1` to `192.168.1.254`, with `192.168.1.255` reserved for the broadcast address.
  
- **IPv6 CIDR Notation**:
  - **2001:db8::/64**: This indicates that the first 64 bits are the network portion, and the remaining 64 bits are used for device (host) addresses on that network.

#### **3. Benefits of CIDR**:
- **Flexible Subnetting**: CIDR allows for creating networks of varying sizes by adjusting the prefix length, unlike the older class-based system, which only supported fixed network sizes.
- **Efficient IP Address Use**: With CIDR, IP addresses can be allocated based on actual needs, reducing waste.

#### **Example**:
- **192.168.0.0/16**: A larger network that allows `65,536` IP addresses (`192.168.0.1` to `192.168.255.254`).
- **192.168.1.0/24**: A smaller network that allows `256` IP addresses (`192.168.1.1` to `192.168.1.254`).

---

### **Summary Table:**
| Concept        | Description                                                   | Example                                               |
|----------------|---------------------------------------------------------------|-------------------------------------------------------|
| **IPv4**       | 32-bit addressing, uses 4 octets (dotted decimal).             | `192.168.1.1`                                         |
| **IPv6**       | 128-bit addressing, uses hexadecimal groups separated by colons. | `2001:0db8:85a3::8a2e:370:7334`                        |
| **Public IP**  | Globally routable address for internet communication.          | `203.0.113.5` (IPv4), `2001:db8::1` (IPv6)             |
| **Private IP** | Used within local networks, not routable on the internet.      | `192.168.1.100` (IPv4), `fd00::/8` (IPv6)              |
| **CIDR**       | Classless IP address notation, specifies network and host bits. | `192.168.1.0/24`, `2001:db8::/64`                      |

---

### **Example of Using CIDR Notation in IP Addressing**

Let’s say you’re running a company with an internal network. You’ve been assigned the **192.168.1.0/24** network. This means you can assign IP addresses ranging from **192.168.1.1** to **192.168.1.254** to devices within your network, and **192.168.1.255** is reserved for broadcasting.

By breaking this into smaller subnets, such as **192.168.1.0/26**, you can create smaller groups with a limited number of devices. **/26** allows for 62 usable host addresses in each subnet (2 bits reserved for network and broadcast addresses).

Refrence: https://www.site24x7.com/tools/ipv4-subnetcalculator.html
