

#  Recommended Project Structure (Professional)

```bash
aws-automation/
│
├── main.py
├── vpc.py
├── subnet.py
├── ec2.py
├── alb.py
└── s3.py
```

---

#  Step-by-Step Refactor 

---

##  1. Keep `vpc.py` CLEAN

 Only VPC logic:

```python
# vpc.py
import boto3

ec2 = boto3.client('ec2')

def create_vpc():
    response = ec2.create_vpc(
        CidrBlock='10.0.0.0/16',
        TagSpecifications=[
            {
                'ResourceType': 'vpc',
                'Tags': [
                    {'Key': 'Name', 'Value': 'rahul-demo-vpc'}
                ]
            }
        ]
    )

    vpc_id = response['Vpc']['VpcId']

    ec2.modify_vpc_attribute(VpcId=vpc_id, EnableDnsSupport={'Value': True})
    ec2.modify_vpc_attribute(VpcId=vpc_id, EnableDnsHostnames={'Value': True})

    return vpc_id
```

---

##  2. Create `subnet.py`

```bash
nano subnet.py
```

---

## 📄 Add this:

```python
import boto3

ec2 = boto3.client('ec2')

def create_subnets(vpc_id):
    subnets = {}

    pub1 = ec2.create_subnet(
        VpcId=vpc_id,
        CidrBlock='10.0.1.0/24',
        AvailabilityZone='ap-south-1a'
    )['Subnet']['SubnetId']

    pub2 = ec2.create_subnet(
        VpcId=vpc_id,
        CidrBlock='10.0.2.0/24',
        AvailabilityZone='ap-south-1b'
    )['Subnet']['SubnetId']

    priv1 = ec2.create_subnet(
        VpcId=vpc_id,
        CidrBlock='10.0.3.0/24',
        AvailabilityZone='ap-south-1a'
    )['Subnet']['SubnetId']

    priv2 = ec2.create_subnet(
        VpcId=vpc_id,
        CidrBlock='10.0.4.0/24',
        AvailabilityZone='ap-south-1b'
    )['Subnet']['SubnetId']

    return {
        "public": [pub1, pub2],
        "private": [priv1, priv2]
    }
```

---

##  3. Create `main.py` (THIS is your entry point)

```python
from vpc import create_vpc
from subnet import create_subnets

vpc_id = create_vpc()
print(f"VPC: {vpc_id}")

subnets = create_subnets(vpc_id)
print(subnets)
```

---

##  Run

```bash
python main.py
```

---







