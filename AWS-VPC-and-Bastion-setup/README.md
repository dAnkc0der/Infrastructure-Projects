# AWS VPC and Bastion Setup

This project provisions a basic AWS network using Ansible:

- A custom VPC
- One public subnet
- One private subnet
- Internet Gateway for the public subnet
- NAT Gateway for private subnet outbound access
- Bastion EC2 instance in the public subnet
- App EC2 instance in the private subnet
- Security groups allowing SSH to the bastion from your IP and SSH to the app server from the bastion


## Main Files

- `group_vars/all/pass.yml`: Vault-encrypted AWS credentials.
- `group_vars/all/network_facts.yml`: Local network values and generated IDs.

## Variables

Edit:

```text
roles/vpc_bastion/vars/main.yml
```

## Secrets

AWS credentials should stay in Ansible Vault, for example:

```text
group_vars/all/pass.yml
```

### Create vault pass

```
openssl rand -base64 2048 > vault.pass
```

### Add AWS credentials using below command

```
ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass
```

Expected variables:

```yaml
aws_access_key: "..."
aws_secret_key: "..."
```

## Run


```bash
ansible-playbook AWS-VPC-and-Bastion-setup/vpc.yaml --vault-password-file AWS-VPC-and-Bastion-setup/vault.pass
```