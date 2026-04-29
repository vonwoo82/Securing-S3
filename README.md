# Securing-S3
## Overview
This project documents the steps taken to secure an Amazon S3 bucket using AWS best practices, including encryption, access control, and public access restrictions.

---

## 1. Server-Side Encryption (SSE)

Enabled server-side encryption on the S3 bucket to ensure all objects stored at rest are automatically encrypted.

- **Encryption Type:** AES-256 (SSE-S3) or AWS KMS (SSE-KMS)
- Encryption is applied automatically to all new objects uploaded to the bucket
- Protects data confidentiality in the event of unauthorized physical access

---

## 2. IAM User Policy (Read-Only)

Created a custom IAM policy to grant read-only access to the S3 bucket.

- Users with this policy can **only** perform read operations (e.g., `s3:GetObject`, `s3:ListBucket`)
- Write, delete, and administrative actions are explicitly denied
- Follows the **principle of least privilege**

**Example Read-Only Policy:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::your-bucket-name",
        "arn:aws:s3:::your-bucket-name/*"
      ]
    }
  ]
}
```

---

## 3. IAM Policy — Attach Policy to User

Attached the read-only IAM policy directly to the target IAM user.

- Navigated to **IAM → Users → Target User → Permissions**
- Selected **Attach policies directly**
- Searched for and attached the custom read-only S3 policy
- Verified the policy appeared under the user's permission set

---

## 4. Block Public Access

Enabled all Block Public Access settings on the S3 bucket to prevent any public exposure of bucket contents.

| Setting | Status |
|---|---|
| Block public ACLs | ✅ Enabled |
| Ignore public ACLs | ✅ Enabled |
| Block public bucket policies | ✅ Enabled |
| Restrict public buckets | ✅ Enabled |

- Configured at the **bucket level** via S3 Console → Permissions → Block Public Access
- Ensures the bucket cannot be made public even if a misconfigured ACL or policy is applied

---

## Summary

| Security Measure | Implementation |
|---|---|
| Server-Side Encryption | AES-256 / SSE-KMS applied to all objects |
| IAM Read-Only Policy | Least privilege access for authorized users |
| Policy Attachment | Policy attached directly to IAM user |
| Block Public Access | All four settings enabled |

> **Result:** The S3 bucket is encrypted at rest, accessible only to authorized IAM users with read-only permissions, and fully protected from public exposure.
