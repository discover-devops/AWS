# Auto Scaling Groups

- Applications may encounter different amount of load depending on their usage span
- In cloud we can create and get rid of resources (servers) quickly
- The goal of an Auto Scaling Group (ASG) is to:
    - Scale out (add more EC2 instances) to match the increased load
    - Scale in (remote EC2 instances) to match a decreased load
    - Ensure we have a minimum and a maximum number of machines running
    - Automatically register new instances to a load balancer



## ASG Attributes

- Launch configuration - consists of:
    - AMI + Instance Type
    - EC2 User Data
    - EBS Volumes
    - Security Groups
    - SSH Key Pair
- Min size, max size, initial capacity
- Network + subnets information
- Load balancer information
- Scaling policies - what will trigger a scale out/scale in

## Auto Scaling Alarms

- It is possible to scale an ASG based on CloudWatch alarms
- An alarm monitors a metric (such as Average CPU)
- **Metrics are computed for the overall ASG instances**
- Based on alarms we can create:
    - Scale-out policies
    - Scale-in policies

## Auto Scaling New Rules

- It is possible to define "better" auto scaling rules managed directly by EC2 instances, for example:
    - Target Average CPU Usage
    - Number of requests on teh ELB per instance
    - Average Network In/Out
- These rules are easier to set up and to reason about then the previous ones

## Auto Scaling Based on Custom Metrics

- We can scale based on a custom metric (ex: number of connected users to the application)
- In order to do this we have to:
    1. Send a custom metric request to CloudWatch (PutMetric API)
    2. Create a CloudWatch alarm to react to values of the metric
    3. Use the CloudWatch alarm as a scaling policy for ASG

## ASG Summary

- Scaling policies can be on CPU, Network, etc. and can even be based on custom metrics or based on a schedule
- ASGs can use launch configurations or launch templates (newer version)
    - Launch configurations allow to specify one instance type
    - Launch templates allow to use a spot fleet of instances
- To update an ASG, we must provide a new launch configuration/template. The underlying EC2 instances will be replaced over time
- IAM roles attached to an ASG will be assigned to the launched EC2 instances
- ASGs are free. We pay for the underlying resources being launched (EC2 instances, attached EBS volumes, etc.)
- Having instances under an ASG means that if they get terminated for any reason, the ASG will automatically create new ones as a replacement
- ASG can terminate instances marked as unhealthy by a load balancer and obviously replace them
- Health checks - we can have 2 types of health checks:
    - EC2 health checks - instances is recreated if the EC2 instance fails to respond to health checks
    - ELB health checks - instance is recreated if the ELB health checks fail, meaning that the application is down for whatever reason

## ASG Scaling Policies

- **Target Tracking Scaling**
    - Most simple and easy to setup
    - Example: we want the average ASG CPU to stay around 40%
- **Simple/Step Scaling**
    - Example: 
        - When a CloudWatch alarm is triggered (example average CPU > 70%), then add 2 units
        - When a CloudWatch alarm is triggered (example average CPU < 30%), then remove 1 unit
- **Scheduled Actions**
    - Can be used if we can anticipate scaling based on known usage patterns
    - Example: increase the min capacity to 10 at 5 PM on Fridays

### Scaling Cool-downs

- The cool-down period helps to ensure that our ASG doesn't launch or terminate additional instances before the previous scaling activity takes effect
- In addition to default cool-down for ASG we can create cool-downs that apply to specific *simple scaling policy*
- A scaling-specific cool-down overrides the default cool-down period
- Common use case for scaling-specific cool-downs is when a scale-in policy terminates instances based in a criteria or metric. Because this policy terminates instances, an ASG needs less time to determine wether to terminate additional instances
- If the default cool-down period of 300 seconds is too long, we can reduce costs by applying a scaling-specific cool-down of 180 seconds for example
- If our application is scaling up and down multiple times each hour, we can modify the ASG cool-down timers and the CloudWatch alarm period that triggers the scale

## Suspend and Resume Scaling Processes

- We can suspend and then resume one or more of the scaling processes for our ASG. This can be useful when we want to investigate a configuration problem or other issue with our web application and then make changes to our application, without invoking the scaling processes.
- We can manually move an instance from an ASG and put it in the standby state
- Instances in standby state are still managed by Auto Scaling, are charged as normal, and do not count towards available EC2 instance for workload/application use. Auto scaling does not perform health checks on instances in the standby state. Standby state can be used for performing updates/changes/troubleshooting etc. without health checks being performed or replacement instances being launched.

## ASG for Solutions Architects

- **ASG Default Termination Policy**
    1. Find the AZ which has to most number of instances
    2. If there are multiple instances to choose from, delete the one with the oldest launch configuration
- Lifecycle Hooks
    - By default as soon as an instance is launched in an ASG, the instance goes in service
    - ASGs provide the ability to perform extra steps before the instance goes in service
    - Also, we have he ability to perform some actions before the instance is terminated
    - Lifecycle hooks diagram: https://docs.aws.amazon.com/autoscaling/ec2/userguide/lifecycle-hooks.html
- Launch Templates vs. Launch Configurations
    - Both allow to specify the AMI, the instance type, a key par, security groups and the other parameters that we use to launch EC2 instances (tags, user-data, etc.)
    - Launch Configurations are considered to be legacy:
        - They must be recreated every time
    - Launch Templates:
        - They can have multiple versions
        - They allow parameter subsets used for partial configuration for re-use and inheritance
        - We can provision both On-Demand and Spot instances (or a mix of two)
        - We can use the T2 unlimited burst feature
        - Recommended by AWS

![       An Auto Scaling group with an Application Load Balancer.     ](https://docs.aws.amazon.com/autoscaling/ec2/userguide/images/elb-tutorial-architecture-diagram.png)

## Step 1: Set up a launch template or launch configuration

**To select an existing launch template**

1. Open the [Launch templates page](https://console.aws.amazon.com/ec2/v2/#LaunchTemplates) of the Amazon EC2 console.
2. On the navigation bar at the top of the screen, choose the Region where the load balancer was created.
3. Select a launch template.
4. Choose **Actions**, **Create Auto Scaling group**.

Alternatively, to create a new launch template, use the following procedure.

## Step 2: Create an Auto Scaling group

**To create an Auto Scaling group**

1. On the **Choose launch template or configuration** page, for **Auto Scaling group name**, enter a name for your Auto Scaling group.

2. [Launch template only] For **Launch template**, choose whether the Auto Scaling group uses the default, the latest, or a specific version of the launch template when scaling out.

3. Choose **Next**.

   The **Choose instance launch options** page appears, allowing you to choose the VPC network settings you want the Auto Scaling group to use and giving you options for launching On-Demand and Spot Instances (if you chose a launch template).

4. In the **Network** section, for **VPC**, choose the VPC that you used for your load balancer. If you chose the default VPC, it is automatically configured to provide internet connectivity to your instances. This VPC includes a public subnet in each Availability Zone in the Region.

5. For **Availability Zones and subnets**, choose one or more subnets from each Availability Zone that you want to include, based on which Availability Zones the load balancer is in. For more information, see [Considerations when choosing VPC subnets](https://docs.aws.amazon.com/autoscaling/ec2/userguide/asg-in-vpc.html#as-vpc-considerations).

6. [Launch template only] In the **Instance type requirements** section, use the default setting to simplify this step. (Do not override the launch template.) For this tutorial, you will launch only On-Demand Instances using the instance type specified in your launch template.

7. Choose **Next** to go to the **Configure advanced options** page.

8. To attach the group to an existing load balancer, in the **Load balancing** section, choose **Attach to an existing load balancer**. You can choose **Choose from your load balancer target groups** or **Choose from Classic Load Balancers**. You can then choose the name of a target group for the Application Load Balancer or Network Load Balancer you created, or choose the name of a Classic Load Balancer.

9. (Optional) To use Elastic Load Balancing health checks, for **Health checks**, choose **ELB** under **Health check type**.

10. When you have finished configuring the Auto Scaling group, choose **Skip to review**.

11. On the **Review** page, review the details of your Auto Scaling group. You can choose **Edit** to make changes. When you are finished, choose **Create Auto Scaling group**.





## Step 3: Verify that your load balancer is attached

**To verify that your load balancer is attached**

1. From the [Auto Scaling groups page](https://console.aws.amazon.com/ec2/v2/home?#AutoScalingGroups) of the Amazon EC2 console, select the check box next to your Auto Scaling group.
2. On the **Details** tab, **Load balancing** shows any attached load balancer target groups or Classic Load Balancers.
3. On the **Activity** tab, in **Activity history**, you can verify that your instances launched successfully. The **Status** column shows whether your Auto Scaling group has successfully launched instances. If your instances fail to launch, you can find troubleshooting ideas for common instance launch issues in [Troubleshoot Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/CHAP_Troubleshooting.html).
4. On the **Instance management** tab, under **Instances**, you can verify that your instances are ready to receive traffic. Initially, your instances are in the `Pending` state. After an instance is ready to receive traffic, its state is `InService`. The **Health status** column shows the result of the Amazon EC2 Auto Scaling health checks on your instances. Although an instance may be marked as healthy, the load balancer will only send traffic to instances that pass the load balancer health checks.
5. Verify that your instances are registered with the load balancer. Open the [Target groups page](https://console.aws.amazon.com/ec2/v2/#TargetGroups) of the Amazon EC2 console. Select your target group, and then choose the **Targets** tab. If the state of your instances is `initial`, it's probably because they are still in the process of being registered, or they are still undergoing health checks. When the state of your instances is `healthy`, they are ready for use.

## Step 4: Next steps

Now that you have completed this tutorial, you can learn more:

- You can configure your Auto Scaling group to use Elastic Load Balancing health checks. If you enable load balancer health checks and an instance fails the health checks, the Auto Scaling group considers the instance unhealthy and replaces it. For more information, see [Add Elastic Load Balancing health checks](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-add-elb-healthcheck.html).
- You can expand your application to an additional Availability Zone in the same Region to increase fault tolerance if there is a service disruption. For more information, see [Add Availability Zones](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-add-availability-zone.html).
- You can configure your Auto Scaling group to use a target tracking scaling policy. This automatically increases or decreases the number of instances as the demand on your instances changes. This allows the group to handle changes in the amount of traffic that your application receives. For more information, see [Target tracking scaling policies](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scaling-target-tracking.html).
