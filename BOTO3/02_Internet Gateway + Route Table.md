
#  STEP 2 — Internet Gateway + Route Table

##  Goal

> Make your **public subnets actually public**

---

#  Context

> “Creating a subnet doesn’t make it public…
> routing decides whether it can access the internet”

---

#  What We Will Do

1. Create Internet Gateway (IGW)
2. Attach IGW to VPC
3. Create Route Table
4. Add route → Internet
5. Associate with public subnets

---

#  Step 2.3.1 — Create `network.py`

```bash
nano network.py
```

---

##  Add Code

```python
import boto3

ec2 = boto3.client('ec2')


def create_internet_gateway(vpc_id):
    igw = ec2.create_internet_gateway()['InternetGateway']['InternetGatewayId']

    ec2.attach_internet_gateway(
        InternetGatewayId=igw,
        VpcId=vpc_id
    )

    print(f"IGW Created & Attached: {igw}")
    return igw


def create_route_table(vpc_id, igw_id, public_subnets):
    rt = ec2.create_route_table(VpcId=vpc_id)['RouteTable']['RouteTableId']

    # Route to Internet
    ec2.create_route(
        RouteTableId=rt,
        DestinationCidrBlock='0.0.0.0/0',
        GatewayId=igw_id
    )

    # Associate with public subnets
    for subnet in public_subnets:
        ec2.associate_route_table(
            SubnetId=subnet,
            RouteTableId=rt
        )

    print("Route table created & associated with public subnets")
```

---

#  Step 2.3.2 — Update `main.py`

```python
from vpc import create_vpc
from subnet import create_subnets
from network import create_internet_gateway, create_route_table

vpc_id = create_vpc()
print(f"VPC: {vpc_id}")

subnets = create_subnets(vpc_id)
print(subnets)

igw_id = create_internet_gateway(vpc_id)

create_route_table(vpc_id, igw_id, subnets["public"])
```

---

#  Run

```bash
python main.py
```

---

#  Expected Output

* IGW created
* Route table created
* Public subnets associated

---

#  Verification

## CLI:

```bash
aws ec2 describe-internet-gateways
aws ec2 describe-route-tables
```

---

## Console:

Check:

###  Internet Gateway

* Attached to your VPC

###  Route Table

* Has route:

  ```
  0.0.0.0/0 → IGW
  ```

###  Subnet Association

* Public subnets linked to this route table

---


##  What is IGW?

> “Internet Gateway is the door to the internet”

---

##  What is Route Table?

> “Route table decides where traffic goes”

---


```python
DestinationCidrBlock='0.0.0.0/0'
```

> “This means — all internet traffic”

---

#  Critical Understanding

> “Public subnet =
> IGW + Route Table + Public IP”

---

#  Important Note

Your subnet is now public-ready
BUT EC2 must still have public IP (later step)

---

> “Subnet placement is design…
> routing is behavior”

---

# Next Step

# STEP 3 — EC2 Instances (Private Backend Servers)


