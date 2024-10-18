### Introduction to AWS Transit Gateway Route Table

In the previous section, we explored how AWS Transit Gateway facilitates connections between VPCs, VPNs, Direct Connect gateways, and third-party software. Now, we will dive deeper into how VPCs connect to a Transit Gateway using VPC attachments and how routing is managed within this architecture.

AWS Transit Gateway acts as a central hub to interconnect multiple VPCs, simplifying the network architecture by eliminating the need for complex VPC peering relationships. When you create a VPC attachment, you are effectively linking the VPC to the Transit Gateway, enabling communication with other attached VPCs, as well as with hybrid networks through VPN or Direct Connect. However, it is essential to ensure that VPC CIDR blocks do not overlap, as overlapping CIDRs are not supported for attachments to the same Transit Gateway.

Once VPC attachments are created, the key element governing traffic flow between these connected networks is the **Transit Gateway Route Table**. This route table automatically propagates the CIDR blocks of all attached VPCs, creating a mesh network where each VPC can communicate with others. For example, if VPC A (10.0.0.0/16), VPC B (10.1.0.0/16), and VPC C (10.2.0.0/16) are attached to the Transit Gateway, their CIDR routes will automatically propagate to the Transit Gateway route table, ensuring seamless communication between these VPCs.

### Routing Logic

For traffic to flow between VPCs, both the Transit Gateway and each VPC’s route tables must be correctly configured. The Transit Gateway route table automatically learns the routes of attached VPCs, but each VPC's route table must be manually updated with a static route pointing to the Transit Gateway. Without these manual entries, VPCs will not know that the Transit Gateway is the path to other networks.

For instance, to enable VPC A to communicate with VPC B, the route table of VPC A must have a route entry directing traffic for the CIDR block of VPC B through the Transit Gateway. Similarly, VPC B's route table must have an entry for VPC A’s CIDR.

### Example of Routing Flow

Let’s consider an example of how traffic flows from an instance in VPC A to an instance in VPC B:
1. **VPC A Route Table**: A route entry is added to VPC A’s route table, specifying that traffic for VPC B’s CIDR (10.1.0.0/16) should be routed through the Transit Gateway.
2. **Transit Gateway Route Table**: The Transit Gateway route table, having learned the routes of all attached VPCs, directs the traffic to the attachment for VPC B.
3. **VPC B Route Table**: VPC B receives the traffic and processes it according to its own route table.

This process ensures that traffic can flow seamlessly between any VPCs attached to the Transit Gateway, provided that both VPC route tables and the Transit Gateway route table are correctly configured.

### Key Takeaways

- **Transit Gateway Route Table**: Automatically propagates VPC CIDRs upon attachment, allowing routing between connected networks.
- **VPC Route Tables**: Must be manually configured with static routes pointing to the Transit Gateway for traffic to flow to other VPCs.
- **Routing Flexibility**: AWS Transit Gateway’s route table offers full-mesh connectivity between VPCs, but it requires proper route management on both the Transit Gateway and the VPC side.

In the next section, we will perform a hands-on lab to configure VPC attachments, set up routing, and establish communication between multiple VPCs using Transit Gateway.
