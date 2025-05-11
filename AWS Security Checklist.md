
#  AWS 3-Tier Architecture Security Checklist

##  Perimeter Security (Internet-facing)

- [ ] Route 53 with DNSSEC enabled
- [ ] AWS WAF attached to CloudFront or ALB with rules (SQLi, XSS, Bot protection)
- [ ] AWS Shield Standard enabled (Shield Advanced if enterprise-grade protection is needed)
- [ ] HTTPS enabled on CloudFront/ALB with valid SSL certificate

##  Network Security (VPC Level)

- [ ] Use a dedicated VPC with separate public and private subnets across 2 AZs
- [ ] Internet Gateway attached to VPC (for public subnets only)
- [ ] NAT Gateways in public subnets to allow private subnets outbound access
- [ ] Public subnets: only ALB and NAT Gateways allowed
- [ ] Private subnets: Web/App/DB servers only
- [ ] VPC Flow Logs enabled for all subnets

##  Application Layer Security

- [ ] Web EC2 instances only accessible via ALB (Web SG)
- [ ] App EC2 instances only accessible from Web SG (App SG)
- [ ] Use SSM Session Manager instead of SSH (no public key required)
- [ ] All EC2s use hardened AMIs and patch management via SSM

##  IAM and Secrets

- [ ] Use IAM roles with least privilege for EC2, Lambda, etc.
- [ ] No hardcoded secrets or keys in code or environment variables
- [ ] Store secrets in AWS Secrets Manager or SSM Parameter Store
- [ ] Enable CloudTrail to log all IAM/API activity

##  Data Layer Security

- [ ] RDS in private subnets, Multi-AZ enabled, encrypted with KMS
- [ ] ElastiCache in private subnet, encryption in transit & at rest enabled
- [ ] Amazon EFS encrypted at rest, mounted via private subnet
- [ ] DB Security Groups allow inbound traffic only from App SG

##  Monitoring and Detection

- [ ] CloudWatch Alarms and Logs enabled for EC2, ALB, RDS, Lambda
- [ ] AWS Config to detect drift/misconfiguration
- [ ] Amazon GuardDuty enabled (threat detection)
- [ ] AWS Security Hub enabled (optional, to unify findings)
