### **Lab: Configuring Path-Based Routing with AWS Application Load Balancer (NLB)**

In this lab, we will demonstrate how to configure **Path-based routing** using an **Application Load Balancer (ALB)**, the below steps will guide you to set up an **ALB** with multiple EC2 instances as targets for different path-based routing rules. If no path is provided, the default page will be served.

---

### **Objective:**
- Deploy a sample web application across multiple EC2 instances.
- Configure an Application Load Balancer (ALB) to route requests based on the URL path.
- Create multiple target groups for different EC2 instances.
- Set a default page if no specific path is provided.

---

### **Architecture Overview:**
- **ALB**: Handles HTTP traffic and routes based on URL paths.
- **EC2 Instances**: Two or more instances running simple web applications.
- **Target Groups**: Each EC2 instance will belong to a different target group, and requests will be routed based on URL paths.

---

### **Step-by-Step Lab Guide**

#### **Step 1: Launch EC2 Instances**

1. **Log in to AWS Management Console** and navigate to the **EC2 Dashboard**.
2. **Launch two or more EC2 instances** using an Amazon Linux or Ubuntu AMI.
3. In the **User Data** field for each instance, use the following script to deploy a basic web application.

   **For EC2 Instance 1 (handles `/app1` path)**:
   ```bash
   #!/bin/bash
   yum update -y
   yum install -y httpd
   systemctl start httpd
   systemctl enable httpd
   echo "<html><body><h1>Welcome to Application 1</h1></body></html>" > /var/www/html/index.htm

   ```

   **For EC2 Instance 2 (handles `/app2` path)**:
   ```bash
   #!/bin/bash
   yum update -y
   yum install -y httpd
   systemctl start httpd
   systemctl enable httpd
   echo "<html><body><h1>Welcome to Application 2</h1></body></html>" > /var/www/html/index.html
   ```

4. Ensure both instances are in the same **VPC** and have **public IP addresses** enabled.

#### **Step 2: Configure Security Group for EC2 Instances**

1. Create a **Security Group** that allows:
   - **Inbound HTTP (port 80)** traffic from any IP (`0.0.0.0/0`).
   - **Outbound traffic** to any IP.

2. Attach this Security Group to both EC2 instances.

#### **Step 3: Create Target Groups**

1. In the **EC2 Dashboard**, navigate to **Target Groups** and click **Create target group**.
   
   - **Target Group 1**: 
     - Name: `app1-target-group`.
     - Target Type: **Instance**.
     - Protocol: **HTTP**.
     - Port: **80**.
     - VPC: Select the same VPC as your EC2 instances.
   
   - **Target Group 2**:
     - Name: `app2-target-group`.
     - Target Type: **Instance**.
     - Protocol: **HTTP**.
     - Port: **80**.
     - VPC: Select the same VPC as your EC2 instances.

2. **Register Targets**:
   - For `app1-target-group`, select the EC2 instance running **Application 1**.
   - For `app2-target-group`, select the EC2 instance running **Application 2**.
   
3. **Health Check Configuration**: Leave the default settings (HTTP and /).

#### **Step 4: Create an Application Load Balancer (ALB)**

1. In the **EC2 Dashboard**, go to **Load Balancers** and click **Create Load Balancer**.
2. Select **Application Load Balancer**.
3. Configure the ALB:
   - **Name**: `PathBasedALB`.
   - **Scheme**: Internet-facing.
   - **IP Address Type**: IPv4.
   - **Listener**: HTTP on port 80.
   - **Availability Zones**: Select the VPC and Availability Zones where your EC2 instances are located.
4. Configure a **Security Group** for the ALB:
   - Create or select a security group that allows **inbound HTTP (port 80)** traffic.

#### **Step 5: Configure Routing Rules**

1. After creating the load balancer, configure the routing rules.
   
   **Default Action** (for no path):
   - Select **app1-target-group** as the default target group.
   
   **Path-Based Routing**:
   - **Rule 1**:
     - Path: `/app1`
     - Action: Forward traffic to `app1-target-group`.
   - **Rule 2**:
     - Path: `/app2`
     - Action: Forward traffic to `app2-target-group`.

2. To add rules:
   - Navigate to **Listeners** in the load balancer configuration.
   - Edit the listener rules and add new path-based routing rules.

#### **Step 6: Test Your Application**

1. Once the ALB is active, copy its **DNS name** from the Load Balancer description.
2. Test the path-based routing by visiting:
   - **Default Page**: `http://<ALB-DNS-name>/` – You should see **Application 1's** page.
   - **App 1**: `http://<ALB-DNS-name>/app1` – You should see **Application 1**'s page.
   - **App 2**: `http://<ALB-DNS-name>/app2` – You should see **Application 2**'s page.

---

### **Expected Output**

- Visiting the ALB’s DNS without any path (e.g., `/`) should display **Application 1**'s webpage.
- Visiting `/app1` should display **Application 1**'s webpage.
- Visiting `/app2` should display **Application 2**'s webpage.

---

### **Cleanup**

To avoid incurring costs, terminate the EC2 instances, delete the load balancer, and remove the target groups once the lab is complete.

---

This lab demonstrates how to configure an **Application Load Balancer (ALB)** for **path-based routing** using multiple EC2 instances as targets. You can extend this setup by adding more paths or custom rules based on different criteria (e.g., host-based routing, HTTP headers).
