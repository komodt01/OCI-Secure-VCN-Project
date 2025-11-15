# Teardown Guide  
**Project:** OCI Secure VCN Project  
**Generated:** 2025-11-13  
**Cloud:** Oracle Cloud Infrastructure (OCI)

This guide provides safe, repeatable steps to remove all OCI resources deployed by this Terraform project.  
Follow each step carefully to avoid accidental deletion of shared or dependent infrastructure.

---

## 1. Pre-Teardown Validation

Before running any destroy operations:

### ✓ Confirm You Are in the Correct Directory
Ensure the directory contains the Terraform files for this project:

```bash
ls
# main.tf  variables.tf  terraform.tfvars  .terraform/  terraform.lock.hcl
