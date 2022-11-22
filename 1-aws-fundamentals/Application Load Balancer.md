# Application Load Balancer



## What is Application Load Balancer?

An Application Load Balancer functions at the application layer, the seventh layer of the Open Systems Interconnection (OSI) model. 

<img src="https://cdn-images-1.medium.com/max/800/0*wCFYPGD3_HBSZnWZ.jpg" alt="img" style="zoom:33%;" />



## Application Load Balancer components



![img](https://cdn-images-1.medium.com/max/800/0*1IuG_VfqysbKhtS6.png)



1: ***load balancer*** serves as the single point of contact for clients. The load balancer distributes incoming application traffic across multiple targets, such as EC2 instances, in multiple Availability Zones.

2: ***listener*** checks for connection requests from clients, using the protocol and port that you configure. 

The **rules** that you define for a listener determine how the load balancer routes requests to its registered targets. 

- Each rule consists of a priority, one or more actions, and one or more conditions. 
- When the conditions for a rule are met, then its actions are performed. You must define a default rule for each listener, and you can optionally define additional rules.

3: ***target group*** routes requests to one or more registered targets, such as EC2 instances, using the protocol and port number that you specify. 

- You can register a target with multiple target groups. 
- You can configure health checks on a per target group basis. 
- Health checks are performed on all targets registered to a target group that is specified in a listener rule for your load balancer.



### **How it works**

- After the load balancer receives a request, it evaluates the listener rules in priority order to determine which rule to apply.
- It  then selects a target from the target group for the rule action. 

What else:

- Routing is performed independently for each target group, even when a target is registered with multiple target groups. 
- I.E. You can configure the routing algorithm used at the target group level. 
- The default routing algorithm is round robin; alternatively, you can specify the least outstanding requests routing algorithm.
- You can add and remove targets from your load balancer as your needs change, without disrupting the overall flow of requests to your application. 
- Elastic Load Balancing scales your load balancer as traffic to your application changes over time. 
- You can configure health checks, which are used to monitor the health of the registered targets so that the load balancer can send requests only to the healthy targets

Why not CLB:

![img](https://cdn-images-1.medium.com/max/800/1*IbSfJJ0W1AAUgQFudM4xpw.png)

![img](https://cdn-images-1.medium.com/max/800/1*zZa0E48rLrt8ftZ5MVGVcg.png)

### 
