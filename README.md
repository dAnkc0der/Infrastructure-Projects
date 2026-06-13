# Ansible AWS Infrastructure Provisioner

This project provisions an AWS EC2 instance with Ansible and deploys a static HTML site to the instance using GitHub Actions.

## What This Project Does

- Creates or reuses an EC2 instance on AWS.
- Generates a dynamic `inventory.ini` file using the EC2 public IP.
- Connects to the instance over SSH as `ec2-user`.
- Installs and starts `httpd`.
- Opens port `80` using `firewalld`.
- Deploys `aws/files/index.html` to `/var/www/html/index.html`.

## Project Structure

```text
.
├── .github/workflows/deploy.yml
├── ansible.cfg
├── configure.yaml
├── configure-web-server.yaml
├── inventory.example.ini
├── requirements.yml
└── aws/
    ├── files/index.html
    └── vars/main.yml
```

## GitHub Actions Pipeline

The workflow runs on:

- Pushes to the `main` branch.
- Manual runs from the GitHub Actions tab.

Pipeline steps:

1. Checkout the repository.
2. Install Python, Ansible, `boto3`, and `botocore`.
3. Install required Ansible collections.
4. Decode the EC2 SSH private key from GitHub Secrets.
5. Run `configure.yaml` to provision the EC2 instance.
6. Run `configure-web-server.yaml` to deploy the static site.

## Required GitHub Secrets

Add these secrets in:

```text
GitHub Repository -> Settings -> Secrets and variables -> Actions
```

| Secret name | Description |
| --- | --- |
| `AWS_ACCESS_KEY_ID` | AWS IAM access key used by Ansible |
| `AWS_SECRET_ACCESS_KEY` | AWS IAM secret key used by Ansible |
| `EC2_SSH_KEY_B64` | Base64 encoded EC2 private key |

Create `EC2_SSH_KEY_B64` from your `.pem` file:

```bash
base64 -i /path/to/your-key.pem | tr -d '\n' | pbcopy
```

Paste the copied value into the GitHub secret named `EC2_SSH_KEY_B64`.

## AWS Configuration

EC2 settings are stored in:

```text
aws/vars/main.yml
```

Example:

```yaml
aws_region: "us-east-1"
key_name: "Key-Pair-2"
instance_type: "t3.micro"
ami_id: "ami-0fdfb4d987b63ae72"
security_group_id: "sg-0984dfd4e1b532d33"
```

Make sure the security group allows:

| Port | Purpose |
| --- | --- |
| `22` | SSH access for Ansible |
| `80` | HTTP access for the static site |

## Local Setup

Install Ansible dependencies:

```bash
ansible-galaxy collection install -r requirements.yml
pip install boto3 botocore
```

Run the provision playbook:

```bash
ansible-playbook configure.yaml
```

Run the web server deployment:

```bash
ansible-playbook -i inventory.ini configure-web-server.yaml --private-key /path/to/your-key.pem
```