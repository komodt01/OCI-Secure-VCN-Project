Cost Considerations

Project: OCI Secure VCN Project
Generated: 2025-11-13
Cloud: Oracle Cloud Infrastructure (OCI)

This document outlines the cost implications of the OCI Secure VCN Project.
Although the architecture is lightweight and intended for lab use, specific components—particularly network gateways—introduce recurring costs that should be monitored

## 1. Cost Drivers Summary

| Component                  | Cost                         | Notes                                                       |
|---------------------------|------------------------------|-------------------------------------------------------------|
| VCN                       | Free                         | Core networking resources are free                          |
| Subnets                   | Free                         | Logical segmentation only                                   |
| Route Tables              | Free                         | No charge                                                   |
| Security Lists / NSGs     | Free                         | No charge                                                   |
| Internet Gateway          | Free                         | No cost to create or use                                   |
| NAT Gateway               | ~$0.045/hr                   | Main recurring cost driver                                  |
| Terraform Execution       | Free                         | Runs locally in WSL2                                       |
| Audit Logs                | Free (Object Storage billed) | Exported logs incur Object Storage costs                    |


2. Monthly Cost Estimate
Based on a typical lab setup where the NAT Gateway runs continuously:
NAT Gateway
$0.045 per hour
~720 hours/month
$32.40/month
Total Estimated Monthly Cost
≈ $30–$40/month, depending on:

Gateway uptime
Whether logs are exported
Any additional networking or DRG usage

3. Cost Optimization Opportunities
A. Disable NAT Gateway When Not Needed
If private subnet workloads do not require outbound access (e.g., early-phase build testing), the NAT Gateway can be commented out in Terraform to eliminate the main recurring cost.
B. Tear Down Environments After Use
Since this deployment is intended for labs, development, and testing, tearing down unused environments prevents unnecessary billing.

terraform destroy -var-file="terraform.tfvars"

C. Use Budget Alerts
OCI Budgets allow the creation of:
Spending alerts
Forecast-based alerts
Cost anomaly alerts
Useful for visibility in both personal and enterprise settings.

D. Optimize Log Storage
OCI Audit Logs are free, but exporting logs generates:
Object Storage cost
Lifecycle cost (if retention rules are applied)

To reduce cost:
Keep short retention windows
Avoid exporting unless required for compliance or analysis

E. NAT-Free Architecture (Advanced Option)
Architectural alternatives to reduce NAT dependency:
Private-only compute with Service Gateway access
Bastion-based administration
Function-as-a-Service (no NAT required)

VCN Flow Logs for observability without NAT routing
These design patterns can dramatically reduce traffic-based gateway spend.

4. Hidden Costs to Be Aware Of
Although minimal in this specific project, the following costs can increase as your OCI footprint grows:
Object Storage buckets (for remote Terraform state or log exports)
VCN Flow Logs (charged per GB ingested)
Dynamic Routing Gateway (DRG) if multi-VCN or hybrid networking is added

Peering bandwidth charges in multi-region or multi-VCN designs

These items are not part of the current lab but should be evaluated for future scaling.
