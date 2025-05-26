# Lessons Learned

- OCI authentication requires properly formatted and permissioned private keys.
- `.tfvars` path references must be absolute or relative and confirmed valid.
- Double-check `provider` block and key format for compatibility with Terraform OCI provider.
- OCI's private key authentication is sensitive to PEM encoding and newline characters.
