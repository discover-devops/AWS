### **Amazon EFS (Elastic File System) Tutorial**

### **Introduction to Amazon EFS**

Amazon Elastic File System (EFS) is a fully managed, scalable file storage service provided by AWS for use with Amazon EC2 instances. It is designed to provide a **shared file system** that can be mounted by multiple EC2 instances, allowing them to access the same data concurrently. EFS is ideal for use cases where multiple instances need shared access to data, such as web servers, content management systems, and big data analytics.

**Key Features of Amazon EFS**:
- **Fully Managed**: AWS handles the infrastructure, scaling, and maintenance, so you can focus on your application.
- **Elastic**: EFS automatically scales up or down based on the amount of data stored, so you don’t need to worry about provisioning or managing storage capacity.
- **Concurrent Access**: EFS can be mounted to thousands of EC2 instances, allowing for shared file access.
- **Durability and Availability**: EFS is designed to be highly available and durable, storing data redundantly across multiple Availability Zones (AZs).

---

### **Amazon EFS vs. Amazon EBS**

#### **Amazon EFS (Elastic File System)**

- **Type**: Network file system, designed for sharing between multiple EC2 instances.
- **Use Case**: Ideal for workloads where multiple instances need to read and write from the same file system concurrently (e.g., content management systems, shared development environments, and big data processing).
- **Access**: Supports concurrent access from multiple instances across multiple AZs.
- **Scalability**: Automatically scales based on the amount of data stored.
- **Pricing**: Charged based on the amount of data stored and the throughput/IOPS used.
- **Durability**: Data is stored redundantly across multiple AZs.
- **Performance Modes**: Standard and Max I/O (for highly parallel workloads).

#### **Amazon EBS (Elastic Block Store)**

- **Type**: Block storage, designed for use by a single EC2 instance at a time (although it can be attached to multiple instances in a read-only mode).
- **Use Case**: Suitable for workloads requiring high-performance storage, such as databases, transactional applications, and persistent storage for a single EC2 instance.
- **Access**: Typically attached to a single instance, but can be used in multi-attach mode for specific cases.
- **Scalability**: Requires manual scaling by resizing or adding more volumes.
- **Pricing**: Charged based on the provisioned storage, whether used or not.
- **Durability**: Data is replicated within a single AZ (although you can use snapshots to back up to S3).
- **Performance Modes**: SSD-backed volumes (for performance) and HDD-backed volumes (for throughput).

---

### **Use Cases for Amazon EFS**

1. **Web Hosting and Content Management**: Websites and content management systems (CMS) with shared media files can benefit from using EFS, as it allows multiple web servers to access the same storage.
2. **Shared Development Environments**: EFS is ideal for shared development environments where multiple users or instances need concurrent access to the same files (e.g., source code repositories or shared project files).
3. **Big Data and Analytics**: EFS can store large amounts of data that need to be accessed concurrently by multiple instances for data processing or analytics.
4. **Container Storage**: EFS can be used as a shared file system for containers, allowing them to store and access persistent data across different instances.

---

### **Step-by-Step Tutorial: Setting Up Amazon EFS**

#### **Step 1: Create an Amazon EFS File System**

1. **Log in to AWS Management Console**.
2. Navigate to the **EFS Dashboard**.
3. Click on **Create file system**.
4. Select the **VPC** where your EC2 instances are running.
5. Choose the default options for **General Purpose** performance mode and **Bursting** throughput mode.
6. Review the settings and click **Create**. This will create a new EFS file system.

---

#### **Step 2: Configure the Mount Targets**

Amazon EFS requires **mount targets** in each AZ where you want to access the file system.

1. After creating the file system, you’ll see the **Mount Targets** tab.
2. AWS will automatically create mount targets in each AZ for your VPC. Ensure that your VPC's security group allows NFS traffic (port 2049).
3. Verify that the mount targets are created and available in all the necessary AZs.

---

#### **Step 3: Create EC2 Instances to Access EFS**

1. Go to the **EC2 Dashboard** and launch at least two **EC2 instances** in the same VPC where you created the EFS file system.
2. Choose an Amazon Linux 2 or Ubuntu AMI for the EC2 instances.
3. Ensure that both EC2 instances are in the same VPC and can access the **NFS port** (2049) in their **security groups**.

---

#### **Step 4: Install NFS Utilities on EC2 Instances**

To mount the EFS file system on your EC2 instances, you need to install the **NFS client**.

1. **Connect to the EC2 instance** via SSH.
2. Install the NFS client:
   - For **Amazon Linux 2** or **Red Hat**:
     ```bash
     sudo yum install -y nfs-utils
     ```
   - For **Ubuntu**:
     ```bash
     sudo apt-get install -y nfs-common
     ```

---

#### **Step 5: Mount the EFS File System**

1. Obtain the **EFS file system DNS name** from the **EFS Console**. It will look like this:
   ```
   fs-0123456789abcdef0.efs.us-east-1.amazonaws.com:/ 
   ```

2. Mount the file system on each EC2 instance:
   ```bash
   sudo mount -t nfs4 -o nfsvers=4.1 fs-0123456789abcdef0.efs.us-east-1.amazonaws.com:/ /mnt/efs
   ```

3. Verify the mount:
   ```bash
   df -h
   ```
   You should see the mounted EFS file system under `/mnt/efs`.

4. Repeat the same steps on all EC2 instances to mount the EFS file system.

---

#### **Step 6: Test the Shared File System**

1. On one EC2 instance, create a test file:
   ```bash
   sudo touch /mnt/efs/testfile.txt
   ```

2. On the other EC2 instance, check if the file is visible:
   ```bash
   ls /mnt/efs
   ```

You should see the `testfile.txt` created from the other EC2 instance, confirming that the file system is shared across multiple instances.

---

#### **Step 7: (Optional) Automatically Mount EFS on Reboot**

To ensure the EFS file system is mounted automatically after the EC2 instance reboots, you need to edit the **/etc/fstab** file.

1. Open the **/etc/fstab** file on each EC2 instance:
   ```bash
   sudo nano /etc/fstab
   ```

2. Add the following entry:
   ```
   fs-0123456789abcdef0.efs.us-east-1.amazonaws.com:/ /mnt/efs nfs4 defaults,_netdev 0 0
   ```

3. Save the file and exit.

Now, the EFS file system will be automatically mounted each time the EC2 instance starts.

---

### **Conclusion**

Amazon EFS provides a scalable and highly available file system for workloads that require shared access to file data across multiple EC2 instances. It is ideal for scenarios like content management, shared development environments, and data analysis.

In contrast, Amazon EBS is more suitable for single-instance use cases that require high-performance block storage, such as databases and transactional applications.

By following this tutorial, you now know how to set up an Amazon EFS file system, mount it on EC2 instances, and configure automatic mounting on reboot. Let me know if you need any further clarifications or advanced configurations!
