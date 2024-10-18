### Hands-on Lab: Setting Up AWS Transit Gateway for VPC Connectivity

In this hands-on lab, we will demonstrate how to set up AWS Transit Gateway to establish connectivity between multiple VPCs. Our goal is to connect three VPCs and enable communication between them using a Transit Gateway, while configuring the necessary route tables and security groups to allow traffic to flow between the VPCs.

#### Lab Overview:
We will be creating the following architecture:
1. Three VPCs (VPC A, B, and C) with private subnets.
2. Transit Gateway to enable communication between the VPCs.
3. A jump host in the public subnet of VPC A for SSH access to private EC2 instances in all three VPCs.
4. Proper route table configurations for VPCs and subnets to route traffic through the Transit Gateway.

#### Prerequisites:
Before starting this lab, make sure you have:
- AWS account access with required permissions to create VPCs, subnets, EC2 instances, and Transit Gateway.
- Familiarity with basic VPC concepts like route tables, subnets, and EC2 instances.


![image](https://github.com/user-attachments/assets/587c1733-088a-4a8b-9759-eb3f11ebe610)





### Step-by-Step Lab Instructions:

#### Step 1: Create VPCs and Subnets
- **Create VPC A**: Use CIDR block `10.0.0.0/16`.
  - Create two subnets: one **public subnet** (for the jump host) and one **private subnet**.
- **Create VPC B**: Use CIDR block `10.1.0.0/16`.
  - Create a **private subnet** in this VPC.
- **Create VPC C**: Use CIDR block `10.2.0.0/16`.
  - Create a **private subnet** in this VPC.

#### Step 2: Launch EC2 Instances
- **In VPC A**:
  - Launch a jump host in the **public subnet** (Ubuntu or Amazon Linux).
  - Launch an EC2 instance in the **private subnet**.
- **In VPC B and C**:
  - Launch EC2 instances in the **private subnets** of each VPC.
  
Ensure that you enable **SSH and ICMP (ping)** traffic in the security group of these EC2 instances.

#### Step 3: Create AWS Transit Gateway
1. Navigate to **VPC Console** → **Transit Gateways** → **Create Transit Gateway**.
2. Provide a name (e.g., `VPC_A_B_C_TGW`), leave the **ASN** at default, enable **DNS support**, and click **Create**.
   
It may take a minute or two for the Transit Gateway to be created.

#### Step 4: Create Transit Gateway Attachments
1. After creating the Transit Gateway, click on **Create Attachment**.
2. For each VPC (A, B, and C):
   - Select the appropriate **Transit Gateway**.
   - Choose **VPC Attachment**.
   - Choose the VPC (e.g., VPC A), and select the **private subnet** where EC2 instances reside.
   - Create the attachment.

Repeat the process for VPC B and VPC C.

#### Step 5: Configure VPC Route Tables
1. For each VPC’s **private subnet** route table:
   - Go to the **Route Tables** section in the VPC console.
   - Select the private subnet's route table and click **Edit Routes**.
   - Add a route to allow traffic to flow through the Transit Gateway:
     - Destination: `10.0.0.0/8` (covers all three VPC CIDR ranges).
     - Target: Select the **Transit Gateway**.
   - Save the changes.

Repeat this step for the **private subnet route tables** of VPC B and C.

#### Step 6: Test the Connectivity
1. SSH into the **jump host** in VPC A’s public subnet.
2. From the jump host, SSH into the EC2 instance in VPC A’s private subnet.
3. From the EC2 instance in VPC A, **ping** the private IP addresses of EC2 instances in VPC B and C to verify connectivity.

For example:
- `ping <Private_IP_of_VPC_B_EC2>`
- `ping <Private_IP_of_VPC_C_EC2>`

If you receive successful responses, the Transit Gateway has been set up correctly, and VPCs can now communicate via the Transit Gateway.

#### Step 7: Clean Up (Optional)
After completing the lab, you can choose to clean up resources by deleting the Transit Gateway attachments, VPCs, EC2 instances, and subnets if they are no longer needed.

### Conclusion:
In this lab, you have successfully created an AWS Transit Gateway to connect multiple VPCs. We walked through the process of configuring Transit Gateway attachments, updating route tables, and testing communication between EC2 instances across different VPCs. This setup can be extended to larger and more complex architectures to simplify your AWS network design.
