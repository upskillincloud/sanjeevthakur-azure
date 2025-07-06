## **Steps to Connect to Internet via Hub VNet Using Custom Routes**

### **Scenario**
- **Hub VNet**: Contains a NAT Gateway or Azure Firewall for internet access.
- **Spoke VNets**: Do **not** have direct internet access; all outbound traffic must go through the hub.

---

### **Step 1: Deploy Hub and Spoke VNets**

1. **Create Hub VNet** (`10.0.0.0/24`) with a **Private** NATSubnet (`10.0.0.0/27`).
2. **Create Spoke VNet** (`10.1.0.0/24`) with a **Private** AppSubnet (`10.1.0.0/27`).

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_1](https://github.com/user-attachments/assets/206ea28d-415f-4f26-bbab-4542a025d837)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_2](https://github.com/user-attachments/assets/3465a85c-3882-46a0-a932-4b8d6a235506)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_3](https://github.com/user-attachments/assets/2c05b221-eb20-46a3-a4c3-e71637695392)

---

### **Step 2: Peer Spoke VNet with Hub VNet**

- Set up **VNet peering** between spoke and the hub.
- In peering settings, **enable “Use remote gateway”** on the spoke side and **“Allow gateway transit”** on the hub side.

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_4](https://github.com/user-attachments/assets/25d8673a-9a49-46d1-a37f-27205f382b1c)

---

### **Step 3: Create NSG for AppSubnet in Spoke**

- Inbound Rules:
	Allow from VirtualNetwork → Port: 22, 443, 80
- Outbound Rules:
	Allow to 0.0.0.0/0 → Port: 443, 80 (Priority: 100)

Create a **NAT Gateway** in NATSubnet in the Hub VNet.

- Attach the NAT Gateway to the NATSubnet.

---


### **Step 3: Deploy NAT Gateway or Azure Firewall in Hub VNet**

- Create a **NAT Gateway** in NATSubnet in the Hub VNet.

- Attach the NAT Gateway to the NATSubnet.

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_5](https://github.com/user-attachments/assets/79aab378-8ea1-4fa5-8621-95ac3d9cf2e0)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_6](https://github.com/user-attachments/assets/99a782c6-a017-4486-83e4-79a0367818a6)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_7](https://github.com/user-attachments/assets/68e7c1a6-6b4e-421f-8717-219a13112160)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_8](https://github.com/user-attachments/assets/0ed25a5d-df68-4d66-80a7-08cc4daf3be0)

---

### **Step 4: Create a Route Table for Spoke Subnets** --> **This step is not needed**

1. Go to **Route tables** > **Create**.
2. Add a route:
   - **Address prefix**: `0.0.0.0/0`
   - **Next hop type**:  
     - If using **Azure Firewall**: `Virtual appliance` and set **Next hop IP** to the firewall’s private IP.
     - If using **NAT Gateway**: Associate the NAT Gateway with the NATSubnet (no need to set next hop in route table; NAT Gateway works by subnet association).

---

### **Step 5: Associate Route Table with Spoke Subnet** --> **This step is not needed**

- Associate the custom route table with **spoke subnet**.

---

### **Step 6: Test Connectivity**

- Deploy a BastionVM in Hub BastionSubnet and a ApppVM in spoke AppSubnet.
- Connect to BastionVM in Hub BastionSubnet and then SSH to ApppVM in spoke AppSubnet.
- Try to access the internet (e.g., `curl https://www.microsoft.com`).
- The traffic will route through the hub VNet’s NAT Gateway or Firewall.

---

### **Diagram**

```plaintext
[Spoke VNet] --(Peering)--> [Hub VNet] --(NAT Gateway/Firewall)--> [Internet]
      |                           |
[Custom Route Table]         [NAT Gateway/Firewall]
      |                           |
   No direct internet         Outbound internet access
```

---

### **Key Points**
- **Spoke subnets** have a route for `0.0.0.0/0` pointing to the hub (firewall or NAT).
- **No public IPs** on spoke VMs.
- **All outbound traffic** from spokes goes through the hub for internet access.

---

**This setup centralizes and secures internet access for all spokes via the hub VNet using custom routes.**