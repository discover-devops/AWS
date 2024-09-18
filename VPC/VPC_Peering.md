### **Step-by-Step Guide to Peer Two VPCs in AWS (ap-south-2 Region)**

In this document, you will learn how to **peer two VPCs** (`VPC A` with CIDR `192.68.0.0/24` and `VPC B` with CIDR `10.0.0.0/24`) in the **ap-south-2 region** (Asia Pacific - Hyderabad). The peering connection will allow instances in each VPC to communicate with each other as if they were part of the same network.


![image](https://github.com/user-attachments/assets/369038ba-6f9e-4a68-ac2c-34d2da82fbac)




---

### **Prerequisites:**
- You have two VPCs:
  1. **VPC A**: `192.68.0.0/24` with `subnet_A`.
  2. **VPC B**: `10.0.0.0/24` with `subnet_B`.

- Both VPCs are located in the **ap-south-2 region**.

### **Overview of the Process:**
1. **Create a VPC Peering connection** between the two VPCs.
2. **Accept the VPC Peering connection**.
3. **Update Route Tables** in both VPCs to route traffic between them.
4. **Update Security Groups** in both VPCs to allow communication.
5. **Test the peering connection** to ensure traffic can flow between VPC A and VPC B.

---

### **Step 1: Create a VPC Peering Connection**

1. **Log in to AWS Management Console**:
   - Go to **VPC Dashboard**.

2. **Navigate to Peering Connections**:
   - On the left-hand panel, under **Peering Connections**, click **Create Peering Connection**.

3. **Configure the Peering Connection**:
   - **Name tag**: `VPC-A-to-VPC-B`
   - **VPC Requester**: Select **VPC A** (`192.68.0.0/24`).
   - **VPC Accepter**:
     - If **VPC B** is in the same AWS account: Select **VPC B** (`10.0.0.0/24`).
     - If **VPC B** is in a different AWS account: Enter the AWS account ID and select **VPC B** in that account.

4. **Region**:
   - Ensure both VPCs are in the **ap-south-2** region.

5. **CIDR Block Overlap Check**:
   - Ensure that the CIDR blocks do not overlap (they are different, which they are in your case).

6. **Create the Peering Connection**:
   - After reviewing the configuration, click **Create Peering Connection**.
   - You should see a status of **Pending Acceptance**.

---

### **Step 2: Accept the VPC Peering Connection**

1. **Accept the Peering Connection**:
   - If VPC B is in the same account:
     - Go to **Peering Connections**.
     - Select the newly created connection (`VPC-A-to-VPC-B`).
     - Click **Actions** and choose **Accept Request**.
   - If VPC B is in a different account:
     - Log in to the AWS account that owns VPC B.
     - Navigate to **Peering Connections**.
     - Select the pending request and click **Accept Request**.

2. **Verify**:
   - Once accepted, the status of the peering connection should change to **Active**.

---

### **Step 3: Update Route Tables for Communication**

To allow traffic to flow between the VPCs, you need to update the route tables in both VPC A and VPC B.

#### **Update Route Table for VPC A (192.68.0.0/24)**:

1. **Go to VPC Dashboard**:
   - Under **VPC > Route Tables**, find the route table associated with **subnet_A** in VPC A.

2. **Add a Route**:
   - Click **Edit routes**.
   - Add a route with the following settings:
     - **Destination**: `10.0.0.0/24` (CIDR of VPC B).
     - **Target**: Select the **VPC peering connection** you created earlier (`VPC-A-to-VPC-B`).
   - Click **Save routes**.

#### **Update Route Table for VPC B (10.0.0.0/24)**:

1. **Go to VPC Dashboard**:
   - Under **VPC > Route Tables**, find the route table associated with **subnet_B** in VPC B.

2. **Add a Route**:
   - Click **Edit routes**.
   - Add a route with the following settings:
     - **Destination**: `192.68.0.0/24` (CIDR of VPC A).
     - **Target**: Select the **VPC peering connection** (`VPC-A-to-VPC-B`).
   - Click **Save routes**.

---

### **Step 4: Update Security Groups**

Both VPCs must allow traffic between their respective EC2 instances. By default, security groups may block this traffic.

#### **Update Security Group for EC2 Instances in VPC A**:

1. **Go to EC2 Dashboard**:
   - Under **Security Groups**, find the security group associated with the instances in **VPC A**.

2. **Edit Inbound Rules**:
   - Click **Edit inbound rules**.
   - Add a rule to allow traffic from VPC B:
     - **Type**: All Traffic (or the specific type of traffic you need, such as HTTP, SSH, etc.)
     - **Source**: `10.0.0.0/24` (CIDR of VPC B).
   - Click **Save rules**.

#### **Update Security Group for EC2 Instances in VPC B**:

1. **Go to EC2 Dashboard**:
   - Under **Security Groups**, find the security group associated with the instances in **VPC B**.

2. **Edit Inbound Rules**:
   - Click **Edit inbound rules**.
   - Add a rule to allow traffic from VPC A:
     - **Type**: All Traffic (or the specific type of traffic you need, such as HTTP, SSH, etc.)
     - **Source**: `192.68.0.0/24` (CIDR of VPC A).
   - Click **Save rules**.

---

### **Step 5: Test the VPC Peering Connection**

1. **Launch EC2 Instances**:
   - Ensure you have EC2 instances running in **VPC A** (subnet_A) and **VPC B** (subnet_B).

2. **Test Connectivity**:
   - Use **ping** or **SSH** from an instance in **VPC A** to an instance in **VPC B**.
   - Example:
     - If the private IP of an EC2 instance in VPC B is `10.0.0.10`, SSH from VPC A to VPC B using:
       ```bash
       ssh ec2-user@10.0.0.10
       ```
   - Similarly, try accessing resources in **VPC B** from **VPC A** to confirm that the peering connection is functioning correctly.

---

### **Additional Configuration (Optional)**

1. **DNS Resolution**:
   - To enable **DNS resolution** between VPCs, go to **VPC Peering Connections**.
   - Select the peering connection (`VPC-A-to-VPC-B`).
   - Under the **Actions** menu, choose **Modify DNS settings** and enable DNS resolution for both requester and accepter VPCs.

2. **Cross-Region Peering**:
   - If you need to peer VPCs across regions, AWS supports **Inter-Region VPC Peering**, but for this lab, both VPCs are in the same region.

---

### **Conclusion**

You have successfully created a **VPC Peering Connection** between two VPCs (`VPC A` and `VPC B`) in the **ap-south-2 region**, updated the necessary **route tables** and **security groups**, and verified that instances in both VPCs can communicate with each other.

By following these steps, you can establish secure communication between your VPCs and enable cross-VPC workloads in AWS.
