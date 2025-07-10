# Terraform AWS Infrastructure — Based on Udemy Course

This project is my hands-on implementation of the **"Complete Terraform Course - Beginner to Advanced [2021]"** on Udemy. I'm using it to gain real-world experience deploying infrastructure on AWS using Terraform, and tweaking the examples to better fit my own understanding and workflow.

> 💡 **Note:** I’m fully aware this course is a bit dated, but that doesn’t matter — the core infrastructure-as-code principles, Terraform syntax, AWS fundamentals, and workflow practices are still solid and relevant. I’m using this as a structured way to learn the real-world basics and build my own projects on top of it.


---

## 🔧 What This Project Does

This Terraform configuration sets up a basic AWS environment in the `eu-west-2` (London) region. It includes:

- A custom **VPC** with CIDR block defined via variable
- A **public subnet** in a user-specified availability zone
- An **internet gateway** and route table to allow outbound internet access
- A **default security group** allowing SSH (port 22) and app access (port 8080)
- A **t2.micro EC2 instance** (or any other specified type), launched using the latest Amazon Linux 2 AMI
- An optional **Docker-based setup** via a startup script (`entry-script.sh`) that installs and runs NGINX in a container

---

## 🧱 Terraform Components Used

- **Provider:** AWS (London region)
- **Resources:**
  - `aws_vpc`
  - `aws_subnet`
  - `aws_internet_gateway`
  - `aws_default_route_table`
  - `aws_default_security_group`
  - `aws_instance`
- **Data Sources:**
  - `aws_ami` (Amazon Linux 2)

---

## 📂 Folder Structure

```bash
terraform-udemy/
├── scripts/
│   └── entry-script.sh         # Startup script run on EC2 launch (Docker/NGINX)
├── terraform/
│   ├── main.tf                 # Core Terraform configuration
│   ├── terraform.tfstate       # (gitignored) Local Terraform state
│   ├── terraform.tfvars        # (gitignored) Variable values
│   └── .terraform.lock.hcl     # Provider dependency lock file
├── .gitignore
└── README.md
```

---

## ⚙️ Usage

1. **Set your input variables** in `terraform.tfvars`:
   ```hcl
   vpc_cidr_block     = "10.0.0.0/16"
   subnet_cidr_block  = "10.0.10.0/24"
   avail_zone         = "eu-west-2a"
   env_prefix         = "dev"
   my_ip              = "YOUR_PUBLIC_IP/32"
   instance_type      = "t2.micro"
   ```

2. **Initialize Terraform** (installs providers):
   ```bash
   terraform init
   ```

3. **Apply the configuration**:
   ```bash
   terraform apply --auto-approve
   ```

4. **SSH into your EC2** (adjust path to your key):
   ```bash
   ssh -i ~/keys/server-key-pair.pem ec2-user@<output_public_ip>
   ```

5. **Destroy resources** when done:
   ```bash
   terraform destroy --auto-approve
   ```

---

## 🔐 Security & .gitignore

Sensitive files like `.tfstate`, `.tfvars`, SSH keys, and `.terraform/` directories are excluded via `.gitignore`. Always avoid committing secrets or access keys to GitHub.

---

## 💬 Notes

- This setup uses **user_data** to bootstrap EC2 instances with Docker and NGINX.
- The project is meant for **learning purposes** and **not production-ready**.
- I'm building this repo as I go through the Udemy course to reinforce understanding and track my progress.

---

## 📚 Course Reference

**Course:** [Complete Terraform Course - Beginner to Advanced (2021)]

---

## ✅ To Do / Next Steps

- Break out resources into modules
- Add S3 backend for remote state storage
- Expand security groups and networking for multiple tiers
- Automate key generation and management

---