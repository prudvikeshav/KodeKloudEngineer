# 🔑 AWS EC2 Key Pair Creation for Migration (RSA)

## 📌 Overview

As part of the phased migration to AWS Cloud, the **Nautilus DevOps team** needs to securely manage access to EC2 instances. This involves generating an **RSA key pair**, where the private key is stored locally, and the public key is uploaded to AWS as a key pair for EC2 access.

---

## 🧩 Problem Statement

To ensure secure and controlled access to EC2 instances during the migration, the team needs to:

- **Create an RSA key pair** with a **key size of 4096 bits**.
- Store the **private key** on the local machine at `/home/bob/datacenter-kp.pem`.
- Upload the **public key** to AWS as a key pair for EC2 access.

---

## ✅ Requirements

- **Key Pair Name**: `datacenter-kp`
- **Key Algorithm**: `RSA`
- **Key Size**: `4096 bits`
- **Private Key Location**: `/home/bob/datacenter-kp.pem`

---

## 💡 Solution

This solution uses **Terraform** to automate the creation of the RSA key pair, upload the public key to AWS, and save the private key locally.

### Terraform Configuration

```hcl

# Generate a 4096-bit RSA private key
resource "tls_private_key" "datacenter" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

# Create AWS Key Pair using the public key generated above
resource "aws_key_pair" "datacenter" {
  key_name   = "datacenter-kp"
  public_key = tls_private_key.datacenter.public_key_openssh
}

# Save the private key locally to /home/bob/datacenter-kp.pem with 0600 permissions
resource "local_file" "private_key" {
  content         = tls_private_key.datacenter.private_key_pem
  filename        = "/home/bob/datacenter-kp.pem"
  file_permission = "0600"
}

# Output the name of the key pair
output "key_pair_name" {
  value = aws_key_pair.datacenter.key_name
}
````

### Steps to Implement the Solution

#### 1. Initialize Terraform

To initialize Terraform and download the necessary providers:

```bash
terraform init
```

#### 2. Validate the Configuration

Ensure that your configuration files are valid:

```bash
terraform validate
```

#### 3. Review the Execution Plan

Review what Terraform will do before applying changes:

```bash
terraform plan
```

#### 4. Apply the Configuration

Apply the Terraform configuration to create the key pair:

```bash
terraform apply --auto-approve
```

#### 5. Check Output

After the configuration is applied successfully, the output will display the name of the created key pair:

```bash
key_pair_name = "datacenter-kp"
```
