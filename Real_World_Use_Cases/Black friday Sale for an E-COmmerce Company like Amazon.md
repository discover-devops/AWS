**Problem Statement**: You are AWS cloud engineer, for an e-commerce organization. You are working for a Black-Friday sale. This sale is going to generate huge traffic and in turn going to increase the revenue of your organization. 

**What is the requirement**: As a cloud Engineer your job is to ensure the website doesn't go down and is able to handle every request efficiently. 

**AWS Services**: Application Load Balancers and Auto Scaling groups. 

 After everything is set up, you'll simulate stress tests on the EC2 instances to confirm the Auto Scaling group works as expected. Successfully complete this lab by achieving the following learning objectives:



![img](https://cdn-images-1.medium.com/max/1200/1*Wh86GOdoOavPBhT2oFuCkA.png)

Create an Application Load Balancer

1. Navigate to **EC2** > **Load Balancers**.

2. Click **Create Load Balancer**.

3. Click the **Create** button under the **Application Load Balancer** and set the following values:

   - *Name*: **My_Lab_ALB**
   - *Scheme*: **internet-facing**
   - *IP address type*: **ipv4**
   - *Load Balancer Protocol*: **HTTP**
   - *Port*: **80**
   - Select the VPC.
   - Add the **ap-south-1a** and **ap-south-1b** AZs to your ALB.

4. Click **Next: Configure Security Settings**

   > **Note:** Skip this screen, as we are not using HTTPS.

5. Click **Next: Configure Security Groups**.

6. Select to **Create a new security group** for your ALB, and set the following values:

   - *Name*: **My_ALB_SG**
   - *Description*: **My_ALB_SG**
   - The default value allows standard HTTP traffic from 0.0.0.0/0 and ::/0 (IPV6), so leave as it.

7. Click **Next: Configure Routing** and enter the following values:

   - *Name*: **My_ALB_TG**
   - *Target type*: **Instance**
   - *Protocol*: **HTTP**
   - *Port*: **80**

8. Expand **Advanced health check settings**, and reduce the healthy and unhealthy threshold checks down to **2**.

   - This means the load balancer can respond faster and instances come into service and vice versa.

9. Click **Next: Register Targets**.

10. Click **Next: Review**.

Make a note of the DNS name associated with the load balancer and open in a new browser tab. You should see a 503 error since we don't have any operational EC2 instances associated with the load balancer.



Create a Launch Template

Create an SSH key pair that the EC2 instances will use to control access. We will use the key pair to connect to the EC2 instances in order to perform load testing:

1. Navigate to **EC2** > **Network & Security** > **Key Pairs**.
2. Click **Create Key Pair**.
3. Call it **My_ALB_KP**, and download the file to your local machine.

Create a security group for EC2 instances. This security group will allow us to SSH into the instances and it will allow the ALB to point frontend connections to the instances over port 80.

1. Navigate to **EC2** > **Network & Security** > **Security Groups**.
2. Click **Create Security Group**.
3. The name and description are **EC2WEBSG**.
4. Set the VPC to the lab VPC.
5. Add a rule allowing SSH from 0.0.0.0/0 and ::/0 (IPV6).
6. Add a rule allowing HTTP from the Security Group ID of the security group for the ALB created in the previous video.
7. Create the security group.

Create a launch template that will be used by the Auto Scaling group. The launch template defines what the instances are and how they are created.

1. Navigate to **EC2** > **Instances** > **Launch Templates**.

2. Create a new template, and call it **My_Lab_LT** for the name and description.

3. Search for "AMI", and pick the **Amazon Linux 2 AMI (64-bit x86)**.

4. Set the instance type as **t2.micro**.

5. Select the key pair you created earlier.

6. The network type is VPC.

7. Select the **EC2WEBSG** security group you created earlier.

8. Storage should automatically be populated with a volume, so leave that as default and don't add anything to the network section.

9. Expand *Advanced Details*, and paste the user data (provided on the lab page) in the box.

   > **Note:** These are commands to install a web server and download website content.

10. Click **Create Launch Template**.

11. Click **Close**.



Create an Auto Scaling Group

**Note:** Make sure the load balancer is ready at this point.

1. **EC2** > **Auto Scaling** > **Auto Scaling Groups**

2. Click **Create Auto Scaling group**.

3. Call the group **My_Lab_ASG**.

4. Select **Launch Template**, and choose the template you just created.

5. Select **Adhere to Launch Template**.

6. Pick the VPC from the lab environment, and select `us-east-1a` and `us-east-1b` as subnets.

7. Click **Next**.

8. Check **Enable load balancing**.

9. Select target group **My_ALB_TG**.

10. Leave the default for *Health checks*.

11. Select **Enable group metrics collection with CloudWatch**.

12. For

     

    Group Size

    , enter the following values:

    - *Desired Capacity*: **2**
    - *Minimum Capacity*: **2**
    - *Maximum Capacity*: 4

13. For

     

    Scaling Policies

    , select

     

    Target tracking scaling policy

     

    and enter the following values:

    - *Scaling Policy Name*: **Target Tracking Policy**
    - *Metric type*: **Average CPU utilization**
    - *Target value*: **30**
    - *Instances need*: **300**

14. Click **Next**.

15. Click **Create Auto Scaling Group**.

### Attempt to Connect to Website

Press **Shift**+**Refresh** to access the simple website.



Test Horizontal Scaling

1. Connect to one of the EC2 instances via SSH.
2. Copy the connection string.
3. Run `chmod` on the `.pem` key pair file before running the connection string.
4. Install the stress test application (using the commands provided on the lab page).
5. Run the stress test on the EC2 instance (using the command provided on the lab page).
6. After a few minutes, watch the number of instances increase. It enacts the scale-out policy.
7. After a few minutes, stop the stress test. It enacts the scale-in policy.