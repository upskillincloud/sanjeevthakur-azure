## OCI to Azure Migration – Short Interview-Ready Version

I led the migration of a core application from OCI to Azure to standardize platforms and leverage Azure-native integration and security. We used ExpressRoute private peering to connect OCI to Azure’s Hub VNet, where traffic was routed through a Palo Alto Firewall and Application Gateway before reaching the appropriate tier in a Spoke VNet (web, app, DB).

I designed the network and infrastructure using modular Terraform code, stored and version-controlled in GitHub. Existing resources were imported into Terraform state; new ones were created as needed. The workflow included `terraform init`, `plan`, and `apply`, followed by DevOps review and CI/CD integration in Azure DevOps.

This approach ensured secure, automated, and auditable infrastructure provisioning, with clear network segmentation and governance between OCI and Azure.

---

## 🔹 OCI to Azure Migration – Realistic Interview Narrative

I led a critical migration from **Oracle Cloud Infrastructure (OCI) to Azure** for a core application called **CSL Services**, which handled customer loyalty and partner transactions. The migration was driven by a business requirement to standardize platforms, improve integration with Microsoft 365, and leverage Azure-native monitoring and security.

---

### **Network & Traffic Flow**

- **Traffic Origin:** OCI, with **ExpressRoute** configured for private peering to Azure.
- **Entry Point:** Traffic entered Azure through the **Virtual Network Gateway** in the **Hub VNet**.

#### **Hub VNet Design (Subnets):**
- **GatewaySubnet:** For the ExpressRoute/VPN Gateway.
- **FirewallSubnet:** Deployed a Palo Alto Firewall for traffic inspection.
- **AppGatewaySubnet:** Hosted the Application Gateway.
- **NATSubnet:** Used for outbound traffic via NAT Gateway.

#### **Spoke VNet Design (3-Tier Model):**
- **WebSubnet:** Frontend VMs.
- **AppSubnet:** Mid-tier logic VMs.
- **DBSubnet:** Backend database connectivity.

---

### **Traffic Path**

1. **Traffic from OCI** entered the **GatewaySubnet**.
2. Inspected by the **Palo Alto Firewall** in the **FirewallSubnet**.
3. Passed through the **Application Gateway** for SSL termination and path-based routing.
4. Routed to the appropriate tier in the **Spoke VNet** (Web, App, or DB).
5. **Custom UDRs** managed all traffic paths.
6. **NSGs** were configured to allow only required ports (e.g., 443, 22, 1433) per subnet.

---

### **Infrastructure as Code & DevOps**

- **Terraform** was used for designing and implementing the entire structure.
- **Modular Terraform code** was created and stored in a **GitHub repository** with version control and SSH key-based access.
- The repo was structured into folders: `network`, `compute`, `storage`, and `security` for clarity and modularity.
- **Existing resources** were imported into Terraform state; missing resources were created from scratch.
- This migration was a mix of **greenfield design** in Azure and **brownfield discovery** from OCI.

---

### **Terraform Workflow**

1. `terraform init` – Initialize the working directory.
2. `terraform plan` – Validate the blueprint.
3. `terraform apply` – Deploy the infrastructure.
4. `git push` – Push code back to the repo for DevOps review and approval.
5. Integration into the **CI/CD pipeline** via Azure DevOps for further validation and deployment.

---

### **Outcome**

- Achieved **secure, automated, version-controlled infrastructure provisioning**.
- Ensured **seamless network integration** between OCI and Azure.
- Maintained **full visibility and governance** through the DevOps pipeline.