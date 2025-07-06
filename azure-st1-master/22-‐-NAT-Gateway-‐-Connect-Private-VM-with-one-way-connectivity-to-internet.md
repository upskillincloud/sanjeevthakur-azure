### **One-Way Connectivity to the Internet in Azure Using NAT Gateway**

---

### **Objective**  
To connect a **private instance** (VM in a private subnet) to the internet for outbound traffic (e.g., downloading updates) while ensuring **one-way connectivity** (no inbound traffic from the internet).

---

### **Steps to Accomplish One-Way Connectivity Using NAT Gateway in Azure**

#### **Step 1: Create a Virtual Network (VNet)**
1. Go to the **Azure Portal**.
2. Navigate to **Virtual Networks** and click **Create**.
3. Configure the VNet:
   - **Name**: `MyVNet`
   - **Address Space**: `10.0.0.0/16`
   - **Region**: East US

![NAT_Gateway_1](https://github.com/user-attachments/assets/5c62b00f-531a-4bc4-95ad-575c9d4dc59f)


4. Add two subnets:
   
   - **pub_hub_subnet**: `10.0.1.0/24` (for Bastion VM).
   - **priv_hub_subnet**: `10.0.2.0/24` (for the private VM & NAT Gateway).

![NAT_Gateway_2](https://github.com/user-attachments/assets/4276fa88-78b8-4d62-a8d4-7f96702debea)

---

#### **Step 2: Deploy 2 Virtual Machines, one each in Public & Private Subnet**
1. Navigate to **Virtual Machines** and click **Create**.
2. Configure the VM as below:
   - **VM1**: `PrivateVM` in **priv_hub_subnet** (`10.0.2.0/24`).
   
     **Public IP**: **None** (to ensure the VM is private).

   - **VM2**: `BastionVM` in **pub_hub_subnet** (`10.0.1.0/24`).
   
3. Complete the setup and deploy the VMs.

![NAT_Gateway_3](https://github.com/user-attachments/assets/50bfbc3d-5118-47dd-95ad-4f16e6911b8e)

---

#### **Step 3: Create a NAT Gateway**

1. Navigate to **NAT Gateways** in the Azure Portal and click **Create**.

2. Configure the NAT Gateway:

   - **Name**: `MyNATGateway`
  
![NAT_Gateway_4_1](https://github.com/user-attachments/assets/61b11bdc-9e16-4fc6-917f-f81137ff687d)

   - **Public IP**: Create a new Public IP.

![NAT_Gateway_4_2](https://github.com/user-attachments/assets/9b02c5ec-8e31-4f92-bb75-e6f15127a49e)

   - **Subnet Association**: Associate the NAT Gateway with the Private Subnet **priv_hub_subnet** (`10.0.1.0/24`).

![NAT_Gateway_4_3](https://github.com/user-attachments/assets/28890b78-6463-4a80-81d5-4492188f929a)
![NAT_Gateway_4_4](https://github.com/user-attachments/assets/1b1f05d1-ea79-405a-9189-ba1cacb2a2a2)

3. Click **Create** to deploy the NAT Gateway.

![NAT_Gateway_4_5](https://github.com/user-attachments/assets/4d3f47cb-13d6-4f83-8285-958cfe251ab7)

---

#### **Step 4: Update the Route Table for the Private Subnet** --> **This step is not needed as No UDR (User-Defined Route) is required when using NAT Gateway.**
    
---

#### **Step 5: Test the Connectivity**
1. SSH into the **Private VM** using a **bastion host**.
2. Test outbound internet connectivity by running a command like:
   ```bash
   ping www.google.com
   ```
3. **As Azure NAT Gateway does NOT support ICMP (ping), we need test using the below 2 commands**
   ```bash
   nslookup www.google.com
   ```

   ```bash
   curl https://www.google.com
   ```


![NAT_Gateway_8](https://github.com/user-attachments/assets/f388de52-870d-4bd8-b0a8-7197f65d868d)

![NAT_Gateway_9](https://github.com/user-attachments/assets/6abd724f-12bc-4a7f-b4da-2e1df37b7950)

![NAT_Gateway_11](https://github.com/user-attachments/assets/fc12449b-109c-4009-b965-663454f0a340)

![NAT_Gateway_12](https://github.com/user-attachments/assets/1f1797b9-bca7-4128-a269-a02f45e7459c)

![NAT_Gateway_13](https://github.com/user-attachments/assets/8949cf6a-a037-4a41-8e65-c805edc70995)

4. **Now disassociate NAT Gateway from the Subnet**

![NAT_Gateway_13_1](https://github.com/user-attachments/assets/5c10399b-3c5f-4b46-9cb2-12b9ab3e6ee1)

![NAT_Gateway_13_2](https://github.com/user-attachments/assets/a8aa4d9c-9129-4e20-9303-c7642ef3cb41)

5. **We are no longer able to connect to www.google.com now, as NAT Gateway has been disassociated**
   
![NAT_Gateway_13_3](https://github.com/user-attachments/assets/4de04f24-34c2-438c-872d-301121765f80)

---

**Why we are unable to ping to www.google.com using NAT Gateway, even after creating outbound security rule for ICMP **

This is a common situation in Azure. By default, **Azure does not allow outbound ICMP (ping) to the internet from VMs**, even if you have a NAT Gateway and outbound NSG rule for ICMP.

---

### **Why?**

- **Azure NAT Gateway** supports outbound TCP and UDP traffic only.  
- **ICMP (ping) is not supported** for outbound internet traffic via NAT Gateway.
- This is [documented by Microsoft](https://learn.microsoft.com/en-us/azure/virtual-network/nat-gateway/nat-overview#protocol-support).

---

### **What Works?**

- Outbound **TCP/UDP** (e.g., `curl`, `wget`, `apt-get`, `yum`, etc.) will work from your private VM via NAT Gateway.
- **ICMP (ping)** to the internet will **not** work, but pinging internal/private IPs within the VNet/subnet (if allowed by NSG) will work.

---

### **How to Test Internet Connectivity Instead?**

From your private VM, try:
```bash
curl https://www.google.com
```
or
```bash
nslookup www.google.com
```
or
```bash
apt-get update
```
These commands should succeed if your NAT Gateway and NSG are configured correctly.

---

**Summary:**  
You cannot ping (ICMP) public internet addresses from a private VM via Azure NAT Gateway. Use TCP/UDP-based tools to verify outbound internet connectivity.

---

### **Diagram: One-Way Connectivity Using NAT Gateway in Azure**

```plaintext
+-----------------------------+       +-----------------------------+
|         Private VM          |       |       NAT Gateway          |
|  Subnet: 10.0.1.0/24        |       |  Subnet: 10.0.2.0/24        |
|  No Public IP               |       |  Public IP Associated       |
+-----------------------------+       +-----------------------------+
            |                                     |
            +-------------------------------------+
                          VNet: MyVNet
                          Address Space: 10.0.0.0/16
            |
+-------------------------------------------------------------+
|                     Azure Internet Gateway                  |
+-------------------------------------------------------------+
```



---

### **Key Points**
1. **NAT Gateway** ensures that the private VM can initiate outbound internet traffic while blocking inbound traffic.
2. The **Private Subnet** does not have a public IP, ensuring it remains inaccessible from the internet.
3. The **Route Table** directs all outbound traffic (`0.0.0.0/0`) from the private subnet to the NAT Gateway.

---

### **Comparison with OCI**
- In **OCI**, a **NAT Gateway** is similarly used to provide one-way internet connectivity for private instances.
- The process in Azure is conceptually the same but involves associating the NAT Gateway with a **public subnet** and updating the **route table** for the private subnet.

This setup ensures secure, one-way connectivity to the internet in Azure.