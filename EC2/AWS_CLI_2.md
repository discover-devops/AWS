To list all EC2 instances using the AWS CLI, you can use the `describe-instances` command. This command retrieves details about all EC2 instances in your AWS account within the default region or the region you specify.

### **Basic Command to List EC2 Instances**

```bash
aws ec2 describe-instances
```

This command will return a detailed JSON output of all EC2 instances, including information such as instance IDs, state, availability zones, public IP addresses, and more.

---

### **Filter and Format the Output**

To make the output more readable and get specific details, you can use the `--query` and `--output` options.

#### **Example 1: List All EC2 Instances with Instance IDs and State**

```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].[InstanceId,State.Name]" --output table
```

This will output a table with the instance IDs and their current state (e.g., running, stopped).

#### **Example 2: List All EC2 Instances with Public IPs and Instance IDs**

```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].[InstanceId,PublicIpAddress]" --output table
```

This will output a table with instance IDs and their associated public IP addresses.

#### **Example 3: List All EC2 Instances with Tags, Instance IDs, and State**

```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].[InstanceId,State.Name,Tags[?Key=='Name'].Value | [0]]" --output table
```

This command will list the instance ID, the state (running, stopped), and the `Name` tag for each EC2 instance.

#### **Example 4: List EC2 Instances by a Specific Tag**

To list instances that have a specific tag (e.g., instances tagged as `Environment=Production`):

```bash
aws ec2 describe-instances --filters "Name=tag:Environment,Values=Production" --query "Reservations[*].Instances[*].[InstanceId,State.Name,Tags[?Key=='Name'].Value | [0]]" --output table
```

This will list all EC2 instances with the `Environment=Production` tag.

---

### **Use Case: Listing Running EC2 Instances**

To list only the EC2 instances that are currently **running**, you can filter by instance state:

```bash
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --query "Reservations[*].Instances[*].[InstanceId,State.Name,PublicIpAddress]" --output table
```

This will output a table of running instances with their instance IDs, state, and public IP addresses.

---

### **Additional Options**

- **Specify Region**: If you want to list EC2 instances in a specific AWS region, add the `--region` flag:

  ```bash
  aws ec2 describe-instances --region ap-south-2
  ```

- **Output Formats**: You can change the output format using the `--output` flag (JSON, table, text). For example:

  ```bash
  aws ec2 describe-instances --output json
  ```

This allows you to view EC2 instances in a structured format that's easier to parse programmatically.

---

### **Summary**

- Use `aws ec2 describe-instances` to list all EC2 instances.
- You can filter and format the output using the `--query` and `--output` options to focus on specific fields such as instance IDs, public IPs, state, and tags.
- Use filters to display instances based on state (e.g., running) or tags.

Let me know if you need further assistance!
