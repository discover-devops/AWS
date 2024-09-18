### Deploying Multiple Applications with Path-Based Routing Using AWS Application Load Balancer (ALB)**

In this lab, we will deploy two applications (`movies.html` and `songs.html`) across two separate EC2 instances, set up an **Application Load Balancer (ALB)**, and configure **path-based routing** to route traffic to the respective applications. If no path is provided, a default page will be displayed.



![image](https://github.com/user-attachments/assets/4de11c1e-2a1e-4273-af35-87dcc2c0c257)



---

### **Objective:**
- Deploy two EC2 instances, each hosting a different application (`movies.html` and `songs.html`).
- Create target groups for each EC2 instance.
- Set up an Application Load Balancer (ALB) to route requests based on URL paths (`/movies` and `/songs`).
- Configure a default page for non-specified paths.

---

### **Architecture Overview:**
- **ALB**: Routes HTTP traffic and directs it based on URL paths.
- **EC2 Instances**: Two instances serving different web pages.
- **Target Groups**: Each target group will route traffic to its corresponding EC2 instance.
- **Path-Based Routing**: `/movies` directs to one target group (movies instance) and `/songs` to the other (songs instance).


![image](https://github.com/user-attachments/assets/cd6e8f64-a576-4567-87cd-71a8d44a67e9)





---

### **Step-by-Step Lab Guide**

#### **Step 1: Launch Two EC2 Instances**

1. **Log in to AWS Management Console** and navigate to the **EC2 Dashboard**.
2. **Launch two EC2 instances** using an Amazon Linux AMI.

3. In the **User Data** field for each instance, use the following scripts to deploy the applications.

   **For EC2 Instance 1 (Application 1 - Movies)**:
   ```bash
   #!/bin/bash
   # Update the package repository
   yum update -y
   # Install Apache HTTP Server (httpd)
   yum install -y httpd
   # Start the Apache service
   systemctl start httpd
   # Enable Apache to start on boot
   systemctl enable httpd
   # Create the index.html page
   echo "<html><body><h1>Welcome to Application 1</h1></body></html>" > /var/www/html/index.html
   # Create a movies.html page
   echo "<html><body><h1>Movies Page</h1><p>This is the Movies page content.</p></body></html>" > /var/www/html/movies.html
   ```

   **For EC2 Instance 2 (Application 2 - Songs)**:
   ```bash
   #!/bin/bash
   # Update the package repository
   yum update -y
   # Install Apache HTTP Server (httpd)
   yum install -y httpd
   # Start the Apache service
   systemctl start httpd
   # Enable Apache to start on boot
   systemctl enable httpd
   # Create the index.html page
   echo "<html><body><h1>Welcome to Application 2</h1></body></html>" > /var/www/html/index.html
   # Create a songs.html page
   echo "<html><body><h1>Songs Page</h1><p>This is the Songs page content.</p></body></html>" > /var/www/html/songs.html
   ```

4. Ensure both instances are in the same **VPC** and have **public IP addresses** enabled.

#### **Step 2: Configure Security Groups for EC2 Instances**

1. Create a **Security Group** that allows:
   - **Inbound HTTP (port 80)** traffic from any IP (`0.0.0.0/0`).
   - **Outbound traffic** to any IP.
   
2. Attach this Security Group to both EC2 instances.

#### **Step 3: Create Target Groups**

1. In the **EC2 Dashboard**, navigate to **Target Groups** and click **Create target group**.

   - **Target Group 1 (movies-target-group)**:
     - Name: `movies-target-group`.
     - Target Type: **Instance**.
     - Protocol: **HTTP**.
     - Port: **80**.
     - VPC: Select the VPC where your EC2 instances reside.
     - **Health Check Path**: `/movies.html`.
     - Register **EC2 Instance 1** (Movies instance).
   
   - **Target Group 2 (songs-target-group)**:
     - Name: `songs-target-group`.
     - Target Type: **Instance**.
     - Protocol: **HTTP**.
     - Port: **80**.
     - VPC: Select the same VPC.
     - **Health Check Path**: `/songs.html`.
     - Register **EC2 Instance 2** (Songs instance).

#### **Step 4: Create an Application Load Balancer (ALB)**

1. In the **EC2 Dashboard**, go to **Load Balancers** and click **Create Load Balancer**.
2. Select **Application Load Balancer**.
3. Configure the ALB:
   - **Name**: `MyPathBasedALB`.
   - **Scheme**: Internet-facing.
   - **IP Address Type**: IPv4.
   - **Listeners**: Choose HTTP on port 80.
   - **VPC**: Select the VPC where your EC2 instances are located.
   - **Availability Zones**: Select the Availability Zones that host your EC2 instances.

4. Configure the **Security Group** for the ALB:
   - Ensure the Security Group allows inbound HTTP traffic (port 80).

5. **Default Action**:
   - Select the **movies-target-group** (for EC2 Instance 1) as the default target group.
   - Click **Create Load Balancer**.

#### **Step 5: Configure Path-Based Routing Rules**

1. After the ALB is created, navigate to **Listeners**.
   - Click **View/edit rules** for the HTTP listener (port 80).

2. **Add Path-Based Routing for Movies**:
   - Click **+** to add a rule.
   - **Condition**: Path is `/movies`.
   - **Action**: Forward to `movies-target-group`.
   - Click **Save**.

3. **Add Path-Based Routing for Songs**:
   - Click **+** to add a second rule.
   - **Condition**: Path is `/songs`.
   - **Action**: Forward to `songs-target-group`.
   - Click **Save**.

4. **Default Action**:
   - If no path is specified, the default action will forward the request to the **movies-target-group**.

#### **Step 6: Test Your Application**

1. Once the ALB is fully configured and the EC2 instances are healthy, copy the **DNS name** of the ALB from its description in the console.

2. **Test Access**:
   - **Movies Page**: Visit `http://<ALB-DNS-name>/movies` to see **Application 1**'s `movies.html` page.
   - **Songs Page**: Visit `http://<ALB-DNS-name>/songs` to see **Application 2**'s `songs.html` page.
   - **Default Page**: Visit `http://<ALB-DNS-name>/` to see **Application 1**'s default `index.html`.

---

### **Expected Output:**

- **Default Page (`/`)**: Displays the **index.html** from the movies instance.
- **Movies Page (`/movies`)**: Displays **Application 1**'s `movies.html` page.
- **Songs Page (`/songs`)**: Displays **Application 2**'s `songs.html` page.

---

### **Summary of Configuration:**

- **ALB**: Internet-facing Application Load Balancer with HTTP listener on port 80.
- **Target Groups**:
  - `movies-target-group`: Routes traffic for `/movies`.
  - `songs-target-group`: Routes traffic for `/songs`.
- **Path-Based Routing**:
  - `/movies` → `movies-target-group` (EC2 Instance 1).
  - `/songs` → `songs-target-group` (EC2 Instance 2).
  - **Default Action**: Routes to `movies-target-group`.

---

### **Cleanup**

To avoid incurring charges, terminate the EC2 instances, delete the target groups, and remove the ALB once the lab is complete.

---

By following this combined lab, you have deployed two applications across separate EC2 instances and configured **path-based routing** using an **Application Load Balancer (ALB)** in AWS. This setup allows different URL paths to serve different applications or content, ensuring efficient traffic distribution and resource utilization.
