### **What is EC2 User Data?**

**EC2 User Data** refers to a script or set of commands that you can pass to an **Amazon EC2 instance** at launch. The **User Data** feature allows you to automate the initial configuration or setup of an EC2 instance when it first starts. This is commonly used for tasks such as installing software, configuring services, or setting environment variables automatically as soon as the instance launches.

User data scripts are typically run only once when the instance is launched for the first time (although it can be configured to run again during instance reboot with specific configurations).

### **How EC2 User Data Works**

- **User Data** is provided as part of the instance's launch configuration.
- The user data script runs as the **root** user.
- Scripts can be written in **bash**, **PowerShell**, or other supported scripting languages.
- The user data runs only during the instance's initial boot cycle unless configured otherwise.
- Common use cases include installing packages, starting services, setting configurations, or downloading application code.

### **Example: User Data Script to Install Apache Web Server on Amazon Linux 2**

Let’s look at an example where we automatically install **Apache (httpd)** and set up a basic web page when the EC2 instance is launched.

#### **Steps**:
1. **Launch an EC2 instance** with the user data script.
2. The script will install **Apache**, start the web server, and create a simple HTML file.

### **Example User Data Script (Bash)**:

```bash
#!/bin/bash
# Update all packages
yum update -y

# Install Apache Web Server
yum install -y httpd

# Start Apache Web Server
systemctl start httpd

# Enable Apache to start on boot
systemctl enable httpd

# Create a simple HTML page
echo "<html><body><h1>Welcome to my EC2 instance!</h1></body></html>" > /var/www/html/index.html
```

This script does the following:
- **Updates** the EC2 instance’s packages (`yum update -y`).
- Installs **Apache** using `yum` (which is the package manager for Amazon Linux 2).
- **Starts** the Apache web server.
- Configures Apache to **start on boot** using `systemctl enable httpd`.
- Creates a basic HTML page at `/var/www/html/index.html`.

When this instance launches, it will automatically configure and run Apache, making the instance serve the simple HTML file. You can access this file via the EC2 instance’s public IP address.

### **How to Use EC2 User Data in the AWS Console**

1. **Launch an EC2 Instance**:
   - Open the **AWS Management Console** and go to **EC2**.
   - Click **Launch Instance**.

2. **Configure User Data**:
   - In the **Advanced Details** section, you will find a field labeled **User Data**.
   - Paste the user data script (such as the one provided above) into the **User Data** text box.

3. **Launch the Instance**:
   - Complete the configuration for the instance (e.g., instance type, key pair, storage, etc.) and click **Launch**.

4. **Verify**:
   - Once the instance is running, you can access the instance’s public IP in a web browser to see the result (in this case, a simple "Welcome to my EC2 instance!" message).

   For example: `http://<public-ip>`.

### **Accessing the User Data Script on Running Instances**

If you want to see the user data associated with a running instance, you can use the following command within the EC2 instance:

```bash
curl http://169.254.169.254/latest/user-data
```

This retrieves the user data that was passed during the instance’s launch.

### **Common Use Cases for EC2 User Data**:

1. **Installing Software**: Automatically installing necessary packages (e.g., Apache, Nginx, Docker) during the instance's first boot.
2. **Configuration Management**: Setting up configurations like environment variables, application parameters, or system settings.
3. **Service Initialization**: Starting services such as databases, web servers, or application servers.
4. **Bootstrap Scripts**: Running bootstrap scripts to prepare instances for specific applications, load balancers, or scaling groups.

### **Limitations**:

- **Run-once behavior**: By default, EC2 user data scripts run only during the first boot cycle unless you modify configurations to run them on every reboot.
- **Script size**: User data scripts can have a maximum size of 16KB. If the script exceeds this, consider hosting the script in an S3 bucket and downloading it within the user data script.
- **Execution time**: User data scripts may increase instance boot time, especially if they perform complex tasks such as installing software or updating the system.

---

### **Summary**

- **EC2 User Data** allows you to pass initialization scripts that automatically execute when an instance launches, enabling automation and customization of instances.
- Scripts can be used to perform tasks such as installing software (e.g., Apache), configuring the environment, and starting services.
- **Example**: Automatically install and configure Apache and serve a simple webpage when an instance launches.
