# Infrastructure Projects

This repository contains hands-on infrastructure and DevOps projects. Each project lives in its own folder, with shared GitHub Actions workflows stored at the repository root.

## Projects

| Project | Description |
| --- | --- |
| `Ansible-AWS-Infrastructure` | Provisions an AWS EC2 instance with Ansible and deploys a static site using GitHub Actions |

## Repository Structure

```text
.
├── .github/
│   └── workflows/
│       └── ansible-aws-deploy.yml
├── Ansible-AWS-Infrastructure/
│   ├── README.md
│   ├── ansible.cfg
│   ├── configure.yaml
│   ├── configure-web-server.yaml
│   ├── inventory.example.ini
│   ├── requirements.yml
│   └── aws/
└── README.md
```

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
