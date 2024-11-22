Assignments for each AWS service:

---

### **AWS EC2 Assignments**

**Assignment 1:**  
- Launch an EC2 instance using the AWS Management Console.  
- Install and configure Apache or NGINX on the instance.  
- Verify the web server is accessible from your browser using the instance's public IP address.  
- Submit a report with the steps, screenshots, and the public IP address.

**Assignment 2:**  
- Create an Amazon EC2 instance using a custom AMI.  
- Attach a new volume to the instance and format it.  
- Mount the volume and store a sample text file in it.  
- Write a document detailing the creation and attachment process.

**Assignment 3:**  
- Configure a Security Group for your EC2 instance to allow access only from a specific IP range.  
- Use the AWS CLI to launch the EC2 instance with the configured Security Group.  
- Document the CLI commands used and provide screenshots of the setup.

---

### **AWS EBS and EC2 Assignments**

**Assignment 1:**  
- Create an EBS volume and attach it to an existing EC2 instance.  
- Format and mount the volume, then write some test data to it.  
- Detach the volume and reattach it to another EC2 instance.  
- Verify the data persists. Submit a report with all steps.

**Assignment 2:**  
- Launch an EC2 instance and configure it to use an EBS volume for storing application logs.  
- Use CloudWatch Logs to monitor the logs in real-time.  
- Document the entire process with screenshots.

**Assignment 3:**  
- Create an EC2 instance with an EBS-backed root volume.  
- Take a snapshot of the EBS volume, then create a new volume from the snapshot.  
- Replace the root volume of the instance with the new volume.  
- Write a report explaining each step and the commands used.

---

### **AWS Load Balancer Assignments**

**Assignment 1:**  
- Set up an Application Load Balancer (ALB) with two EC2 instances in different availability zones.  
- Configure a listener to route traffic to the instances.  
- Test and verify that the ALB distributes traffic evenly.  
- Submit a report with screenshots of the configuration and testing.

**Assignment 2:**  
- Create a Network Load Balancer (NLB) and attach it to two EC2 instances.  
- Use Path-based routing to route traffic to specific instances.  
- Document the steps and test results.

**Assignment 3:**  
- Implement an Auto Scaling Group with an ALB to handle incoming traffic.  
- Configure the ALB to redirect HTTP traffic to HTTPS.  
- Demonstrate scalability by increasing the load and capturing logs from the ALB.  
- Provide a report with configurations and test results.

---

### **AWS IAM Assignments**

**Assignment 1:**  
- Create a custom IAM policy to allow read-only access to S3 buckets.  
- Attach this policy to a new IAM user.  
- Verify the permissions by attempting various S3 operations.  
- Document the steps and the test results.

**Assignment 2:**  
- Create an IAM role for an EC2 instance to access an S3 bucket.  
- Launch an EC2 instance with the role attached.  
- Verify that the instance can list and download objects from the S3 bucket without AWS credentials.  
- Submit a report with all steps.

**Assignment 3:**  
- Set up Multi-Factor Authentication (MFA) for an IAM user.  
- Ensure the user cannot log in without MFA enabled.  
- Document the steps to enable and test MFA with screenshots.

---

### **AWS S3 Assignments**

**Assignment 1:**  
- Create an S3 bucket and enable versioning.  
- Upload multiple versions of a file and test retrieving previous versions.  
- Submit a report with steps and screenshots.

**Assignment 2:**  
- Configure an S3 bucket with lifecycle policies to move files to Glacier after 30 days.  
- Test the lifecycle policy with sample files.  
- Document the configuration and test results.

**Assignment 3:**  
- Set up a public S3 bucket to host a static website.  
- Upload HTML and CSS files, then access the website using the bucket's endpoint.  
- Submit a report with the steps and the website URL.

---

