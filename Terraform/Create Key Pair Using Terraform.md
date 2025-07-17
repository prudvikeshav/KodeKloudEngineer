📌 **Problem Statement**:

The Nautilus DevOps team is preparing for a phased migration of their infrastructure to AWS Cloud. To ensure a smooth and controlled transition, they are breaking the migration into smaller tasks.

As part of this effort, the team needs to create an RSA key pair to securely manage access to EC2 instances. The private key should be saved on the local machine, and the public key uploaded to AWS as a key pair.

✅ **Requirements:**

*Key Pair Name: datacenter-kp*

*Key Algorithm: RSA*

*Key Size: 4096 bits*

*Private Key Location: /home/bob/datacenter-kp.pem*


**Solution**

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
```
*Initialize Terraform*

```bash
terraform init
```
*Validate the Configuration*

```bash
terraform validate
```
*Review the Execution Plan*

```bash
terraform plan
```
*Apply the Configuration*

```bash

terraform apply --auto-approve
```
*Check Output*

After successful execution, the output should display:

```bash
key_pair_name = "datacenter-kp"
```