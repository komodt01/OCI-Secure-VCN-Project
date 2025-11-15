# Teardown Guide  
**Project:** OCI Secure VCN Project   
**Cloud:** Oracle Cloud Infrastructure (OCI)

This guide provides safe, repeatable steps to remove all OCI resources deployed by this Terraform project.  
Follow each step carefully to avoid accidental deletion of shared or dependent infrastructure.

---

## Pre-Teardown Validation

Before running any destroy operations:

### ✓ Confirm You Are in the Correct Directory
Ensure the directory contains the Terraform files for this project:

```bash
ls
# main.tf  variables.tf  terraform.tfvars  .terraform/  terraform.lock.hcl

cat ~/.oci/config
echo $TF_VAR_tenancy_ocid

terraform state list

terraform destroy -var-file="terraform.tfvars"

Post-Teardown Cleanup

After Terraform finishes:

✓ Validate Resource Deletion in OCI Console

In the correct compartment, verify that:

The VCN no longer exists

Public/private subnets are gone

Route tables are deleted

Internet Gateway is removed

NAT Gateway is removed

Security Lists no longer appear

If you generated keys for this lab only, delete them locally and in OCI:
rm -f ~/.oci/oci_secure_vcn_key.pem ~/.oci/oci_secure_vcn_key_public.pem

Situations Where You Should NOT Destroy

Avoid running terraform destroy when:

The VCN is now part of a larger multi-project environment.

Other teams/services depend on this VCN or its subnets.

The environment has been promoted from lab → staging → production.

If unsure, perform a dependency review before teardown

Summary
Use terraform destroy to safely remove all managed resources.
Validate deletion in Console and clean up local artifacts.
Never destroy shared or production network environments without review.

This process ensures controlled deprovisioning consistent with cloud governance best practices
