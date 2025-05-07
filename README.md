# 🚀 EC2 Automation with Terraform

This guide provides a step-by-step approach to automating the provisioning and management of an EC2 instance using **Terraform**.

By the end of this tutorial, you'll be able to:
- Install and configure Terraform
- Set up AWS credentials
- Write and apply Terraform configuration
- Deploy, verify, and destroy an EC2 instance

---

## 1. 📥 Install Terraform

Before you begin, install Terraform:

- Download the latest version from the [Terraform Downloads Page](https://www.terraform.io/downloads)
- Follow OS-specific installation instructions
- Verify the installation:

```bash
terraform -v
```

##  2. 🔐 Configure AWS Credentials

```bash
aws configure
```

You'll be prompted to enter:

AWS Access Key ID

AWS Secret Access Key

Default region (e.g., us-east-1, eu-west-2)

Default output format (optional: json, text, or table)

Verify your setup:

```bash
aws sts get-caller-identity
```

## 📂 Create Terraform Configuration Directory

```bash
mkdir terraform-ec2 && cd terraform-ec2
touch main.tf
```

## 4. ✍️ Define the EC2 Instance in main.tf

```hcl
provider "aws" {
  profile = "default"          # Uses your AWS CLI profile
  region  = "eu-west-2"        # Update this to your region
}

resource "aws_instance" "UGO_Server" {
  ami           = "ami-0cbf43fd299e3a464"  # Ensure this AMI is valid in your region
  instance_type = "t2.micro"

  tags = {
    Name = "MyNCAAInstance"
  }
}
```
🔎 To find a valid AMI ID for your region:

```bash
aws ec2 describe-images --owners amazon --filters "Name=name,Values=amzn2-ami-hvm-*-x86_64-gp2"
```

## 5. ⚙️ Initialize Terraform

```bash
terraform init
```

This command:

Downloads the required provider plugins

Initializes the project directory

Prepares Terraform for execution

## 6. ✅ Validate and Plan

```bash
terraform validate
```
```bash
terraform plan
```

## 7. 🚀 Apply the Configuration
Deploy the EC2 instance:

```bash
terraform apply -auto-approve
```

## 8. 🔍 Verify the Instance

```bash
aws ec2 describe-instances \
  --query 'Reservations[*].Instances[*].[InstanceId,State.Name,PublicIpAddress]' \
  --output table
```

## 9. 🧹 Destroy the Instance
Clean up resources when done:

```bash
terraform destroy -auto-approve
```

Terraform will:

Identify resources

Confirm (auto-approved)

Destroy the instance

## 🏁 10. Conclusion
You have successfully:

Installed and configured Terraform

Set up AWS credentials

Created and applied a Terraform EC2 configuration

Verified and destroyed an EC2 instance

🎉 Congrats on automating your cloud infrastructure!
