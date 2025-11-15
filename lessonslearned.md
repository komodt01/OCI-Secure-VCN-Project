# Lessons Learned

# Lessons Learned  
**Project:** OCI Secure VCN Project   
**Cloud:** Oracle Cloud Infrastructure (OCI)

This project provided hands-on experience designing, deploying, and managing a secure Virtual Cloud Network (VCN) in OCI using Terraform.  
Key lessons learned are documented below for future reference and continuous improvement.

---

## 1. Importance of Clear Network Segmentation
Properly separating public and private subnets is foundational.  
During early iterations, it was easy to misconfigure:
- Public subnets receiving unintended traffic  
- Private subnets accidentally obtaining public IPs  

**Lesson:** Always validate subnet definitions, CIDRs, and IP assignment behavior before deployment. A single mistake can expose workloads or break routing.

---

## 2. Terraform Requires Discipline to Avoid Drift
OCI resources can be modified in the console without Terraform knowing, leading to drift.  
Even minor changes (security list updates, gateway modifications) can cause future Terraform plans to fail.

**Lesson:** Treat Terraform as the source of truth. Avoid console-based changes unless absolutely necessary, and document them when they occur.

---

## 3. IAM Least Privilege is Easy to Misjudge
OCI’s IAM policies are powerful, and it is easy to unintentionally grant more access than needed.  
Early testing showed that:

- `manage virtual-network-family` is broad but required  
- `inspect` permissions were insufficient for Terraform to calculate plans  
- Overly broad policies created potential privilege escalation paths  

**Lesson:** Start with minimum permissions and expand only when Terraform errors indicate a required privilege.

---

## 4. NAT Gateway is the Main Cost Driver
While most VCN components are free, NAT Gateway incurs continuous hourly charges.  
In early lab iterations, the NAT Gateway was left running unnecessarily, driving up cost.

**Lesson:** Destroy NAT-enabled lab environments when not in use, or disable NAT Gateway in initial test deployments unless explicitly needed.

---

## 5. OCI API Key Management Matters
Terraform uses RSA keys to authenticate with OCI.  
Early attempts failed due to:

- Incorrect file permissions  
- Misconfigured config file paths  
- Tenant and user OCIDs being swapped  

**Lesson:** Double-check the OCI config file, validate file permissions, and maintain a secure directory for all lab keys.

---

## 6. Logging and Audit Are Often Overlooked
The VCN deployed correctly, but analyzing configuration changes required enabling and reviewing OCI Audit logs.  
Without this, identifying network misconfigurations becomes difficult.

**Lesson:** Audit logs are critical for forensics and troubleshooting. They should be reviewed during every project iteration.

---

## 7. Documenting the Architecture Helps Prevent Mistakes
Creating an architecture diagram (public/private subnets, gateways, route tables) made it easier to verify what Terraform should build.

**Lesson:** Visualization helps catch logical errors before writing code. Diagrams improve clarity, consistency, and stakeholder communication.

---

## 8. Compartment Choice Matters More Than Expected
Before Terraform deployment, resources were accidentally placed in the wrong compartment.  
This made them appear “missing” in the console.

**Lesson:** Always verify the compartment OCID and use variables to avoid misplacement.

---

## Summary
This project reinforced the importance of:
- Strong IaC discipline  
- Clear network segmentation  
- Least privilege IAM design  
- Cost awareness  
- Continuous logging and audit review  
- Accurate architecture documentation  

These lessons will guide future OCI and multi-cloud network security work.

---

