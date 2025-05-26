# Teardown Guide

To destroy all resources provisioned by this Terraform project:

```bash
terraform destroy -var-file="terraform.tfvars"
```

Ensure your working directory is the same one where the `.tf` files and state files are stored.
