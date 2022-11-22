### Cloud Computing Basics



#### Virtualization**

- Virtualization uses software to create an abstraction layer over computer hardware that allows the hardware elements of a single computer—processors, memory, storage and more—to be divided into multiple virtual computers, commonly called virtual machines (VMs).
- Each VM runs its own operating system (OS) and behaves like an independent computer, even though it is running on just a portion of the actual underlying computer hardware.
- Today, virtualization is a standard practice in enterprise IT architecture. It is also the technology that drives cloud computing economics.
- Virtualization enables cloud providers to serve users with their existing physical computer hardware; it enables cloud users to purchase only the computing resources they need when they need it, and to scale those resources cost-effectively as their workloads grow.

In short, **Virtualization is a process that allows for more efficient utilization of physical computer hardware and is the foundation of cloud computing**.



![img](https://cdn-images-1.medium.com/max/800/0*Wp07GQd1iInU08XE.jpg)

**Benefits of virtualization**

Virtualization brings several benefits to data center operators and service providers:

- Resource efficiency
- Easier management
- Faster provisioning
- Minimal downtime

**What is Hypervisors**

- A hypervisor is the software layer that coordinates VMs. 
- It serves as an interface between the VM and the underlying physical hardware, ensuring that each has access to the physical resources it needs to execute. 
- It also ensures that the VMs don’t interfere with each other by impinging on each other’s memory space or compute cycles.

**Types of hypervisors**

- Type 1 or “bare-metal” hypervisors interact with the underlying physical resources, replacing the traditional operating system altogether. They most commonly appear in virtual server scenarios.
- Type 2 hypervisors run as an application on an existing OS. Most commonly used on endpoint devices to run alternative operating systems, they carry a performance overhead because they must use the host OS to access and coordinate the underlying hardware resources.



![img](https://cdn-images-1.medium.com/max/800/1*XYVNfwUqBFNlPBBePwtdSg.png)

**What is a virtual machine (VM)?**

A virtual machine is a virtual representation, or emulation, of a physical computer. They are often referred to as a guest while the physical machine they run on is referred to as the host.



**How virtualization works**

- When a hypervisor is used on a physical computer or server, (also known as bare metal server), it allows the physical computer to separate its operating system and applications from its hardware. Then, it can divide itself into several independent “virtual machines.”

- Each of these new virtual machines can then run their own operating systems and applications independently while still sharing the original resources from the bare metal server, which the hypervisor manages. Those resources include memory, RAM, storage, etc.

<u>---------------------------------------------------------------------------------------------------------------------------</u>



**Throughput & IOPS**

- throughput is a measure of how many units of information a system can process in a given amount of time.
- Storage throughput also called data transfer rate measures the amount of data transferred to and from the storage device per second. 
- The IOPS metric shows how many read and/or write operations a storage device can perform per second.
- A single operation is performed on one Hard Disk Drives (HDD) normally has 512B or 4KB blocks, whereas modern Solid State Drive (SSD) exposes storage memory in pages joined in blocks that can reach 512KB in size.
- Normally, throughput is measured in MB/s. Throughput is closely related to IOPS and block size. Given the same IOPS, the block size can make a substantial difference in terms of throughput. 
- In particular, smaller block sizes result in lower throughput because each I/O reads or writes data in non-divisible blocks.
- Similarly to IOPS, data throughput can also depend on the type of I/O: random or sequential. Data throughput is close to the maximum sustained bandwidth for sequential I/O. 
- The data throughput of a random I/O is significantly smaller than that of a sequential I/O for HDDs but is comparable to modern SSDs.

![img](https://cdn-images-1.medium.com/max/800/1*ZIHnsPnvGc1xO1m5WgHrXQ.png)



**Measuring IOPS**

You can measure IOPS with open-source testing tools, such as Vdbench, which work by simulating I/O workloads on the target storage devices.



**Measuring throughput**

Given the IOPS and block size, you can calculate storage throughput for sequential writes as follows:

```
Throughput = IOPS X block_size 
```

So, for example, if the IOPS is 3,000 and the block size is 512 KB, the throughput of the drive is:

```
3000 * 512KB = 1,535 Mb
```





**Scalability**

- Cloud scalability in cloud computing refers to the ability to increase or decrease IT resources as needed to meet changing demand. 
- Scalability is one of the hallmarks of the cloud and the primary driver of its exploding popularity with businesses.

**Elasticity**

- Elasticity refers to a system’s ability to grow or shrink dynamically in response to changing workload demands, like a sudden spike in web traffic. 
- An elastic system automatically adapts to match resources with demand as closely as possible, in real-time. 
- A business that experiences variable and unpredictable workloads might seek an elastic solution in the public cloud

**Scalability versus Elasticity**

- A scalable solution enables stable, longer-term growth in a pre-planned manner, while an elastic solution addresses more immediate, variable shifts in demand. 
- Elasticity and scalability in cloud computing are both important features for a system, but the priority of one over the other depends in part on whether your business has predictable or highly variable workloads.

**Types of scalability:**

**Vertical scale or scale-up**

- Adding more computing or memory resources for your applications increases the maximum capacity of the server. 
- When demand spikes, there should not be any noticeable change to your applications.

![img](https://cdn-images-1.medium.com/max/800/1*Y84_Ngkng2OWN9SCy2GEQA.png)



**Horizontal scale or scale-out**:

- Adding more individual servers into a resource pool where your applications run

![img](https://cdn-images-1.medium.com/max/800/1*yjAsSeM-lyuus9eMxm69CQ.png)



**Diagonal scaling**:

- Essentially a combination of vertical and horizontal scaling, this setup will scale vertically first until you reach a preset limit and then scale the system horizontally.

![img](https://cdn-images-1.medium.com/max/800/1*9Dt9xfFVOi0F9OXgCoT-FQ.png)

**Advantage of Cloud**

- If you are running your Apps on-premises  in order to achieve scalability you have to procure the hardware, provision it, configure it, and test both the infrastructure and application prior to making it available in production. 
- This requires significant time and resources.
- Business loss in terms of downtime 
- Now after doing all this, if your business demand decreases then organizations have no choice other than to absorb the cost.
- In contrast, scalability in cloud computing is much more seamless for organizations to adapt as demand increases or decreases. 
- This elasticity is managed behind the scenes on-demand as an end customer you can very easily automate this process.

**High Availability**

- High availability is a quality of computing infrastructure that is important for mission-critical systems. 
- High availability permits the computing infrastructure to continue functioning, even when certain components fail.
- High availability is a quality of computing infrastructure that allows it to continue functioning, even when some of its components fail. 
- This is important for mission-critical systems that cannot tolerate interruption in service, and any downtime can cause damage or result in financial loss.

**Elements of high availability**

It is impossible for systems to be available 100% of the time, so true high-availability systems generally strive for five nines as the standard of operational performance.

- **Redundancy** — ensuring that any elements critical to system operations have an additional, redundant component that can take over in case of failure.
- **Monitoring** — collecting data from a running system and detecting when a component fails or stops responding.
- **Failover** — a mechanism that can switch automatically from the currently active component to a redundant component, if monitoring shows a failure of the active component.

**Why high availability matters**

- Failing any systems or their components may have a severe impact on a business or people’s lives if they fall below a certain level of operational performance.
- So, it results in business loss

**How to achieve HA**

Many ways we can build a HA architecture, I’ll be discussing Disaster Recovery.

A well-architected disaster recovery (DR) plan enables you to recover quickly from disasters and continue to provide services to your users.

- DR is the process of preparing for and recovering from a disaster. A disaster can be any event that puts your applications at risk, from network outages to equipment and application failures to natural disasters. 
- It’s almost impossible to predict when you will need disaster recovery, just like you can’t predict when you’ll get in a car accident. If you can’t control when a disaster strikes, the next best thing is to be able to control the recovery process

The first step in planning for DR involves determining the **recovery time objective (RTO) and recovery point objective (RPO)**.

- The RTO is the target time within which a given application must be restored after a disaster occurs. Typically, the more critical the application, the lower the RTO.
- The RPO is the period after a disaster occurs for which an application can tolerate lost data before the disaster begins to affect the business.
- To build a plan that guarantees the recovery of your applications after a disaster and is cost-effective, you must consider both the target time to recover and the tolerance for data loss.

![img](https://cdn-images-1.medium.com/max/800/1*MRMDEDCSwAPeZdxXzhQVHQ.png)