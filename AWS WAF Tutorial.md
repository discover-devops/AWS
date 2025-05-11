**AWS WAF tutorial with lab** 

---

#  **AWS WAF Tutorial — Step-by-Step with Lab**

##  **Objective**

Deploy **AWS WAF** in front of an **Application Load Balancer (ALB)** to protect a web application from common threats like SQL injection, cross-site scripting (XSS), and bot attacks.

---

##  **Prerequisites**

* AWS account with admin or WAF permissions
* A working **ALB** in front of EC2 instances (any sample app)
* Web app that responds to HTTP/HTTPS requests

---

##  **Step-by-Step Lab**

---

###  **Step 1: Create a Web ACL (Web Access Control List)**

1. Open **AWS Console → WAF & Shield**
2. Click **Create web ACL**
3. Enter:

   * **Name**: `my-web-acl`
   * **Region**: Match your ALB’s region
   * **Resource type**: `Regional resources`
4. Associate with:

   * **Resource type**: `Application Load Balancer`
   * **Choose ALB** from dropdown

---

###  **Step 2: Add AWS Managed Rules**

1. Click **Add rules → Add managed rule groups**
2. Select the following:

   * `AWSManagedRulesCommonRuleSet` – protects from common app attacks
   * `AWSManagedRulesSQLiRuleSet` – blocks SQL injection attempts
   * `AWSManagedRulesKnownBadInputsRuleSet` – filters known malicious payloads
3. Click **Add rule groups**

---

###  **Step 3: Set Default Action**

* Set to **Allow all traffic by default**
* WAF will only **block what rules catch**

---

###  **Step 4: Enable Logging (Optional but Recommended)**

1. Click **Enable logging**
2. Choose a CloudWatch Log Group (or create one)
3. Configure Redacted Fields if needed (e.g., password fields)

---

###  **Step 5: Associate Web ACL with ALB**

* Confirm your Web ACL is associated with your ALB
* Click **Create Web ACL**

---

##  **Testing WAF Rules (Simulated Attacks)**

You can now test your WAF by making requests with attack-like patterns.

###  SQL Injection Test

```bash
curl "http://<your-alb-dns-name>/?user=' OR '1'='1"
```

###  XSS Attack Test

```bash
curl "http://<your-alb-dns-name>/?search=<script>alert('XSS')</script>"
```

###  Bad Input Pattern Test

```bash
curl "http://<your-alb-dns-name>/?cmd=../etc/passwd"
```

**Expected Result**: You should get **403 Forbidden** if the rule triggered.

---

##  **Best Practices**

| Area         | Best Practice                                                         |
| ------------ | --------------------------------------------------------------------- |
| Rules        | Start with AWS Managed Rules                                          |
| Custom Rules | Add IP blocklists, geoblocking, rate limits                           |
| Monitoring   | Enable WAF logging + CloudWatch metrics                               |
| Tuning       | Use “Count” mode first for custom rules to test impact                |
| Integration  | Combine with **Shield Advanced** for full DDoS + app layer protection |

---

##  **Clean Up (to avoid charges)**

* Delete the Web ACL
* Remove test EC2s or ALBs if not needed

---

##  **Bonus Tip**

> Use WAF in front of **CloudFront** or **ALB** for both static and dynamic application protection.
> You can even apply **rate limiting**, **CAPTCHA**, and **IP reputation blocking**.

---

Would you like me to now prepare a **custom WAF rule creation tutorial**, like:

* Block country
* Block based on header
* Add CAPTCHA challenge


