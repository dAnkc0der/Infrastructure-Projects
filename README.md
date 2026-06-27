# Infrastructure Projects

This repository contains infrastructure and DevOps projects. 

## Projects

| Project | Description |
| --- | --- |
| `Ansible-AWS-Infrastructure` | Provisions an AWS EC2 instance with Ansible and deploys a static site using GitHub Actions |
| `AWS-VPC-and-Bastion-setup` | Provisions a VPC, public/private subnets, bastion host, private app server, and supporting network resources |


## GitHub Actions

Workflows are stored in:

```text
.github/workflows/
```

For project-specific workflows, use path filters so only the relevant pipeline runs when that project changes.

Example:

```yaml
on:
  push:
    branches:
      - main
    paths:
      - "Ansible-AWS-Infrastructure/**"
      - ".github/workflows/ansible-aws-deploy.yml"
```

## Secrets

GitHub Actions secrets are configured at the repository level:

```text
Settings -> Secrets and variables -> Actions
```

Project-specific secret names should be documented inside each project README.
