## Network Load Balancer

- A Network Load Balancer functions at the fourth layer of the Open Systems Interconnection (OSI) model. 
- It can handle millions of requests per second. 
- After the load balancer receives a connection request, it selects a target from the target group for the default rule.
-  It attempts to open a TCP connection to the selected target on the port specified in the listener configuration.



## Network Load Balancer components

1: ***load balancer*** The load balancer distributes incoming traffic across multiple targets, such as Amazon EC2 instances. This increases the availability of your application. You add one or more listeners to your load balancer.

2 ***listener*** checks for connection requests from clients, using the protocol and port that you configure, and forwards requests to a target group.

3 ***target group*** routes requests to one or more registered targets, such as EC2 instances, using the TCP protocol and the port number that you specify. 

You can register a target with multiple target groups. 

You can configure health checks on a per target group basis. Health checks are performed on all targets reg istered to a target group that is specified in a listener rule for your load balancer.

![img](https://cdn-images-1.medium.com/max/800/0*JoDe-WqHwvuzz9HU.png)

## Use Cases

- When you need to seamlessly support spiky or high-volume inbound TCP requests without pre-warming.
- When you need support of a static or elastic IP address.
- When you want to support for routing requests to multiple applications on a single EC2 instance. NLB is well suited to ECS.
- When you need to support an IP address or an IP target outside of the VPC.