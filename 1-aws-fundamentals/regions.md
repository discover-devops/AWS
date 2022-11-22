# AWS Regions

- AWS Regions are clusters of data centers
- Example of regions: us-east-1, eu-west-1, etc.
- Most services provided by AWS are regions scoped. This means a data for a service used in one region is not replicated in another region

While writing this Document, AWS  Cloud spans 87 Availability Zones within 27 geographic regions around the world, with announced plans for 21 more Availability Zones and 7 more AWS Regions in Australia, Canada, India, Israel, New Zealand, Spain, and Switzerland.

![img](https://cdn-images-1.medium.com/max/800/1*q5_zygTFHmB2RFGmaZu6NA.png)

![img](https://cdn-images-1.medium.com/max/800/1*_HhQ_0ncEZb3lLJAQ7H6CA.png)

## AWS Availability Zones (AZ)

- AWS Regions are divided into availability zones
- Each region has between 2 and 6 AZs. Usually they have 3
- Each AZ is one or more discrete data center with redundant power, networking an connectivity
- All AZs from a region are geographically separated from each other
- Even if they are separated, they have high bandwidth connection between them
- When you launch an instance, you select a Region and a virtual private cloud (VPC), and then you can either select a subnet from one of the Availability Zones or let us choose one for you. 
- If you distribute your instances across multiple Availability Zones and one instance fails, you can design your application so that an instance in another Availability Zone can handle requests. 



![img](https://cdn-images-1.medium.com/max/800/0*DHnbIcLM0uyK71QH.png)

To find your Availability Zones using the console

- Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.

- From the navigation bar, choose the Regions selector and then choose the Region.

- On the navigation pane, choose EC2 Dashboard.

- The Availability Zones are listed in the Service health pane.

