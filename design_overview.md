# Design Overview  
**Project:** OCI Secure VCN Project   
**Cloud:** Oracle Cloud Infrastructure (OCI)

---

## 1. Business Requirements

The organization requires a **secure, repeatable, and scalable network foundation** in OCI that supports application workloads while enforcing segmentation, governance, and least-privilege access.

**Drivers:**
- Establish a baseline VCN architecture for future workloads.
- Enforce strong network boundaries (public vs. private subnets).
- Prevent exposure of internal workloads to the public internet.
- Ensure all infrastructure is defined, deployed, and governed through Terraform (IaC).
- Support compliance frameworks including NIST 800-53, ISO 27001, and PCI DSS.
- Reduce misconfiguration risks and operational drift.

**Success Criteria:**
- VCN deployed end-to-end using Terraform with zero manual steps.
- Private subnets use NAT Gateway for secure outbound traffic.
- Public subnets allow only minimal, controlled ingress.
- OCI Audit logs record network and IAM configuration activity.
- Infrastructure reproducible in any compartment or environment.

---

## 2. Architecture Summary

### Identity
- Terraform authenticates with OCI using a securely stored RSA key pair.
- IAM policies enforce least privilege and restrict Terraform execution to:
  - `manage virtual-network-family`
  - `inspect compartments`

### Network
- **VCN (Virtual Cloud Network):** Primary network boundary.
- **Public Subnet:** Restricted ingress; for internet-facing components only.
- **Private Subnet:** No public IP assignment; for workloads requiring isolation.
- **Internet Gateway:** Enables outbound access from the public subnet.
- **NAT Gateway:** Allows private subnet workloads to reach the internet securely.
- **Route Tables:** Explicit routing for each subnet.
- **Security Lists:** Least-privilege inbound/outbound rules with tight CIDRs.

### Logging & Monitoring
- OCI Audit Service captures IAM and network configuration events.
- Logs can optionally be forwarded to Log Analytics or Object Storage.
- Terraform output avoids sensitive values for security.

### Governance
- All infrastructure changes governed by IaC.
- No console-driven manual changes (except in emergencies).
- Resource tagging supported for cost visibility and lifecycle management.

---

## 3. Design Trade-offs

### Security vs. Usability
- Public subnet is retained to support future bastion or ingress solutions.
- For maximum security, a bastionless pattern (Bastion Service + private subnets) would be preferred.

### NAT Gateway Cost vs. Isolation
- NAT Gateway adds cost but ensures private workloads never obtain a public IP or direct internet path.
- Chosen to maximize isolation and compliance alignment.

### Security Lists vs. Network Security Groups
- Security Lists are simpler and suitable for foundational labs.
- NSGs provide more granular segmentation, recommended for mature workloads.

---

## 4. Cost Estimate

| Component | Estimated Cost | Notes |
|----------|----------------|-------|
| VCN | Free | Core networking is free in OCI |
| Subnets | Free | Logical segmentation |
| Route Tables | Free | No charge |
| Internet Gateway | Free | No cost to use |
| NAT Gateway | ~$0.045/hour | Main cost driver |
| Terraform Execution | Free | Runs locally |

**Estimated Monthly Cost:**  
✨ **$30–$40/month**, depending primarily on NAT Gateway uptime.

---

## 5. Well-Architected Pillar Mapping

| Pillar | Design Decision | Rationale |
|--------|-----------------|-----------|
| **Security** | Segmented subnets, NAT egress, IAM least privilege | Protects workloads and enforces Zero Trust principles |
| **Resilience** | Declarative IaC allowing rebuild on demand | Supports predictable recovery and environment cloning |
| **Performance** | Logical separation of subnets | Allows workloads to scale independently |
| **Cost Optimization** | Minimal resource footprint; NAT gateway only when needed | Reduces unnecessary spending |
| **Operational Excellence** | Terraform-driven changes and version control | Reduces drift, improves maintainability |

---
