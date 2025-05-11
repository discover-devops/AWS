 **AWS Shield + WAF + CloudFront Protection Checklist**

---

# 🛡️ **AWS Shield + WAF + CloudFront Protection Checklist**

This checklist helps secure internet-facing applications using **Shield Advanced**, **AWS WAF**, and **CloudFront**.

---

##  1. Prepare Your Application

* [ ] Your application is deployed behind:

  * [ ] Application Load Balancer (ALB) **or**
  * [ ] Amazon CloudFront
* [ ] Route 53 is configured for domain DNS
* [ ] HTTPS (TLS) enabled

---

##  2. Enable AWS Shield Advanced

* [ ] Go to AWS Console → **WAF & Shield**
* [ ] Navigate to **Shield → Protected Resources**
* [ ] Click **Enable AWS Shield Advanced**
* [ ] Select and protect:

  * [ ] ALB
  * [ ] CloudFront Distribution
  * [ ] Route 53 Hosted Zone
  * [ ] Elastic IPs (for EC2/GWLB if needed)
* [ ] Add to **Protection Group** (e.g., `prod-web-tier`)

---

##  3. Configure Proactive Engagement

* [ ] Add 24/7 **emergency contact info**
* [ ] Enable access for the **AWS DDoS Response Team (DRT)**
* [ ] Confirm permissions for automated engagement

---

##  4. Attach AWS WAF

* [ ] Create a **Web ACL**
* [ ] Associate it with:

  * [ ] ALB
  * [ ] or CloudFront
* [ ] Add **Managed Rule Groups**:

  * [ ] `AWSManagedRulesCommonRuleSet`
  * [ ] `AWSManagedRulesSQLiRuleSet`
  * [ ] `AWSManagedRulesKnownBadInputsRuleSet`
* [ ] (Optional) Add **custom rules**:

  * [ ] Block specific IPs/countries
  * [ ] Add **Rate limiting** rules
  * [ ] Add **CAPTCHA** challenge for bots

---

##  5. Logging & Monitoring

* [ ] Enable **WAF logs** to CloudWatch Logs or S3
* [ ] Enable **Shield Advanced event logging**
* [ ] Set CloudWatch **Alarms** for:

  * [ ] `DDoSDetected`
  * [ ] Spike in `BlockedRequests`
  * [ ] Unusual traffic patterns

---

##  6. Test & Validate Setup

* [ ] Simulate blocked requests (SQLi/XSS/test IPs)
* [ ] Monitor WAF logs for rule matches
* [ ] Verify CloudWatch alerts are triggered
* [ ] Review Shield DDoS metrics & attack summary

---

##  7. Incident Readiness

* [ ] Bookmark AWS DRT contact portal
* [ ] Review Shield Advanced **attack summaries**
* [ ] Conduct tabletop DDoS response drill
* [ ] Subscribe to **AWS Security Bulletins**

---

##  Pro Tips

* Use **Security Hub** to centralize WAF/Shield findings
* Shield Advanced **absorbs scaling costs** during attack
* For global apps, always use **CloudFront + WAF + Shield** stack
* Block traffic **before it hits origin** with CloudFront edge

---


