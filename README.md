# OCI Secure VCN Project

## 📌 Overview
This project provisions a **secure Virtual Cloud Network (VCN)** in Oracle Cloud Infrastructure (OCI) using Terraform.  
It establishes a hardened, repeatable network foundation that supports application, database, and security workloads while enforcing strong baseline controls for identity, routing, segmentation, and governance.

The design ensures:
- Least-privilege IAM access
- Segmented public and private subnets
- Secure outbound-only internet access for private resources (NAT)
- Enforced Infrastructure-as-Code change control
- Full auditability via OCI Audit Service

This project serves as a reusable foundation for future OCI projects and multi-cloud portfolio work.

---

## 🧰 Technologies Used

| Technology              | What It Is                                                                 | Why It Was Used                                               |
|------------------------|-----------------------------------------------------------------------------|---------------------------------------------------------------|
| **Terraform**          | Infrastructure as Code (IaC) tool                                          | Enables repeatable, declarative provisioning of OCI resources |
| **Oracle Cloud (OCI)** | Oracle’s public cloud platform                                             | Target environment for secure network provisioning            |
| **OCI Terraform Provider** | Plugin enabling Terraform–OCI API integration                         | Required to create VCNs, gateways, route tables, IAM policies |
| **RSA Key Pair**       | Asymmetric authentication mechanism                                        | Used for secure provider authentication into OCI APIs         |
| **Ubuntu WSL2**        | Linux subsystem for Windows                                                | Execution environment for Terraform and key generation        |
| **Visual Studio Code** | Source code editor                                                         | For authoring and managing Terraform configurations           |

---

## 🧪 Commands Used

```bash
# Install and update
sudo apt update && sudo apt install -y gnupg software-properties-common curl

# HashiCorp key and repo
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

# Install Terraform
sudo apt update && sudo apt install terraform

# RSA key generation
openssl genrsa -out ~/.oci/oci_secure_vcn_key.pem 2048
openssl rsa -pubout -in ~/.oci/oci_secure_vcn_key.pem -out ~/.oci/oci_secure_vcn_key_public.pem
chmod 600 ~/.oci/oci_secure_vcn_key.pem

# Terraform workflow
terraform init -upgrade
terraform validate
terraform plan -var-file="terraform.tfvars"
terraform apply -var-file="terraform.tfvars"

