### **How to Install AWS CLI on Different Operating Systems**

The AWS Command Line Interface (CLI) allows you to interact with AWS services from the command line. You can install and configure the AWS CLI on various operating systems such as Linux, macOS, and Windows.

---

### **1. Installation of AWS CLI on Different Operating Systems**

#### **1.1 Installing AWS CLI on Linux**

1. **Download the AWS CLI Installer**:
   
   Use the following command to download the AWS CLI version 2 installation file:
   ```bash
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   ```

2. **Unzip the Installation Package**:
   ```bash
   unzip awscliv2.zip
   ```

3. **Run the Installer**:
   ```bash
   sudo ./aws/install
   ```

4. **Verify the Installation**:
   To check if AWS CLI was installed correctly, run:
   ```bash
   aws --version
   ```

---

#### **1.2 Installing AWS CLI on macOS**

1. **Download the AWS CLI Installer**:
   
   Use the following command to download AWS CLI for macOS:
   ```bash
   curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
   ```

2. **Run the Installer**:
   Use the built-in macOS installer:
   ```bash
   sudo installer -pkg ./AWSCLIV2.pkg -target /
   ```

3. **Verify the Installation**:
   ```bash
   aws --version
   ```

---

#### **1.3 Installing AWS CLI on Windows**

1. **Download the Installer**:
   Download the AWS CLI MSI installer for Windows from [here](https://awscli.amazonaws.com/AWSCLIV2.msi).

2. **Run the Installer**:
   Run the downloaded `.msi` file and follow the on-screen instructions.

3. **Verify the Installation**:
   Open the **Command Prompt** or **PowerShell** and run:
   ```bash
   aws --version
   ```

---

### **2. How to Configure AWS CLI**

After installing the AWS CLI, you need to configure it with your **AWS credentials** and **region**.

1. **Open the Terminal or Command Prompt** and run the following command:
   ```bash
   aws configure
   ```

2. **Enter the following details** when prompted:

   - **AWS Access Key ID**: Enter your access key.
   - **AWS Secret Access Key**: Enter your secret access key.
   - **Default Region Name**: Enter your region (e.g., `us-west-2` or `ap-south-2`).
   - **Default Output Format**: Choose the output format (`json`, `text`, or `table`).

   Example:
   ```bash
   AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
   AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   Default region name [None]: us-west-2
   Default output format [None]: json
   ```

   **Note**: You can create access keys from the AWS Management Console under **IAM**.

---

### **3. Real-World Examples: Creating Resources Using AWS CLI**

Once AWS CLI is configured, you can create and manage AWS resources. Below are real-world examples of creating commonly used AWS resources.

---

#### **Example 1: Launching an EC2 Instance Using AWS CLI**

1. **Create a Key Pair**:
   To securely connect to an EC2 instance, you need a key pair.
   ```bash
   aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem
   ```

   **Note**: The `.pem` file is your private key, and it must be kept secure.

2. **Launch an EC2 Instance**:
   Launch an EC2 instance using the Amazon Linux 2 AMI and the key pair you just created.
   ```bash
   aws ec2 run-instances \
   --image-id ami-0123abcd1234abcd1 \
   --instance-type t2.micro \
   --key-name MyKeyPair \
   --security-group-ids sg-0123abcd1234abcd1 \
   --subnet-id subnet-0123abcd1234abcd1 \
   --count 1 \
   --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=MyFirstEC2Instance}]'
   ```

   - **image-id**: The AMI ID for the operating system you want to run.
   - **instance-type**: The type of instance (e.g., `t2.micro` for the free tier).
   - **key-name**: The name of the key pair you created.
   - **security-group-ids**: The security group that allows SSH/HTTP traffic.
   - **subnet-id**: The subnet in which the instance will be launched.
   - **count**: The number of instances to launch.
   - **tag-specifications**: Tags applied to your instance.

3. **Verify the EC2 Instance**:
   List all running EC2 instances:
   ```bash
   aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,State.Name,PublicIpAddress,Tags[?Key==`Name`].Value]' --output table
   ```

---

#### **Example 2: Creating an S3 Bucket Using AWS CLI**

1. **Create an S3 Bucket**:
   ```bash
   aws s3api create-bucket --bucket my-unique-bucket-name --region us-west-2
   ```

   **Note**: Bucket names must be globally unique.

2. **Upload a File to the S3 Bucket**:
   ```bash
   aws s3 cp myfile.txt s3://my-unique-bucket-name/
   ```

3. **List Files in the S3 Bucket**:
   ```bash
   aws s3 ls s3://my-unique-bucket-name/
   ```

4. **Make the File Public**:
   Make an object publicly accessible:
   ```bash
   aws s3api put-object-acl --bucket my-unique-bucket-name --key myfile.txt --acl public-read
   ```

5. **Delete the S3 Bucket**:
   Before deleting the bucket, you need to empty it:
   ```bash
   aws s3 rm s3://my-unique-bucket-name/ --recursive
   ```

   Then delete the bucket:
   ```bash
   aws s3api delete-bucket --bucket my-unique-bucket-name --region us-west-2
   ```

---

#### **Example 3: Creating an RDS Database Using AWS CLI**

1. **Create a DB Subnet Group**:
   RDS instances must be launched in a subnet group.
   ```bash
   aws rds create-db-subnet-group \
   --db-subnet-group-name my-subnet-group \
   --db-subnet-group-description "My DB subnet group" \
   --subnet-ids subnet-0123456789abcdefg subnet-abcdefg0123456789
   ```

2. **Launch an RDS MySQL Database**:
   ```bash
   aws rds create-db-instance \
   --db-instance-identifier mydbinstance \
   --db-instance-class db.t2.micro \
   --engine mysql \
   --allocated-storage 20 \
   --master-username admin \
   --master-user-password password123 \
   --vpc-security-group-ids sg-0123456789abcdefg \
   --db-subnet-group-name my-subnet-group \
   --backup-retention-period 7
   ```

   - **db-instance-identifier**: The unique name for your RDS instance.
   - **engine**: The database engine to use (e.g., MySQL).
   - **allocated-storage**: The amount of storage (in GB) to allocate for the database.
   - **master-username**: The admin username.
   - **master-user-password**: The password for the master user.
   - **vpc-security-group-ids**: The security group allowing access to the DB.
   - **db-subnet-group-name**: The DB subnet group created earlier.

3. **List RDS Instances**:
   ```bash
   aws rds describe-db-instances --query 'DBInstances[*].[DBInstanceIdentifier,DBInstanceStatus,Endpoint.Address,AllocatedStorage]'
   ```

4. **Delete the RDS Instance**:
   ```bash
   aws rds delete-db-instance --db-instance-identifier mydbinstance --skip-final-snapshot
   ```

---

### **Example 4: Create a VPC Using AWS CLI**

1. **Create a VPC**:
   ```bash
   aws ec2 create-vpc --cidr-block 10.0.0.0/16
   ```

2. **Create a Subnet**:
   ```bash
   aws ec2 create-subnet --vpc-id vpc-0123456789abcdef --cidr-block 10.0.1.0/24
   ```

3. **Create an Internet Gateway**:
   ```bash
   aws ec2 create-internet-gateway
   ```

4. **Attach the Internet Gateway to the VPC**:
   ```bash
   aws ec2 attach-internet-gateway --vpc-id vpc-0123456789abcdef --internet-gateway-id ig


---

To list all EC2 instances using the AWS CLI, you can use the `describe-instances` command. This command retrieves details about all EC2 instances in your AWS account within the default region or the region you specify.

### **Basic Command to List EC2 Instances**

```bash
aws ec2 describe-instances
```

This command will return a detailed JSON output of all EC2 instances, including information such as instance IDs, state, availability zones, public IP addresses, and more.

---

### **Filter and Format the Output**

To make the output more readable and get specific details, you can use the `--query` and `--output` options.

#### **Example 1: List All EC2 Instances with Instance IDs and State**

```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].[InstanceId,State.Name]" --output table
```

This will output a table with the instance IDs and their current state (e.g., running, stopped).

#### **Example 2: List All EC2 Instances with Public IPs and Instance IDs**

```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].[InstanceId,PublicIpAddress]" --output table
```

This will output a table with instance IDs and their associated public IP addresses.

#### **Example 3: List All EC2 Instances with Tags, Instance IDs, and State**

```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].[InstanceId,State.Name,Tags[?Key=='Name'].Value | [0]]" --output table
```

This command will list the instance ID, the state (running, stopped), and the `Name` tag for each EC2 instance.

#### **Example 4: List EC2 Instances by a Specific Tag**

To list instances that have a specific tag (e.g., instances tagged as `Environment=Production`):

```bash
aws ec2 describe-instances --filters "Name=tag:Environment,Values=Production" --query "Reservations[*].Instances[*].[InstanceId,State.Name,Tags[?Key=='Name'].Value | [0]]" --output table
```

This will list all EC2 instances with the `Environment=Production` tag.

---

### **Use Case: Listing Running EC2 Instances**

To list only the EC2 instances that are currently **running**, you can filter by instance state:

```bash
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --query "Reservations[*].Instances[*].[InstanceId,State.Name,PublicIpAddress]" --output table
```

This will output a table of running instances with their instance IDs, state, and public IP addresses.

---

### **Additional Options**

- **Specify Region**: If you want to list EC2 instances in a specific AWS region, add the `--region` flag:

  ```bash
  aws ec2 describe-instances --region ap-south-2
  ```

- **Output Formats**: You can change the output format using the `--output` flag (JSON, table, text). For example:

  ```bash
  aws ec2 describe-instances --output json
  ```

This allows you to view EC2 instances in a structured format that's easier to parse programmatically.

---

### **Summary**

- Use `aws ec2 describe-instances` to list all EC2 instances.
- You can filter and format the output using the `--query` and `--output` options to focus on specific fields such as instance IDs, public IPs, state, and tags.
- Use filters to display instances based on state (e.g., running) or tags.



---


