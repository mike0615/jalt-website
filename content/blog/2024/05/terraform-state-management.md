---
title: "Terraform State Management: Remote State Done Right"
date: 2024-05-01
draft: true
tags: [terraform, infrastructure-as-code, devops]
categories: [technical]
summary: "How to set up remote state safely, avoid lock conflicts, and keep your infrastructure code reliable."
---

# Terraform State Management: Remote State Done Right

## The Problem

Local state files are convenient until they're not. One person runs `terraform apply` while you're running it, and suddenly your infrastructure is out of sync. Or someone deletes the state file and panics.

Remote state solves this. But there are pitfalls.

## The Minimum Setup

**Backend**: S3 (or equivalent) with versioning enabled
**Locking**: DynamoDB (or equivalent)
**Access**: IAM roles, not long-lived credentials

```hcl
terraform {
  backend "s3" {
    bucket         = "my-org-tf-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

## What Can Go Wrong

1. **State conflicts**: Two people running apply at the same time
2. **Lock timeouts**: A stuck process leaves the lock held
3. **Accidental local state**: Forgetting to configure the backend
4. **State files in git**: Never, ever do this

## The Checklist

- [ ] S3 versioning enabled
- [ ] S3 encryption at rest
- [ ] Locking table created and configured
- [ ] IAM roles with least-privilege access
- [ ] State file excluded from version control
- [ ] Backups tested quarterly

---

More patterns coming in the next post.
