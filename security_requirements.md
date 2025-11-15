# Security Requirements  
**Project:** OCI Secure VCN Project  
**Cloud:** Oracle Cloud Infrastructure (OCI)

---

## 1. Security Objectives

The OCI Secure VCN Project provides a hardened network foundation.  
Security objectives ensure the confidentiality, integrity, and availability of workloads deployed within this network.

### Identity Protection
- Ensure Terraform and administrative actions use secure RSA key authentication.
- Restrict IAM permissions using least-privilege access patterns.
- Prevent unauthorized modification of VCN, route tables, gateways, or security lists.

### Data Confidentiality
- Encrypt traffic in transit using TLS for OCI API calls.
- Ensure private subnets remain fully isolated from public endpoints.
- Prevent public IP exposure for sensitive workloads.

### Data Integrity
- Infrastructure must deploy consistently using Terraform to avoid configuration drift.
- Explicit CIDRs, security rules, and routing paths must be validated before deployment.

### Availability
- VCN routing must support stable outbound/inbound flows.
- Misconfigurations must not result in unreachable subnets or blocked workloads.

### Auditability
- All identity, network, and configuration changes must be logged via OCI Audit Service.
- Terraform state changes must be traceable.

---

## 2. Derived Requirements

| **Category** | **Requirement** |
|--------------|------------------|
| **Identity & Access** | Use RSA key-based authentication; apply least-privilege IAM policies; restrict Terraform to required network permissions; no wildcard privileges. |
| **Network Security** | Restrict ingress by CIDR; prevent public access to private subnets; enforce explicit routing; use NAT Gateway for secure outbound internet access. |
| **Data Protection** | Enforce HTTPS for all OCI API calls; prevent sensitive values from entering Terraform state or code; avoid plaintext secrets in version control. |
| **Monitoring & Logging** | Enable OCI Audit logs; detect IAM policy updates, route table changes, gateway additions/deletions; maintain log retention for forensics. |
| **Governance** | All changes performed through Terraform (IaC); manual console updates prohibited except for break-glass situations. |
| **Resource Governance** | Apply tagging standards for cost tracking; deploy into designated compartments to maintain separation of duties. |
| **State Security** | Protect Terraform state files; use encrypted Object Storage backend if remote state is enabled. |

---

## 3. Controls Implemented

### Identity Controls
- RSA private key stored locally with `chmod 600`.
- Terraform provider authenticated using documented tenancy/user OCIDs.
- IAM permissions limited to:
  - `manage virtual-network-family`
  - `inspect compartments`

### Network Controls
- Public subnet created with **restricted inbound rules**.
- Private subnet **does not assign public IPs**.
- Internet Gateway supports controlled outbound access from the public subnet.
- NAT Gateway secures outbound access for private subnets.
- Route tables explicitly define paths to Internet Gateway and NAT Gateway.
- Security Lists enforce least-privilege ingress/egress.

### Governance Controls
- Terraform used exclusively to deploy/manage network resources.
- Git version control maintains a full audit history of changes.
- Manual console modifications discouraged and documented when necessary.

### Monitoring Controls
- OCI Audit Service logs all API calls and identity changes.
- Log retention maintained to support troubleshooting or security investigations.
- Terraform outputs exclude sensitive data.

---

## 4. Residual Risks & Mitigations

| **Residual Risk** | **Description** | **Mitigation** |
|-------------------|------------------|----------------|
| **CIDR Misconfiguration** | Incorrect or overlapping CIDRs can break routing and subnet scaling. | CIDR planning, Terraform input validation, documentation review. |
| **Terraform State Exposure** | Local state may expose sensitive resource metadata. | Use encrypted Object Storage remote backend with restricted access. |
| **IAM Policy Drift** | Console edits may bypass IaC governance. | Enforce “Terraform-only” deployments; periodic IAM policy review. |
| **NAT Gateway Cost** | Long-running NAT Gateway increases monthly costs. | Use Budgets + Alerts; tear down unused environments. |
| **Insufficient Log Retention** | Short retention limits ability to investigate incidents. | Extend log retention or export audit logs to Object Storage. |

---
