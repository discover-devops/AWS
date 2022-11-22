**<u>How to design a 3- Tier Architecture in AWS</u>**

What is 3-tier Architecture?

 



The three-tier architecture is the most popular implementation of a multi-tier architecture and consists of a **single presentation tier**, **logic tier**, and **data tier**. 

Each of these layers or tiers does a specific task and can be managed independently of each other. This a shift from the monolithic way of building an application where the frontend, the backend and the database are both sitting in one place.

Here in this section I'll be making use of the following AWS services to design and build a three-tier cloud infrastructure:
 [Elastic Compute Cloud (EC2)](https://aws.amazon.com/ec2/?hp=tile&so-exp=below),
 [Auto Scaling Group](https://aws.amazon.com/ec2/autoscaling/?hp=tile&so-exp=below),
 [Virtual Private Cloud](https://aws.amazon.com/vpc/?hp=tile&so-exp=below)(VPC),
 [Elastic Load Balancer (ELB)](https://aws.amazon.com/elasticloadbalancing/?hp=tile&so-exp=below), [Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) and
 [Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html). 

Using above services I'll design a highly available and fault tolerant architecture.



![img](https://cdn-images-1.medium.com/max/800/0*Me4AkhZxBJ-qu5UG.jpeg)



### Benefits:

**scalability** — each item can scale horizontally, application servers can be deployed on multiple machines
**database integrity and security**— the client cannot directly access the data with an application layer between them and the database
**improved performance** — the presentation tier can cache requests, minimizing network utilization and load
**easier to maintain and modify** — modifications or replacement of one tier does not affect the other tiers (decoupling), multiple developers can work on the different layers, reducing cost and time needed to integrate changes



### **Overview of Steps**

Create a VPC, subnets, internet gateway, and edit route tables.

Create an application load balancer for the web tier (Internet-facing) and application tiers.

Create the application and web tiers with EC2 auto-scaling groups. 

Configure security groups so the web tier only accepts traffic from the ALB (Application Load Balancer), and the application tier only accepts traffic from the web tier security group.

Create the database tier using RDS (free tier).

 Verify the web tier can be accessed from the Internet and that it can ping the application tier.



 Before we get started

To follow along, you need to have an [AWS](http://console.aws.amazon.com/) account. We shall be making use of the AWS free-tier resources so we do not incur charges while learning.

**Note**: At the end of this tutorial, you need to stop and delete all the resources such as the EC2 instances, Auto Scaling Group, Elastic Load Balancer etc you set up. Otherwise, you get charged for it when you keep them running for a long.

## Let’s Begin

1. **Setup the Virtual Private Cloud (VPC):** VPC stands for Virtual Private Cloud (VPC). It is a virtual network where you create and manage your AWS resource in a more secure and scalable manner. Go to the VPC section of the AWS services, and click on the **Create VPC** button.

Give your VPC a name and a [CIDR](https://searchnetworking.techtarget.com/definition/CIDR) block of 10.0.0.0/16



![img](https://cdn-images-1.medium.com/max/800/0*kWkfB0ecFOMmKFdq.png)





Create VPC



![img](https://cdn-images-1.medium.com/max/800/0*NUon2kf5iEbuWasf.png)







**2**. **Setup the Internet Gateway**: The Internet Gateway allows communication between the EC2 instances in the VPC and the internet. To create the Internet Gateway, navigate to the **Internet Gateways** page and then click on **Create internet gateway** button.

![img](https://cdn-images-1.medium.com/max/800/0*4Y9LdWxnumsLrOOp.png)





![img](https://cdn-images-1.medium.com/max/800/0*e7_d7Xvmj4-zuc2A.png)We need to attach our VPC to the internet gateway. To do that:

**a.** we select the internet gateway

**b.** Click on the **Actions** button and then select Attach to VPC.

**c.** Select the VPC to attach the internet gateway and click **Attach**

![img](https://cdn-images-1.medium.com/max/800/0*e8WSoez2tm1kgD5y.png)







**3. Create 4 Subnets:** The subnet is a way for us to group our resources within the VPC with their IP range. A subnet can be public or private. EC2 instances within a public subnet have public IPs and can directly access the internet while those in the private subnet does not have public IPs and can only access the internet through a [NAT](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) gateway.

For our setup, we shall be creating the following subnets with the corresponding IP ranges.

·    demo-public-subnet-1 | CIDR (10.0.1.0/24) | Availability Zone (us-east-1a)

·    demo-public-subnet-2 | CIDR (10.0.2.0/24) | Availability Zone (us-east-1b)

·    demo-private-subnet-3 | CIDR (10.0.3.0/24) | Availability Zone (us-east-1a)

·    demo-private-subnet-4 | CIDR(10.0.4.0/24) | Availability Zone (us-east-1b)



create subnets

![img](https://cdn-images-1.medium.com/max/800/0*btsaRxVqMYNbRTii.png)



![img](https://cdn-images-1.medium.com/max/800/0*IaGxNDS85yiSlT0c.png)



Four subnets got sreated





**4. Create Two Route Tables:** Route tables is a set of rule that determines how data moves within our network. We need two route tables; private route table and public route table. The public route table will define which subnets that will have direct access to the internet ( ie public subnets) while the private route table will define which subnet goes through the NAT gateway (ie private subnet).

To create route tables, navigate over to the **Route Tables** page and click on **Create route table** button.

![img](https://cdn-images-1.medium.com/max/800/0*CsEXOWVV37nrHKi2.png)







Route Table Created

![img](https://cdn-images-1.medium.com/max/800/0*-G03uAfy0SYhdlCu.png)







Private and Public Route Tables





The public and the private subnet needs to be associated with the public and the private route table respectively.

To do that, we select the route table and then choose the **Subnet Association** tab.

![img](https://cdn-images-1.medium.com/max/800/0*Px4jvBly7MveEN8n.png)







Subnet Associations



![img](https://cdn-images-1.medium.com/max/800/0*FYdfBK93RXttzbny.png)



Select the public subnet for the public route table





We also need to route the traffic to the internet through the **internet gateway** for our public route table.

To do that we select the public route table and then choose the **Routes** tab. The rule should be similar to the one shown below:

![img](https://cdn-images-1.medium.com/max/800/0*EhGWtvDmqWRbwUjn.png)







Edit Route for the public route table





**5. Create the NAT Gateway:** The NAT gateway enables the EC2 instances in the private subnet to access the internet. The NAT Gateway is an AWS managed service for the NAT instance. To create the NAT gateway, navigate to the NAT Gateways page, and then click on the **Create NAT Gateway.**

Please ensure that you know the Subnet ID for the **demo-public-subnet-2.** This will be needed when creating the NAT gateway.





![img](https://cdn-images-1.medium.com/max/800/0*93st8Vy_HkTed7QO.png)



Create NAT Gateway





Now that we have the NAT gateway, we are going to edit the private route table to make use of the NAT gateway to access the internet.

![img](https://cdn-images-1.medium.com/max/800/0*GwvsugzF5go2AjCU.png)





![img](https://cdn-images-1.medium.com/max/800/0*XHHrfdHBZWV6wt05.png)





Edit Private Route Table to use NAT Gateway for private EC2 instances





**6**. **Create Elastic Load Balancer:** From our architecture, our frontend tier can only accept traffic from the elastic load balancer which connects directly with the internet gateway while our backend tier will receive traffic through the internal load balancer. The essence of the load balancer is to distribute load across the EC2 instances serving that application. If however, the application is using sessions, then the application needs to be rewritten such that sessions can be stored in either the Elastic Cache or the DynamoDB. To create the two load balancers needed in our architecture, we navigate to the **Load Balancer** page and click on **Create Load Balancer.**

**a.** Select the Application Load Balancer.

![img](https://cdn-images-1.medium.com/max/800/0*iqutDfqxKvpTy5Ur.png)







Select Application Load Balancer





**b.** Click on the **Create** button

**c.** Configure the Load Balancer with a name. Select **internet facing** for the load balancer that we will use to communicate with the frontend and **internal** for the one we will use for our backend.

![img](https://cdn-images-1.medium.com/max/800/0*adWF-ZAwNUvvBVFA.png)

Internet Facing Load Balancer for the Frontend tier

![img](https://cdn-images-1.medium.com/max/800/0*SU5rIRKfh9eqCRmz.png)









Internal Load Balancer for the Backend Tier

![img](https://cdn-images-1.medium.com/max/800/0*qPjTS2jndEbWknPv.png)



**d.** Under the Availability Zone, for the **internet facing Load Balancer**, we will select the two **public subnets** while for our **internal Load Balancer**, we will select the two **private subnet.**

Availability Zone for the Internet Facing Load Balancer

![img](https://cdn-images-1.medium.com/max/800/0*AcpnnIgt5WYNZY3O.png)

Availability Zone for the internal Load Balancer

![img](https://cdn-images-1.medium.com/max/800/0*JiR2mKMj1RAbhnIL.png)



**e.** Under the Security Group, we **only need** to allow ports that the application needs. For instance, we need to allow **HTTP port 80 and/or HTTPS port 443** on our **internet facing load balancer**. For the **internal load balancer**, we only open the port that the backend runs on (eg: port 3000) and the make such port **only open to the security group of the frontend**. This will allow only the frontend to have access to that port within our architecture.

**f.** Under the **Configure Routing,** we need to configure our Target Group to have the **Target type** of **instance.** We will give the **Target Group** a name that will enable us to identify it. This is will be needed when we will create our **Auto Scaling Group.** For example, we can name the Target Group of our frontend to be **Demo-Frontend-TG**

Skip the Register Targets and then go ahead and review the configuration and then click on the **Create** button.

**7. Auto Scaling Group:** We can simply create like two EC2 instances and directly attach these EC2 instances to our load balancer. The problem with that is that our application will no longer scale to accommodate traffic or shrink when there is no traffic to save cost. With Auto Scaling Group, we can achieve this feat. Auto Scaling Group is can automatically adjust the size of the EC2 instances serving the application based on need. This is what makes it a good approach rather than directly attaching the EC2 instances to the load balancer.

To create an Auto Scaling Group, navigate to the **Auto Scaling Group** page, Click on the **Create Auto Scaling Group** button.

**a.** Auto Scaling Group needs to have a common configuration that instances within it **MUST** have. This common configuration is made possible with the help of the **Launch Configuration**. In our Launch configuration, under the Choose AMI, the best practice is to choose the AMI which contains the application and its dependencies bundled together. You can also create your custom AMI in AWS.



Custom AMI for each tier of our application

![img](https://cdn-images-1.medium.com/max/800/0*93W3qolJTnqPVzgw.png)



**b.** Choose the appropriate instance type. For a demo, I recommend you choose t2.micro (free tier eligible) so that you do not incur charges.

**c.** Under the Configure details, give the Launch Configuration a name, eg **Demo-Frontend-LC.** Also, under the **Advance Details** dropdown, the **User data** is provided for you to type in a command that is needed to install dependencies and start the application.

![img](https://cdn-images-1.medium.com/max/800/0*LRSGLLatsCy_vXo7.png)







**d.** Again under the security group, we want to only allow the ports that are necessary for our application.



**e.** Review the Configuration and Click on **Create Launch Configuration** button. Go ahead and create a new key pair. Ensure you download it before proceeding.

**f.** Now we have our Launch Configuration, we can finish up with the creating our Auto Scaling Group. Use the below image as a template for setting up yours.

Auto Scaling Group 1

![img](https://cdn-images-1.medium.com/max/800/0*zT5t0ApUO5YrsI0i.png)



Auto Scaling Group 2

![img](https://cdn-images-1.medium.com/max/800/0*IgiETt2kZaqX5p4L.png)



**g.** Under the Configure scaling policies, we want to add one instance when the CPU is greater than or equal to 80% and to scale down when the CPU is less than or equal to 50%. Use the image as a template.

Scale-up

![img](https://cdn-images-1.medium.com/max/800/0*gf-uewKVj_z5-dXA.png)



Scale Down

![img](https://cdn-images-1.medium.com/max/800/0*0lc6pyFCiloZYWNV.png)



**h.** We can now go straight to Review and then Click on the **Create Auto Scaling group** button. This process is to be done for both the frontend tier and the backend tier but not the data storage tier.

We have almost setup or architecture. However, we cannot SSH into the EC2 instances in the private subnet. This is because have not created our bastion host. So the last part of this article will show how to create the bastion host.

**8. Bastion Host:** The bastion host is just an EC2 instance that sits in the public subnet. The best practice is to only allow SSH to this instance from your trusted IP. To create a bastion host, navigate to the EC2 instance page and create an EC2 instance in the **demo-public-subnet-1** subnet within our VPC. Also, ensure that it has public IP.



Bastion Host EC2 instance in public subnet

![img](https://cdn-images-1.medium.com/max/800/0*6b6f5Dyq-bDsZU-j.png)



Security Group of the Bastion Host

![img](https://cdn-images-1.medium.com/max/800/0*EKgijkU9gtHxN8LH.png)



We also need to allow SSH from our private instances from the Bastion Host.

# 

Ok, that's it !!





 