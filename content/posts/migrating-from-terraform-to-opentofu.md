---
title: "Migrating from Terraform to OpenTofu"
date: 2025-02-06
---

OpenTofu maintains the core functionality that made Terraform successful while introducing important improvements. The project operates under community governance, ensuring features and fixes align with the community. All of the Terraform providers worked with OpenTofu with the OpenTofu registry.

## Executing the Migration

The migration to OpenTofu is a straightforward process:

1. Install OpenTofu using your preferred package manager or download the binary from the OpenTofu releases page.
2. Update your scripts and CI/CD configurations to use the `tofu` command instead of `terraform`:

```
# Before
terraform init
terraform plan
terraform apply

# After
tofu init
tofu plan
tofu apply
```

For teams using Terrateam, update your `.terrateam/config.yml`:  

```
engine:
  name: tofu
  version: 1.9.0
```

This configuration ensures Terrateam uses OpenTofu for all operations while maintaining your existing GitOps workflow.

## Building for the Future

OpenTofu is more than just an alternative to Terraform. It brings the principles of open-source software and community-driven progress to infrastructure as code. The migration process is very simple to do and it's also possible to migrate back to Terraform. The long-term benefits of an open-source solution make OpenTofu a worthwhile investment for organizations committed to infrastructure as code.

Go to Source
