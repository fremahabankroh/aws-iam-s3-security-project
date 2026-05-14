
Cloud security project: AWS IAM user creation, custom S3 bucket policies, and permission testing. Built for blue team portfolio.
# AWS IAM + S3 Cloud Security Project

> Implementing least-privilege access control on AWS cloud storage
> using IAM users, custom JSON policies, and permission testing.

## Project overview

A hands-on cloud security project demonstrating how to control access
to AWS S3 using IAM. Built to simulate real-world access control
decisions made by cloud security and blue team engineers.

## Skills demonstrated
- AWS Identity and Access Management (IAM)
- Principle of least privilege
- JSON policy authoring and testing
- Cloud storage security (S3)
- Access control verification and documentation

## Architecture

![Architecture diagram](docs/architecture-diagram.png)

## What I built — step by step

### Step 1 — Created an IAM user
Created `s3-project-user` with console access only.
No permissions attached at creation (zero-trust starting point).
### Creation of Access key for s3-project-user
CLI was the ideal pick
Access keys were labeled with a description noting their purpose and scope — a real-world credential management practice that supports safe key rotation and audit trails.

<img width="984" height="1204" alt="IAM user created" src="https://github.com/user-attachments/assets/bdf3b791-00e9-4316-b942-73ba8292de04" /># aws-iam-s3-security-project

### Enabled without MFA 
In a production environment, MFA would be enforced on all IAM users. For this project it was intentionally left off to simplify testing, but enforcing MFA via an IAM policy condition would be the next hardening step

After AWS CLI installation, verified CLI identity using aws sts get-caller-identity — confirms programmatic access is configured correctly as the least-privilege IAM user, not the root account.

<img width="565" height="706" alt="Screenshot 2026-05-10 at 9 01 45 PM" src="https://github.com/user-attachments/assets/d399c11e-552e-4c5c-a48e-54744b831eb8" />


### Step 2 — Created an S3 bucket
Bucket: `my-iam-project-fremzbankshunt` | Region: eu-north-1
Public access blocked. Two test files uploaded.
### Bucket Versioning 
Server-side encryption (SSE-S3) was enabled by default, ensuring all objects are encrypted at rest automatically. This satisfies a core data protection control without requiring additional key management overhead. Version Control is crucial.

<img width="968" height="1184" alt="Screenshot 2026-05-10 at 1 58 41 PM" src="https://github.com/user-attachments/assets/8587a42f-1aa7-4c69-a5eb-717550d6b1f4" />

### Step 3 — Wrote a custom IAM policy
See policy/s3-access-policy.json for the full document.

Key decisions:
- Allowed: ListBucket, GetObject, PutObject, DeleteObject
- Denied: DeleteBucket, PutBucketPolicy, PutBucketAcl

  <img width="1170" height="1184" alt="Screenshot 2026-05-10 at 5 58 53 PM" src="https://github.com/user-attachments/assets/6f06ddf8-dabc-4483-8f7a-b1dfb619f88f" />


### Step 4 — Tested permissions

| Action | Expected | Result |
|--------|----------|--------|
| List bucket contents | Allow | ✅ Pass |
| Download a file | Allow | ✅ Pass |
| Upload a file | Allow | ✅ Pass |
| Delete a file | Allow | ✅ Pass |
| Delete the bucket | Deny | ✅ Pass |
| Change bucket policy | Deny | ✅ Pass |
During testing, an attempt to delete the bucket via CLI as s3-project-user was blocked by the explicit deny policy — even with the --force flag. Deletion was only possible through the admin console, demonstrating that explicit denies are absolute and cannot be overridden programmatically by the restricted user.
<img width="916" height="539" alt="Screenshot 2026-05-10 at 8 56 02 PM" src="https://github.com/user-attachments/assets/cdc3d7c5-a5d4-4afd-ad1f-b0fc5ead1339" />


## Key concepts learned

### Principle of least privilege
Granting only the minimum permissions needed...

### IAM policy structure
Each policy statement has Effect, Action, and Resource...

## What I would do next
- Add MFA enforcement to the IAM policy
- Set up AWS CloudTrail to log all S3 API calls
- Replace IAM user access keys with IAM roles...

## Tools used
AWS IAM · AWS S3 · AWS Console · JSON · VSCODE

---
⚠️ No credentials, access keys, or sensitive data are stored in this repo.
