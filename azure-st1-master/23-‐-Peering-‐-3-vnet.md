### **Configure VNet Peering in Azure for Hub-and-Spoke Architecture**

In this setup, we will create **3 VNets**: one **Hub VNet** and two **Spoke VNets**. The **Hub VNet** will act as the central point for communication, and the **Spoke VNets** will communicate with each other only through the **Hub VNet**.

---

### **1. Create the VNets**
1. **Hub VNet**:
   We will use Terraform to create the 3 VNets as follows
   - Hub VNet - HubVNet (10.0.0.0/24)
   - Spoke VNet1 - SpokeVNet1 (10.0.0.0/27)
   - Spoke VNet2 - SpokeVNet2 (10.0.0.32/27).

![23_Peering_3_Vnet_1](https://github.com/user-attachments/assets/1f63fde3-c684-46ce-b300-9391f9869367)
  
---

### **2. Configure VNet Peering**

#### **Step 1: Peer Hub VNet with Spoke VNet 1**
1. Go to **HubVNet** in the Azure Portal.
2. Navigate to **Peerings** under the **Settings** section and click **+ Add**.
3. Configure the peering:
   - **Name**: `HubToSpoke1`
   - **Peering Link Name (Hub to Spoke)**: `HubToSpoke1`
   - **Peering Link Name (Spoke to Hub)**: `Spoke1ToHub`
   - **Virtual Network**: Select `SpokeVNet1`.
   - Enable **Allow forwarded traffic** and **Allow gateway transit**.
   - Disable **Allow virtual network access** for Spoke-to-Spoke communication.
4. Click **OK** to create the peering.

#### **Step 2: Peer Hub VNet with Spoke VNet 2**
1. Go to **HubVNet** and navigate to **Peerings**.
2. Click **+ Add** and configure the peering:
   - **Name**: `HubToSpoke2`
   - **Peering Link Name (Hub to Spoke)**: `HubToSpoke2`
   - **Peering Link Name (Spoke to Hub)**: `Spoke2ToHub`
   - **Virtual Network**: Select `SpokeVNet2`.
   - Enable **Allow forwarded traffic** and **Allow gateway transit**.
   - Disable **Allow virtual network access** for Spoke-to-Spoke communication.
3. Click **OK** to create the peering.

#### **Step 3: Disable Direct Peering Between Spoke VNets**
- Do **not** create a direct peering between `SpokeVNet1` and `SpokeVNet2`. This ensures that all communication between the spokes is routed through the **Hub VNet**.

---

### **3. Configure Route Tables**

#### **Step 1: Create a Route Table for Spoke VNets**
1. Navigate to **Route Tables** in the Azure Portal and click **+ Create**.
2. Configure the route table:
   - **Name**: `SpokeRouteTable`
   - **Region**: Same as the VNets.
3. Add a route to the route table:
   - **Route Name**: `DefaultRoute`
   - **Address Prefix**: `0.0.0.0/0` (or the address space of the other spoke VNet if specific routing is required).
   - **Next Hop Type**: **Virtual Network Gateway** (if using a gateway) or **Virtual Appliance** (if using a firewall in the Hub VNet).
4. Associate the route table with the subnets in `SpokeVNet1` and `SpokeVNet2`.

#### **Step 2: Configure the Hub VNet**
- If using a firewall or NVA (Network Virtual Appliance) in the Hub VNet, ensure that it is configured to forward traffic between the spokes.

---

### **4. Test the Connectivity**
1. Deploy a VM in each VNet:
   - One VM in `HubVNet`.
   - One VM in `SpokeVNet1`.
   - One VM in `SpokeVNet2`.
2. Test connectivity:
   - From the VM in `SpokeVNet1`, try to ping the VM in `SpokeVNet2`. The traffic should route through the `HubVNet`.
   - Verify that direct communication between `SpokeVNet1` and `SpokeVNet2` is not possible without going through the hub.

---

### **Diagram: Hub-and-Spoke Architecture with VNet Peering**

```plaintext
                +-------------------+
                |    Hub VNet       |
                |  Address: 10.0.0.0/16 |
                +-------------------+
                        |
        +---------------+---------------+
        |                               |
+-------------------+         +-------------------+
|   Spoke VNet 1    |         |   Spoke VNet 2    |
| Address: 10.1.0.0/16 |         | Address: 10.2.0.0/16 |
+-------------------+         +-------------------+
```

---

### **Key Points**
1. **Hub VNet** acts as the central point for all communication.
2. **Spoke VNets** communicate with each other only through the Hub VNet.
3. **Route Tables** ensure that traffic between spokes is routed through the Hub.
4. **No direct peering** is created between the spokes to enforce the Hub-and-Spoke model.

This setup ensures centralized control, scalability, and secure communication between VNets in Azure.