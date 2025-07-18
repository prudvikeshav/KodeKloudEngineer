# 🔒 AWS Security Group Creation for Nautilus App Servers

## 📌 Overview

The **Nautilus DevOps team** is strategizing the migration of their infrastructure to the AWS cloud in incremental phases. As part of this effort, they need to create a **security group** for **Nautilus App Servers** in the **default VPC** within the **us-east-1 region** using **Terraform**. This security group will be configured to allow inbound HTTP and SSH traffic for the app servers.

---

## 🧩 Problem Statement

The DevOps team needs to:

- Create a security group named **`devops-sg`**.
- Set the description to **`Security group for Nautilus App Servers`**.
- Add inbound rules for **HTTP** (port 80) and **SSH** (port 22), with source CIDR `0.0.0.0/0` to allow access from anywhere.

---

## ✅ Requirements

- **Security Group Name**: `devops-sg`
- **Description**: `Security group for Nautilus App Servers`
- **Inbound Rules**:
  - **HTTP** (port 80), source: `0.0.0.0/0`
  - **SSH** (port 22), source: `0.0.0.0/0`
- **Region**: `us-east-1`
- **Terraform Working Directory**: `/home/bob/terraform`

---

## 💡 Solution

This solution uses **Terraform** to create the security group, and configure inbound rules for HTTP and SSH access. The security group is created in the **default VPC**.

### Terraform Configuration

```hcl
# Fetch the default VPC information
data "aws_vpc" "default" {
  default = true
}

# Create the security group for Nautilus App Servers
resource "aws_security_group" "devops" {
  name        = "devops-sg"
  description = "Security group for Nautilus App Servers"
  vpc_id      = data.aws_vpc.default.id
}

# Inbound rule to allow SSH (port 22) from anywhere
resource "aws_security_group_rule" "allow_ssh" {
  type              = "ingress"
  from_port         = 22
  to_port           = 22
  protocol          = "tcp"
  cidr_blocks       = ["0.0.0.0/0"]
  security_group_id = aws_security_group.devops.id
}

# Inbound rule to allow HTTP (port 80) from anywhere
resource "aws_security_group_rule" "allow_http" {
  type              = "ingress"
  from_port         = 80
  to_port           = 80
  protocol          = "tcp"
  cidr_blocks       = ["0.0.0.0/0"]
  security_group_id = aws_security_group.devops.id
}
````

### Steps to Implement the Solution

1. **Navigate to your Terraform working directory**

```bash
cd /home/bob/terraform
```

2. **Initialize Terraform**

This will download the necessary provider plugins.

```bash
terraform init
```

3. **Validate the Configuration**:

Ensure that the configuration files are valid:

```bash
terraform validate
```

4. **Review the Execution Plan**:

Terraform will show what it plans to do before making any changes:

```bash
terraform plan
```

5. **Apply the Configuration**:

Apply the Terraform configuration to create the security group and its rules:

```bash
terraform apply --auto-approve
```

6. **Check the Output**:

Verify that the security group has been created successfully in AWS:

```bash
terraform output
```
