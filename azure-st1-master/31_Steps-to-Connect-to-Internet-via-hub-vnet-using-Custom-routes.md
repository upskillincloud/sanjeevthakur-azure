## Steps to Allow AppVM in Spoke to Access Internet via Hub NAT Gateway


### 1. Create Hub VNet
- **Address space:** `10.0.0.0/24`
- **Subnets:**
  - `NatSubnet` → `10.0.0.0/27`
  - `BastionSubnet` → `10.0.0.32/27`

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_1](https://github.com/user-attachments/assets/0a12fe15-814d-4c18-8e81-2973d940f6c7)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_2](https://github.com/user-attachments/assets/4f0b1027-9e5b-4c90-891b-1ec3c0c04d1b)

---

### 2. Create Spoke VNet
- **Address space:** `10.1.0.0/24`
- **Subnet:** `AppSubnet` → `10.1.0.0/27`

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_3](https://github.com/user-attachments/assets/0ff7c2a2-fefd-453e-843b-f8048dc1bb8f)

---

### 3. Create NSG for AppSubnet in Spoke
- **Inbound Rules:**
  - Allow from `VirtualNetwork` → Ports: `22`, `443`, `80`
- **Outbound Rules:**
  - Allow to `0.0.0.0/0` → Ports: `443`, `80` (Priority: 100)

### 4. Create NSG for NatSubnet in Hub
- **Inbound Rules:**
  - Allow from `VirtualNetwork` → Port: `*` (Priority: 100)
- **Outbound Rules:**
  - Allow to `0.0.0.0/0` → Ports: `443`, `80` (Priority: 100)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_19](https://github.com/user-attachments/assets/913f2798-ed9a-4f28-8ab4-ebcb2d1a84a1)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_21](https://github.com/user-attachments/assets/91e31a7d-42a6-4995-8d9a-d877c17b6f9b)

---

### 5. Create NAT Gateway
- Associate a new Public IP address.
- Link NAT Gateway to `NatSubnet` in Hub VNet.

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_5](https://github.com/user-attachments/assets/65bcc968-afef-47e9-b3f2-eee46b3c5a37)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_6](https://github.com/user-attachments/assets/5c801360-e2fe-4804-83b4-c4a40d31614d)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_7](https://github.com/user-attachments/assets/f130e4a8-dfcf-4cfb-bd01-c38a8e7301d6)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_8](https://github.com/user-attachments/assets/48ff6af6-c6e2-42c1-96fe-b3019bdea777)


---

### 6. Deploy NVA - nat-forwarder (Linux VM) in Hub's NatSubnet
- Choose Ubuntu or similar.
- NIC: Enable IP forwarding.
- No public IP.
- Assign private IP (e.g., `10.0.0.4`).

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_17](https://github.com/user-attachments/assets/bcdde161-1d4e-4e16-8c46-8182d29e2ced)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_18](https://github.com/user-attachments/assets/f4f9045b-94d8-4eb0-915b-044bdbfd4343)

---

### 7. Enable IP Forwarding Inside NVA
SSH into NVA and run:
```bash
sudo sysctl -w net.ipv4.ip_forward=1
sudo sed -i 's/#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/' /etc/sysctl.conf
sudo sysctl -p
```

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_28](https://github.com/user-attachments/assets/09e14290-128c-49ab-b446-4f48cd1dc70e)

### 8. Configure iptables on NVA
```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo apt install iptables-persistent -y
sudo netfilter-persistent save
```

---

### 9. Deploy BastionVM (Linux VM – Public IP) in Hub's BastionSubnet to act as the Jump server. Also Deploy AppVM (Linux VM – No Public IP) in Spoke's AppSubnet

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_10](https://github.com/user-attachments/assets/3e3a2f4d-0335-44f8-b945-b334d32eb4c2)

---

### 10. Create VNet Peering: Hub ↔ Spoke
- **Hub to Spoke:**
  - Allow virtual network access: Yes
  - Allow forwarded traffic: Yes
- **Spoke to Hub:**
  - Allow virtual network access: Yes
  - Allow forwarded traffic: Yes

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_4](https://github.com/user-attachments/assets/a5f55ee4-bb39-4584-820f-4e18de86bb5c)

---

### 11. Create Route Table and Associate with AppSubnet (Spoke)
- **Route Name:** `Custom_Route_To_NATGateway_in_Hub`
- **Address prefix:** `0.0.0.0/0`
- **Next hop type:** Virtual Appliance
- **Next hop address:** `10.1.0.4` (private IP of NVA)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_9](https://github.com/user-attachments/assets/fa15b993-b3b5-4df2-a81a-f6ec54b4ecb5)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_9_3](https://github.com/user-attachments/assets/eb9175c4-f5f3-432f-9deb-9efc9f4d38f7)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_15](https://github.com/user-attachments/assets/7c7cb2d8-0fa6-417d-a269-54be9dd3c867)

---

### 13. Validate End-to-End
- Connect to BastionVM, then SSH into AppVM in Spoke.
- Run:
  ```bash
  curl -4 -v https://www.microsoft.com
  traceroute www.microsoft.com
  ```
- **Expected:** First hop is NVA IP, connection succeeds via NAT Gateway.

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_23](https://github.com/user-attachments/assets/0f27e109-e428-4dc5-a786-dd8d7913c6cd)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_24](https://github.com/user-attachments/assets/2f971ceb-8cf4-4914-a5cd-6da6b21488a4)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_26](https://github.com/user-attachments/assets/a1fadb92-a9c5-4857-b9e7-09a30df5a25b)

![31_Connect_Internet_via_hub_ vnet_using_Custom_routes_27](https://github.com/user-attachments/assets/562a9cb8-8e1d-4125-890b-efbfd365bee5)


---
