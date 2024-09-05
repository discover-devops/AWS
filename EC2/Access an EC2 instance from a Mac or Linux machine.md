To access an EC2 instance from a **Mac** or **Linux** machine, follow these steps:

### **Prerequisites:**
1. You need the **.pem** private key file that you downloaded when creating the EC2 instance.
2. Your instance must have **SSH** access enabled (port 22 should be open in the Security Group settings).

### **Step 1: Open Terminal**
- On both **Mac** and **Linux**, open the terminal to execute commands.

### **Step 2: Change Permissions for Your Private Key**
1. Ensure your private key file (.pem) has the correct permissions. Run the following command to make sure it’s not publicly viewable:
   ```bash
   chmod 400 /path/to/your-key.pem
   ```
   Replace `/path/to/your-key.pem` with the actual path to your downloaded key file.

### **Step 3: Get the Public IP Address or DNS of Your Instance**
1. Go to the **EC2 Dashboard** in the AWS Management Console.
2. Find your instance and copy its **Public IP address** or **Public DNS** (available in the instance description).

### **Step 4: Connect to Your EC2 Instance**
1. Use the following command to SSH into your EC2 instance:
   ```bash
   ssh -i /path/to/your-key.pem ec2-user@your-instance-public-dns
   ```
   Replace the following:
   - `/path/to/your-key.pem`: The path to your private key.
   - `your-instance-public-dns`: The Public DNS or IP address of your EC2 instance.

2. If you’re using an **Ubuntu EC2 instance**, the username is `ubuntu`, so the command would be:
   ```bash
   ssh -i /path/to/your-key.pem ubuntu@your-instance-public-dns
   ```

### Example:

If your private key file is called **my-key.pem** and the public IP of your EC2 instance is **203.0.113.25**, the SSH command would be:
```bash
ssh -i ~/Downloads/my-key.pem ec2-user@203.0.113.25
```

### **Step 5: Accept the SSH Key Fingerprint**
1. The first time you connect to the instance, you’ll get a message asking to confirm the fingerprint of the server. Type **yes** and press Enter.

### **Step 6: You Are Now Logged In**
You should now be logged into your EC2 instance!

### Troubleshooting:
- Ensure that your **Security Group** allows SSH (port 22) from your IP address.
- Make sure you’re using the correct **username** (`ec2-user` for Amazon Linux, `ubuntu` for Ubuntu, etc.).
