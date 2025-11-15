# Compliance Mapping  
**Project:** OCI Secure VCN Project  
**Generated:** 2025-11-13  
**Cloud:** Oracle Cloud Infrastructure (OCI)

This document maps the security controls implemented in the OCI Secure VCN Project to common industry frameworks.  
The mapping demonstrates how IaC-driven cloud networking decisions align with recognized security and governance standards.

---

## 1. NIST 800-53 Rev. 5 Mapping

| NIST Control | Description | How This Project Meets It |
|--------------|-------------|----------------------------|
| **AC-2 (Account Management)** | Manage access and permissions | IAM least-privilege Terraform policies; no console-only configuration |
| **AC-3 (Access Enforcement)** | Restrict access to authorized users | RSA key-based provider auth; controlled network access |
| **AC-4 (Information Flow Enforcement)** | Enforce flow restrictions | Subnet boundaries, route tables, and security list rules |
| **AU-2 (Audit Events)** | Log security-relevant events | OCI Audit logs enabled by default for network and IAM actions |
| **AU-6 (Audit Review, Analysis)** | Review and monitor logs | Audit logs can be queried for IAM, VCN, and gateway activity |
| **CM-2 (Baseline Configuration)** | Enforce baseline configurations | Infrastructure-as-Code (Terraform) enforces consistent VCN deployments |
| **CM-3 (Configuration Change Control)** | Control modifications | All modifications occur via `terraform plan/apply`; no unmanaged drift |
| **SC-7 (Boundary Protection)** | Restrict and monitor network communications | Internet Gateway, NAT Gateway, routing rules, and security lists |
| **SC-13 (Cryptographic Protection)** | Encryption in transit & at rest | OCI-native encryption; secure API key handling |
| **SC-28 (Protection of Information at Rest)** | Stored data must be protected | All OCI resources encrypted by default; Terraform state can use encrypted storage |

---

## 2. ISO 27001:2022 Annex A Mapping

| ISO Control | Description | How This Project Meets It |
|-------------|-------------|----------------------------|
| **A.5.15 Access Control** | Ensure appropriate user access | IAM policy restrictions + key-based provider authentication |
| **A.5.17 Authentication** | Secure authentication mechanisms | RSA key pair used for provider authentication |
| **A.8.20 Logging** | Log and monitor actions | Native OCI Audit logs automatically track changes |
| **A.8.9 Configuration Management** | Standardized configurations | Terraform standardizes network resources and policy creation |
| **A.8.28 Secure Coding** | Maintain secure IaC | No hard-coded secrets; variables + `.tfvars` separation |
| **A.7.4 Network Security** | Protect network boundaries | VCN structure, security lists, Internet/NAT gateways |

---

## 3. CIS OCI Foundations Benchmark Mapping

| CIS Section | Requirement | Alignment |
|-------------|-------------|-----------|
| **1.1 - IAM** | Avoid unnecessary user access | No console passwords; Terraform uses API keys only |
| **2.1 - Networking** | VCNs should restrict public access | Only public subnet has IGW; private subnet stays isolated |
| **2.3 - Routing** | Route tables should not expose internal networks | Routes scoped to specific gateways only |
| **3.4 - Logging** | Ensure Audit is enabled | OCI Audit enabled by default; validated in project |
| **3.5 - Object Storage Security** | Secure storage for state files | (If used) encrypted Object Storage buckets recommended |

---

## 4. Control Coverage Summary

| Area | Coverage |
|-------|----------|
| **Identity & Access** | ✔ Strong least-privilege IAM + API-key auth |
| **Network Security** | ✔ Segmentation, routing control, security lists |
| **Monitoring & Audit** | ✔ Audit logs enabled; centralized review |
| **Governance** | ✔ Terraform enforces consistent, compliant config |
| **Data Security** | ✔ Encryption by default |

---

## 5. Gaps & Not Implemented (Honest Transparency)

| Control Area | Status | Notes |
|--------------|--------|-------|
| SOC 2 Logging Retention | ❌ Not implemented | Would require Object Storage export |
| Multi-Region Resilience | ❌ Not included | Lab-focused, single-region |
| Advanced IAM Conditions | ❌ Not required | Could add time/IP-based access in future |
| Flow Logs | Optional | Not enabled by default; can be added |

---

## Summary

This project demonstrates strong alignment with:
- **NIST 800-53 Rev. 5**
- **ISO 27001:2022**
- **CIS OCI Foundations**

Even though this is a lab-scale VCN deployment, the design principles and IaC workflow reflect real enterprise security and governance best practices.

---
