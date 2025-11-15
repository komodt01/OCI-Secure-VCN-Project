# Risks and Mitigations  
**Project:** OCI Secure VCN Project  
**Generated:** 2025-11-13  
**Cloud:** Oracle Cloud Infrastructure (OCI)

---

## Overview
Every cloud network architecture carries inherent risks related to identity, configuration, network exposure, auditing, and operational control.  
This document outlines the key risks associated with the OCI Secure VCN Project and the mitigations implemented to reduce the likelihood or impact of each.

---

## Risks & Mitigations

| **Risk** | **Description** | **Impact** | **Likelihood** | **Mitigation Strategy** |
|----------|------------------|------------|----------------|--------------------------|
| **Misconfigured IAM Policies** | Permissions granted too broadly can lead to unauthorized network changes or privilege escalation. | High | Medium | Enforce least privilege; restrict Terraform to network-only permissions; use groups and compartments for separation. |
| **Exposed RSA Private Keys** | If Terraform’s private key is leaked, OCI API access may be compromised. | High | Low | Store keys securely with `chmod 600`; never commit keys to Git; rotate keys regularly. |
| **CIDR Overlap or Incorrect Network Design** | Overlapping or invalid CIDRs can break routing or prevent future subnet expansion. | Medium | Medium | Pre-validate CIDRs; include Terraform `validation` blocks; maintain documented IP addressing strategy. |
| **Terraform State Exposure** | Terraform state files may contain metadata or references that should not be public. | High | Low | Use encrypted Object Storage backend; restrict access; avoid storing sensitive values in state. |
| **Lack of Audit Logging** | Without logs, security events cannot be investigated or correlated. | Medium | Low | Rely on OCI Audit Service; ensure logs are retained or exported; configure notifications for IAM and network changes. |
| **NAT Gateway Cost Overruns** | NAT Gateway charges accrue when enabled 24/7. | Medium | Medium | Use budgets + alerts; shut down lab resources when not needed; evaluate NAT-free designs when possible. |
| **Security List Misconfiguration** | Overly permissive inbound/outbound rules can expose workloads. | High | Low | Apply least-privilege firewall rules; restrict CIDRs; periodically review rules. |
| **Manual Console Changes (Configuration Drift)** | Changes made outside Terraform undermine consistency and governance. | Medium | Medium | Enforce IaC-only deployment policy; lock down IAM permissions; use Git reviews for all Terraform changes. |

---

## Residual Risks
Even with strong controls, certain risks remain:

- Human error in future subnet expansion  
- Unexpected routing interactions in multi-VCN or peered environments  
- Log retention dependent on tenancy-level settings  
- Cost variance if NAT usage increases over time  

These risks should be reviewed periodically with architecture and security teams.

---

## Summary
This risk register helps ensure that identity, network, and governance vulnerabilities are identified and controlled early.  
By using Terraform and OCI native controls, the architecture maintains strong security posture with minimal operational overhead.

---
