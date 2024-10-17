### **NACLs (Network ACLs) vs Security Groups (SGs) in AWS**

**Network Access Control Lists (NACLs)** and **Security Groups (SGs)** are both important components in securing AWS VPCs (Virtual Private Clouds), but they function in distinct ways. Understanding the differences between NACLs and Security Groups is key to properly managing access to and between AWS resources.

---

### **Key Differences Between NACLs and Security Groups**

| Feature                       | **Security Groups (SGs)**                               | **Network Access Control Lists (NACLs)**               |
|--------------------------------|---------------------------------------------------------|--------------------------------------------------------|
| **State**                      | **Stateful**: Tracks connections, allows return traffic | **Stateless**: Does not track connections, separate rules for inbound/outbound traffic |
| **Applies to**                 | **Instances (ENI)**: Applied at the instance level      | **Subnets**: Applied at the subnet level               |
| **Default Behavior**           | All inbound and outbound traffic is **denied** by default | All inbound and outbound traffic is **allowed** by default for the default NACL |
| **Number of Rules**            | Limited number of rules (usually 60) per security group | Up to 20 rules per NACL                                 |
| **Inbound and Outbound Rules** | **Single rule** for each connection (stateful)          | Separate rules for **inbound** and **outbound** traffic (stateless) |
| **Order of Evaluation**        | No specific order, all rules are evaluated              | Rules are evaluated in **numbered order**, from lowest to highest |
| **Allow/Deny**                 | Can only **allow** traffic                              | Can both **allow** and **deny** traffic                 |
| **Use Cases**                  | Fine-grained control at the instance level              | Broader control at the subnet level                     |

---

### **Security Groups (SGs)**

Security Groups are **stateful** and function as virtual firewalls that control traffic at the **instance level**. They allow or deny traffic based on defined rules, but since they are stateful, return traffic is automatically allowed even if it's not explicitly defined.

#### **Key Features of Security Groups**:
- **Stateful**: If you allow an inbound request, the outbound response is automatically allowed, and vice versa.
- **Applied at the instance level**: You can assign multiple security groups to an instance.
- **Rules**:
  - **Inbound rules**: Define the traffic allowed **into** the instance (e.g., allowing HTTP traffic on port 80).
  - **Outbound rules**: Define the traffic allowed **out of** the instance (e.g., allowing internet access).

#### **Use Case for Security Groups**:
Security Groups are typically used to control traffic to and from specific EC2 instances. For example, you might use a security group to:
- Allow inbound SSH traffic (port 22) from specific IP addresses.
- Allow inbound HTTP and HTTPS traffic to your web server instances.
- Restrict outbound traffic to only certain services.

---

### **Example of a Security Group Configuration**:

```bash
# Example of a Security Group that allows inbound SSH and HTTP traffic
resource "aws_security_group" "web_sg" {
  name = "web-server-sg"

  # Inbound Rules
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["203.0.113.0/24"]  # Allow SSH from a specific IP range
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]  # Allow HTTP traffic from anywhere
  }

  # Outbound Rules
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]  # Allow all outbound traffic
  }
}
```

In this example, the security group allows **SSH (port 22)** access from a specific IP range, **HTTP (port 80)** traffic from anywhere, and all **outbound traffic**.

---

### **Network Access Control Lists (NACLs)**

NACLs are **stateless** and operate at the **subnet level**. They act as a firewall to control both **inbound and outbound traffic** to and from the entire subnet. NACLs require you to explicitly allow or deny traffic, and because they are stateless, return traffic must also be explicitly allowed in both directions.

#### **Key Features of NACLs**:
- **Stateless**: You need to create rules for both **inbound** and **outbound** traffic explicitly. For example, if you allow inbound HTTP traffic, you must also allow outbound HTTP traffic for the responses.
- **Applied at the subnet level**: NACLs are attached to subnets, and all instances within that subnet are subject to the NACL's rules.
- **Rules are evaluated in order**: Rules are numbered and evaluated from lowest to highest. The first rule that matches is applied, and the rest are ignored.
- **Allow/Deny**: You can explicitly **allow** or **deny** traffic, giving you more control over traffic than security groups, which can only allow traffic.

#### **Use Case for NACLs**:
NACLs are typically used for broader security control at the **subnet level**. For example, you might use NACLs to:
- Allow all web traffic (HTTP/HTTPS) to a public subnet.
- Deny all traffic from a known malicious IP range.
- Provide an additional layer of security to control traffic to and from an entire subnet.

---

### **Example of a NACL Configuration**:

```bash
# Example of a NACL that allows inbound HTTP traffic and denies SSH traffic
resource "aws_network_acl" "my_nacl" {
  vpc_id = "vpc-12345678"  # Attach NACL to the appropriate VPC

  # Inbound Rules
  ingress {
    rule_no    = 100
    protocol   = "tcp"
    action     = "allow"
    cidr_block = "0.0.0.0/0"
    from_port  = 80
    to_port    = 80  # Allow HTTP traffic
  }

  ingress {
    rule_no    = 200
    protocol   = "tcp"
    action     = "deny"
    cidr_block = "0.0.0.0/0"
    from_port  = 22
    to_port    = 22  # Deny SSH traffic
  }

  # Outbound Rules
  egress {
    rule_no    = 100
    protocol   = "tcp"
    action     = "allow"
    cidr_block = "0.0.0.0/0"
    from_port  = 80
    to_port    = 80  # Allow HTTP outbound responses
  }
}
```

In this example, the NACL:
- **Allows inbound HTTP traffic** (rule 100).
- **Denies inbound SSH traffic** (rule 200).
- **Allows outbound HTTP responses** (rule 100).

---

### **Comparison: When to Use NACLs vs. Security Groups**

- **Security Groups (SGs)** are better suited for controlling traffic to **individual instances** (ENIs) and are typically easier to manage due to their **stateful** nature. They are commonly used in:
  - **Fine-grained access control** at the instance level.
  - Allowing traffic to specific instances (e.g., web servers, application servers).
  - Automatically handling return traffic, which makes them easier to configure for most applications.

- **Network ACLs (NACLs)** provide broader control and are generally used at the **subnet level**. Because they are **stateless**, they are more complex to manage, but they offer greater flexibility for more **complex security rules**, such as:
  - Controlling traffic across an entire **subnet**.
  - Explicitly **denying traffic** from specific IP ranges or regions.
  - Adding an extra layer of security for **highly sensitive subnets** (e.g., those containing sensitive databases).

### **Best Practice**

1. **Security Groups**: Use security groups for **instance-level security** as the primary means of controlling inbound and outbound traffic to/from EC2 instances.
2. **NACLs**: Use NACLs to add an **additional layer of security** at the subnet level, particularly when you need to **explicitly deny** traffic or block traffic at the subnet level.

---

### **Real-World Example: Combining NACLs and Security Groups**

**Scenario**: You're running a multi-tier application in AWS with public-facing web servers and private databases. You want to:
- **Allow** web traffic (HTTP/HTTPS) to your web servers.
- **Block** SSH access to the web servers from the public internet.
- **Allow** database access from your web servers but **block** all other traffic to the database subnet.

**Solution**:
1. **Security Groups**:
   - For the **web servers**:
     - Allow inbound HTTP/HTTPS (ports 80/443) from the internet.
     - Deny inbound SSH (port 22) from the public internet.
   - For the **database servers**:
     - Allow inbound MySQL (port 3306) from the web servers' security group.
     - Deny all other traffic.
   
2. **NACLs**:
   - For the **public subnet**
