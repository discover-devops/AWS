### **3.4 Networking in AWS**

Amazon Web Services (AWS) provides highly customizable networking options to ensure secure and scalable networking infrastructure through **Amazon Virtual Private Cloud (VPC)**. VPC allows you to define your own network environment, including IP ranges, subnets, route tables, gateways, and more. Additionally, AWS provides mechanisms like **NAT Gateways** and **Bastion Hosts** to enhance network security and connectivity.

---

### **VPC (Virtual Private Cloud)**

#### **What is a VPC?**
- A **Virtual Private Cloud (VPC)** is an isolated network within AWS where you can launch resources such as **EC2 instances**, **databases**, and **containers**.
- A VPC provides control over network settings, including IP address ranges, subnets, routing tables, and security settings.
- By default, each AWS account is provided with a default VPC, but you can create multiple custom VPCs tailored to specific applications.

#### **Key Concepts of VPC:**
1. **IP Addressing**: 
   - When creating a VPC, you must specify an **IPv4 CIDR block** (such as `10.0.0.0/16`), which defines the range of private IP addresses available within the VPC. You can also optionally enable **IPv6**.
   
2. **Network Isolation**: 
   - Resources in a VPC are isolated from other VPCs and the internet unless explicitly configured to communicate.

3. **Customization**:
   - You can configure subnets, route tables, gateways, and security settings within the VPC to suit your needs.

#### **Example**:
- A company wants to deploy a private database and an internet-facing web server. They create a VPC with the CIDR block `10.0.0.0/16` and divide it into subnets: one public subnet for the web server and one private subnet for the database.

---

### **Subnets, Route Tables, and Gateways**

#### **1. Subnets**:
A **subnet** is a segment of a VPC's IP address range where you can place resources such as EC2 instances. You can create **public subnets** (which are connected to the internet) and **private subnets** (which are not directly connected to the internet).

- **Public Subnet**: A subnet that is associated with an **Internet Gateway** (allowing resources like EC2 instances to have public IP addresses and connect to the internet).
- **Private Subnet**: A subnet that is not associated with an internet gateway. Resources in a private subnet are not accessible from the internet but can access the internet via a **NAT Gateway**.

#### **Example**:
- In a VPC with CIDR `10.0.0.0/16`, you create two subnets:
  - Public subnet: `10.0.1.0/24` (for web servers).
  - Private subnet: `10.0.2.0/24` (for databases).

#### **2. Route Tables**:
A **Route Table** contains rules that determine where network traffic is directed within a VPC. Each subnet must be associated with a route table, and the table defines how traffic is routed between subnets and other network resources (like the internet or other VPCs).

- **Local Route**: Automatically allows communication between subnets within the same VPC.
- **Custom Routes**: You can create routes to direct traffic to **Internet Gateways**, **NAT Gateways**, **VPNs**, or **Peering Connections**.

#### **Example**:
- For the public subnet, you would configure a route that directs traffic to the internet via an **Internet Gateway**.
  - Route Table:
    ```
    Destination    Target
    0.0.0.0/0      igw-xxxxxx (Internet Gateway)
    ```

#### **3. Gateways**:
- **Internet Gateway (IGW)**: Enables resources in a VPC to connect to the internet. Public subnets need an Internet Gateway to allow internet access.
  - **Example**: A web server in a public subnet needs to access the internet, so the subnet's route table points to the **IGW**.

- **Virtual Private Gateway (VGW)**: Connects a VPC to an on-premises data center via a **VPN**.

- **Egress-Only Internet Gateway**: Allows outbound-only IPv6 traffic.

---

### **NAT Gateway and Bastion Host**

#### **1. NAT Gateway**:
A **NAT Gateway** (Network Address Translation Gateway) allows resources in a private subnet to access the internet (for updates, patching, etc.) without being directly exposed to the internet. It performs network address translation (NAT) for outbound traffic, enabling instances in a private subnet to initiate internet connections, but not receive unsolicited inbound connections.

- **Use Case**: A private database server that needs to download security patches from the internet but should not be accessible from the internet.

- **Example**:
  - A database in a private subnet can connect to the internet to download updates, but external users cannot access the database directly.

#### **2. Bastion Host**:
A **Bastion Host** is a special-purpose server designed to provide secure access to resources in private subnets. Typically, a Bastion Host is placed in a public subnet and acts as an intermediary for SSH or RDP access to instances in private subnets.

- **Use Case**: When you want to securely access instances in a private subnet (like a database server) from the internet, you use a Bastion Host to SSH into the private instances.

- **Example**:
  - You connect to the **Bastion Host** using SSH, and from there, you can SSH into instances within a private subnet.

---

### **3.5 Security Groups and NACLs (Network Access Control Lists)**

AWS provides two key mechanisms to control traffic flow to and from resources in a VPC: **Security Groups** and **Network Access Control Lists (NACLs)**.

#### **Security Groups**

A **Security Group** acts as a virtual firewall for your EC2 instances to control inbound and outbound traffic. It operates at the instance level, and you can attach multiple security groups to an instance.

- **Stateful**: Security groups remember the state of a connection. If an inbound rule allows traffic, the outbound traffic for that connection is automatically allowed.

- **Inbound and Outbound Rules**:
  - **Inbound**: Controls what traffic is allowed into the instance.
  - **Outbound**: Controls what traffic is allowed to leave the instance.

- **Example**:
  - A security group for a web server might allow inbound HTTP (port 80) and HTTPS (port 443) traffic, and allow SSH (port 22) traffic from a specific IP address.

  ```
  Inbound Rules:
  Type     | Protocol | Port Range | Source
  ---------|----------|------------|--------
  HTTP     | TCP      | 80         | 0.0.0.0/0
  HTTPS    | TCP      | 443        | 0.0.0.0/0
  SSH      | TCP      | 22         | 192.168.1.1/32
  ```

  - **Outbound**: By default, security groups allow all outbound traffic.

#### **Network Access Control Lists (NACLs)**

A **NACL** is an optional layer of security for your VPC that acts as a firewall for controlling traffic at the **subnet level**. NACLs provide stateless filtering, meaning they do not remember the state of a connection. You must define both inbound and outbound rules for any traffic that needs to be allowed.

- **Stateless**: Unlike security groups, NACLs are stateless. This means you must explicitly allow both inbound and outbound traffic for each request. If you allow inbound traffic, you also need an outbound rule to allow the response.

- **Inbound and Outbound Rules**:
  - **Inbound**: Controls what traffic is allowed into the subnet.
  - **Outbound**: Controls what traffic is allowed to leave the subnet.

- **Example**:
  - A NACL for a public subnet might allow inbound HTTP and HTTPS traffic and outbound traffic to any destination.

  ```
  Inbound Rules:
  Rule # | Type     | Protocol | Port Range | Source        | Allow/Deny
  -------|----------|----------|------------|---------------|------------
  100    | HTTP     | TCP      | 80         | 0.0.0.0/0     | ALLOW
  110    | HTTPS    | TCP      | 443        | 0.0.0.0/0     | ALLOW
  120    | SSH      | TCP      | 22         | 192.168.1.1/32| ALLOW
  ```

  ```
  Outbound Rules:
  Rule # | Type     | Protocol | Port Range | Destination    | Allow/Deny
  -------|----------|----------|------------|--------------- |------------
  100    | HTTP     | TCP      | 80         | 0.0.0.0/0      | ALLOW
  110    | HTTPS    | TCP      | 443        | 0.0.0.0/0      | ALLOW
  ```

#### **Security Group vs NACL**:
| **Feature**           | **Security Group**                          | **NACL**                                   |
|-----------------------|---------------------------------------------|--------------------------------------------|
| **Operates at**        | Instance level                             | Subnet level                              |
| **Stateful/Stateless** | Stateful                                   | Stateless                                 |
| **Rules**


Refrebce: https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html
