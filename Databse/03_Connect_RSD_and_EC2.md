### LAB: Launch AWS MySQL RDS and Connect It with Amazon Linux EC2 Instance

This lab guides you through the process of launching an AWS MySQL RDS instance and connecting it to an Amazon Linux EC2 instance. By the end of this lab, you will have a MySQL RDS instance accessible from an EC2 instance.

![image](https://github.com/user-attachments/assets/826fba50-42b1-4031-9d9a-227778bf401f)



---

### **Step 1: Launch a MySQL RDS Instance**

1. **Login to AWS Management Console:**
   - Navigate to the [AWS Console](https://aws.amazon.com/console/), and open the **RDS Console**.

2. **Create a New RDS Instance:**
   - In the **RDS Console**, click on **Databases** in the left-hand menu.
   - Click **Create database**.

3. **Select the Database Creation Method:**
   - Choose **Standard Create** under **Database creation method**.

4. **Choose Database Engine:**
   - Under **Engine options**, select **MySQL**.

5. **Database Settings:**
   - Choose **Version**: Use the default version or choose the one you prefer (e.g., MySQL 8.x).
   - **Templates**: Select **Free Tier** if applicable to avoid additional charges.
   - **DB Instance Identifier**: Name your database (e.g., `my-rds-mysql`).
   - **Master Username**: Enter a username (e.g., `admin`).
   - **Master Password**: Enter a secure password.

6. **Instance Specifications:**
   - **DB Instance Class**: Select the instance type (e.g., `db.t3.micro` for free tier).
   - **Allocated Storage**: Set the default storage (e.g., 20 GB).

7. **Storage Settings:**
   - Leave the default settings unless you want to enable **Auto Scaling**.

8. **Connectivity:**
   - **VPC**: Choose the default VPC.
   - **Subnet Group**: Choose the default subnet.
   - **Publicly Accessible**: Set to **Yes** to allow external access (for this lab).
   - **VPC Security Group**: You can either use the default security group or create a new one.

9. **Database Authentication and Encryption:**
   - Keep the default settings for **Database Authentication** (Password authentication).
   - For this lab, keep encryption disabled.

10. **Backup and Monitoring:**
   - Set a **Backup Retention Period** (e.g., 7 days).
   - Enable or disable monitoring as per your preference.

11. **Create the Database:**
   - Review all the settings, and click **Create Database**.

12. **Wait for Database Creation:**
   - It may take several minutes for the RDS instance to be available.

---

### **Step 2: Launch an Amazon Linux EC2 Instance**

1. **Open EC2 Management Console:**
   - Navigate to the [EC2 Console](https://console.aws.amazon.com/ec2/).

2. **Launch Instance:**
   - Click **Launch Instance**.
   
3. **Choose AMI:**
   - Select **Amazon Linux 2023** AMI (or any preferred Amazon Linux version).

4. **Choose Instance Type:**
   - Select `t2.micro` (free tier eligible).

5. **Configure Instance Details:**
   - Ensure the EC2 instance is launched in the same **VPC** and **subnet** as the RDS instance for connectivity.

6. **Add Storage:**
   - Use the default storage settings or customize as needed.

7. **Configure Security Group:**
   - Create a new security group with the following rules:
     - **Inbound Rule**: Allow **SSH** (Port 22) from your IP (to connect to the instance).
     - **Inbound Rule**: Allow **MySQL/Aurora** (Port 3306) from the security group attached to your EC2 instance.

8. **Key Pair:**
   - Choose an existing key pair or create a new one to access the EC2 instance.

9. **Launch the EC2 Instance:**
   - Click **Launch** and wait for the EC2 instance to be available.

---

### **Step 3: Configure Security Groups for RDS and EC2**

1. **RDS Security Group:**
   - Go to the **EC2 Console**, click on **Security Groups** under **Network & Security**.
   - Find the security group attached to your RDS instance and add an inbound rule:
     - **Type**: MySQL/Aurora
     - **Port**: 3306
     - **Source**: The security group of your EC2 instance (select from dropdown).

2. **EC2 Security Group:**
   - Ensure that the security group attached to your EC2 instance has an outbound rule that allows all traffic (default setting).

---

### **Step 4: Connect to EC2 Instance**

1. **Connect to Your EC2 Instance:**
   - In the EC2 Console, select your instance, and click **Connect**.
   - Use the SSH client from your terminal to connect to the instance:
     ```bash
     ssh -i /path/to/your-key.pem ec2-user@your-ec2-public-ip
     ```
   - Make sure to replace `/path/to/your-key.pem` with the path to your key file and `your-ec2-public-ip` with the EC2 instance's public IP.

---

### **Step 5: Install MySQL Client on EC2 Instance**

1. **Install MySQL Client:**
   - After connecting to the EC2 instance, run the following commands to install the MySQL client:
     ```bash
     sudo yum update -y
     sudo yum install mysql -y
     ```

2. **Verify MySQL Installation:**
   - Run the following command to ensure the MySQL client is installed:
     ```bash
     mysql --version
     ```

---

### **Step 6: Connect to the MySQL RDS from EC2**

1. **Get RDS Endpoint:**
   - In the RDS console, navigate to your MySQL instance.
   - Copy the **Endpoint** from the **Connectivity & security** tab (e.g., `my-rds-mysql.xxxxxxx.us-east-1.rds.amazonaws.com`).

2. **Connect to RDS MySQL:**
   - From the EC2 instance terminal, use the following command to connect to the RDS instance:
     ```bash
     mysql -h your-rds-endpoint -u admin -p
     ```
   - Replace `your-rds-endpoint` with the actual endpoint of your RDS instance, and `admin` with your master username.
   - Enter the password when prompted.

3. **Test the Connection:**
   - Once logged in, you can run the following command to show the databases:
     ```sql
     SHOW DATABASES;
     ```
   - You should see the default databases like `information_schema`, `mysql`, etc.

---

### **Step 7: Create a Sample Database and Table**

1. **Create a New Database:**
   ```sql
   CREATE DATABASE mydb;
   ```

2. **Switch to the New Database:**
   ```sql
   USE mydb;
   ```

3. **Create a Table:**
   ```sql
   CREATE TABLE employees (
       id INT AUTO_INCREMENT PRIMARY KEY,
       name VARCHAR(100),
       position VARCHAR(100),
       salary DECIMAL(10,2)
   );
   ```

4. **Insert Data:**
   ```sql
   INSERT INTO employees (name, position, salary) VALUES 
   ('John Doe', 'Developer', 75000),
   ('Jane Smith', 'Manager', 85000);
   ```

5. **Verify Data Insertion:**
   ```sql
   SELECT * FROM employees;
   ```

---

### **Step 8: Cleanup (Optional)**

1. **Delete RDS Instance:**
   - Go to the **RDS Console**, select your database, and click **Actions** -> **Delete**.
   - You can choose whether or not to create a final snapshot before deletion.

2. **Terminate EC2 Instance:**
   - In the **EC2 Console**, select your instance, click **Actions**, and choose **Terminate**.

---

### **Conclusion**

In this lab, you successfully launched an Amazon RDS MySQL instance, created an Amazon Linux EC2 instance, and connected them. You learned how to set up security groups for secure access and performed database operations through the EC2 instance.


Refrence: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.MySQL.html
