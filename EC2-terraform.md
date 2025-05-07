
# EC2 Instance Deployment with Terraform

This guide walks you through automating the provisioning of an **Amazon EC2 instance** using **Terraform** — an open-source Infrastructure as Code (IaC) tool for predictable and repeatable cloud deployments.

---

## ✅ Prerequisites

Ensure the following are ready before you begin:

* ✅ **AWS Account**
* ✅ **IAM User** with programmatic access and permissions for EC2, VPC, etc.
* ✅ **Access Key ID** and **Secret Access Key**
* ✅ **Terraform** installed — [Download Terraform](https://developer.hashicorp.com/terraform/downloads)
* ✅ **AWS CLI** installed (optional but recommended) — [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

---

## 📁 Project Structure

```bash
ec2-terraform/
├── main.tf            # Main Terraform configuration
├── variables.tf       # Input variable definitions
├── outputs.tf         # Output values after provisioning
├── terraform.tfvars   # Values for input variables (optional)
```

---

## 🔧 Step 1: Write Terraform Configuration

### `main.tf`

```hcl
provider "aws" {
  region     = var.aws_region
  access_key = var.aws_access_key
  secret_key = var.aws_secret_key
}

resource "aws_instance" "ec2_example" {
  ami           = var.ami_id
  instance_type = var.instance_type
  key_name      = var.key_name

  tags = {
    Name = "Terraform-EC2"
  }
}
```

---

### `variables.tf`

```hcl
variable "aws_region" {
  default = "us-east-1"
}

variable "aws_access_key" {
  type      = string
  sensitive = true
}

variable "aws_secret_key" {
  type      = string
  sensitive = true
}

variable "ami_id" {
  default = "ami-0c94855ba95c71c99"  # Ubuntu 20.04 LTS (us-east-1)
}

variable "instance_type" {
  default = "t2.micro"
}

variable "key_name" {
  description = "Name of an existing AWS key pair"
}
```

---

### `outputs.tf`

```hcl
output "instance_id" {
  value = aws_instance.ec2_example.id
}

output "public_ip" {
  value = aws_instance.ec2_example.public_ip
}
```

---

### `terraform.tfvars` (Optional)

```hcl
aws_access_key = "YOUR_ACCESS_KEY"
aws_secret_key = "YOUR_SECRET_KEY"
key_name       = "your-keypair-name"
```

---

## ⚙️ Step 2: Initialize and Deploy

Run the following commands from the project directory:

```bash
# Initialize Terraform and download provider plugins
terraform init

# Preview the actions Terraform will perform
terraform plan

# Apply the configuration to launch the EC2 instance
terraform apply
```

Confirm by typing `yes` when prompted.

---

## ✅ Output

On successful deployment, Terraform will display the EC2 instance ID and public IP:

```bash
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Outputs:
instance_id = "i-0123456789abcdef0"
public_ip   = "3.92.165.123"
```

---

## Tear Down

To destroy the created resources:

```bash
terraform destroy
```

Confirm when prompted.

---

## Additional Notes

* **Security Group**: You can define custom security groups to allow traffic (e.g., SSH on port 22). Terraform will otherwise use the default.
* **Key Pair**: Ensure the key pair exists in your AWS account, or define one in Terraform using the `aws_key_pair` resource.
* **Terraform State**: The file `terraform.tfstate` maintains your resource state. Do not modify or delete it manually.

---

## Useful Links

* [Terraform AWS Provider Documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
* [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/index.html)

---

Would you like a version of this README formatted for GitHub with badges and a license section?
