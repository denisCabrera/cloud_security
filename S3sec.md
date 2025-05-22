## 🔐 IAM & S3 Security Highlights

### ✅ **Principle: Least Privilege**

* Only grant the minimum permissions needed.
* Start with no access and incrementally allow actions.
* Deny all by default; use **explicit allows** via IAM or bucket policies.

---

## 🛠️ Main AWS Tools for IAM and S3 Security

| Tool                             | Purpose                                                              |
| -------------------------------- | -------------------------------------------------------------------- |
| **IAM Policy Simulator**         | Test what actions a user or role can perform with current policies   |
| **IAM Access Analyzer**          | Detect unintended access to S3, IAM roles, KMS keys, etc.            |
| **AWS CloudTrail**               | Log all API calls, including who accessed what and when              |
| **S3 Server Access Logs**        | Log requests made to specific S3 buckets                             |
| **Amazon Macie**                 | Discover, classify, and alert on sensitive S3 data (e.g. PII)        |
| **Amazon GuardDuty**             | Detect anomalous or malicious behavior like unusual S3 access        |
| **AWS Trusted Advisor**          | Flag security risks (like public S3 buckets or lack of logging)      |
| **Policy Sentry (open-source)**  | Generate least-privilege IAM policies                                |
| **Cloudsplaining (open-source)** | Analyze IAM policies for over-permissive access and escalation risks |

---

## 🧪 Core Commands for Testing Policies

### 🔎 Simulate IAM Access for a Role

```bash
aws iam simulate-principal-policy \
  --policy-source-arn arn:aws:iam::123456789012:role/DeveloperRole \
  --action-names ec2:RunInstances s3:ListBucket \
  --output table
```

### 📜 Test Custom IAM Policy Document

```bash
aws iam simulate-custom-policy \
  --policy-input-list file://policy.json \
  --action-names s3:GetObject
```

### 📋 Create IAM Analyzer

```bash
aws accessanalyzer create-analyzer \
  --analyzer-name S3Analyzer \
  --type ACCOUNT
```

---

## 🧱 Core S3 Security Best Practices

### 🔒 Block Public Access

```bash
aws s3api put-bucket-public-access-block \
  --bucket your-bucket-name \
  --public-access-block-configuration BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
```

### 🧾 Enable Versioning

```bash
aws s3api put-bucket-versioning \
  --bucket your-bucket-name \
  --versioning-configuration Status=Enabled
```

### 🔐 Use KMS Encryption

```bash
aws s3api put-bucket-encryption \
  --bucket your-bucket-name \
  --server-side-encryption-configuration file://encryption.json
```

**Example `encryption.json`:**

```json
{
  "Rules": [{
    "ApplyServerSideEncryptionByDefault": {
      "SSEAlgorithm": "aws:kms"
    }
  }]
}
```

---

## 📈 S3 Monitoring & Audit Summary

| Feature              | What it Monitors                                                             |
| -------------------- | ---------------------------------------------------------------------------- |
| **S3 Inventory**     | Lists all objects, encryption status, ACLs                                   |
| **S3 Storage Lens**  | Reports misconfigured buckets, object counts, versioning metrics             |
| **CloudTrail**       | API-level logging (who did what and when)                                    |
| **GuardDuty for S3** | Detects data exfiltration attempts and unusual access patterns               |
| **Macie**            | Scans buckets for PII, compliance risks, and publicly exposed sensitive data |

---

## ✅ Key Takeaways

1. **IAM Testing Tools:**

   * Use **Policy Simulator** and `simulate-principal-policy` CLI to validate access.
   * Combine this with **CloudTrail logs** to confirm real behavior.

2. **S3 Security Essentials:**

   * **Block public access**, use **encryption** (SSE-KMS), enable **versioning**, and audit with **Macie** and **Access Analyzer**.
   * Enforce **MFA Delete**, **Object Lock**, and **S3 Replication** for compliance-grade setups.

3. **Least Privilege Implementation:**

   * Write scoped IAM policies (use `Action`, `Resource`, `Condition`)
   * Attach policies to roles, not users.
   * Test all policies before deployment using simulators or test accounts.

---
