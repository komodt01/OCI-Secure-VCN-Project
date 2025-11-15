# Cost Considerations  
**Project:** OCI Secure VCN Project  
**Generated:** 2025-11-13  
**Cloud:** Oracle Cloud Infrastructure (OCI)

This document outlines the cost implications of the resources deployed by the OCI Secure VCN Project.  
Although the architecture is lightweight and designed for lab and foundational use, certain components — particularly gateways — introduce ongoing costs.

---

## 1. Cost Drivers Summary

| Component | Cost | Notes |
|----------|------|-------|
| **VCN** | Free | Core networking resources are free in OCI |
| **Subnets** | Free | Logical segmentation only |
| **Route Tables** | Free | No charge |
| **Security Lists / NSGs** | Free | No charge |
| **Internet Gateway** | Free | Zero cost to create or use |
| **NAT Gateway** | ~$0.045/hr | Main recurring cost driver |
| **Terraform Execution** | Free | Runs locally using WSL2 |
| **Audit Logs** | Free (storage may incur cost if exported) | Native audit service is free |

---

## 2. Monthly Cost Estimate

Assuming a typical lab scenario with the NAT Gateway running full-time:

### **NAT Gateway**
- $0.045 per hour  
- ~720 hours per month  
= **~$32.40/month**

### **Total Estimated Monthly Cost**
**≈ $30–$40/month**, depending on NAT usage and whether logs are exported.

---

## 3. Cost Optimization Opportunities

### **A. Disable NAT Gateway When Not Needed**
If private subnets do not require outbound access for certain phases of testing, the NAT Gateway can be removed or commented out in Terraform to reduce costs.

### **B. Tear Down Environments After Use**
Because this is a development/lab-focused deployment, destroying the infrastructure when idle prevents unnecessary spend.

Use:
```bash
terraform destroy -var-file="terraform.tfvars"

### **C. Use Budget Alerts

OCI Budgets allow creation of:
Spending alerts
Forecast alerts
Cost anomaly detection
These are useful for both personal projects and enterprise production governance.

### ** D. Log Storage Optimization
While Audit Logs are free, exporting them to Object Storage generates:
Storage cost
Lifecycle cost (if configured)
If high log volume is not required, keep short retention windows

### **E. NAT-Free Architecture (Advanced Option)
For specific workloads, alternatives include:
Private-only compute with service gateways
Bastion-based access
Use of Function-as-a-Service
VCN Flow Logs for observability without NAT traffic
These reduce NAT dependency and overall gateway costs.

---

## 4. Hidden Costs to Be Aware Of

### Although small in this project, these costs can grow if the architecture scales:
- Object Storage buckets for remote Terraform state  
- VCN Flow Logs (charged per GB ingested)  
- Dynamic Routing Gateway (DRG) if added later  
- Peering bandwidth charges in multi-VCN designs  

These are not part of the current project but should be considered in expansions.
s
