### **Steps to Create Azure Resources Using Terraform in Azure Cloud Shell**

---

### **1. Open Azure Cloud Shell**
1. Log in to the **Azure Portal**.
2. Click on the **Cloud Shell** icon in the top-right corner.
3. Select **Bash** as the shell environment.


![Terraform_1](https://github.com/user-attachments/assets/8f0b6340-b6c9-44aa-bebe-baac051ddaec)

![Terraform_2](https://github.com/user-attachments/assets/0e845235-753f-4366-b62e-a82cc4a05924)

![Terraform_3](https://github.com/user-attachments/assets/d25f6ff8-eee1-4215-aec5-7d9e1d0c6b8e)

![Terraform_4](https://github.com/user-attachments/assets/646bdc1f-04e4-479d-ab88-2bd8f21f76f0)

![Terraform_5](https://github.com/user-attachments/assets/da3e4858-acfc-4c32-8e31-14234e3dc7b6)

---

### **2. Initialize Terraform in Azure Cloud Shell**
1. Verify Terraform is pre-installed by running:
   ```bash
   terraform --version
   ```
2. Create a working directory for your Terraform files:
   ```bash
   mkdir terraform-azure && cd terraform-azure
   ```

![20_Terraform_Resource_Creation_1](https://github.com/user-attachments/assets/321c03fc-4c80-4251-96b9-55671f71d330)

---

### **3. Create a Terraform Configuration File**
1. Create a file named `main.tf`:
   ```bash
   vi main.tf
   ```

![20_Terraform_Resource_Creation_2](https://github.com/user-attachments/assets/a4ef5716-f008-4ba1-a9de-42e19dd8d409)


2. Add the following configuration to create Azure resources:
   
   
```hcl
provider "azurerm" {
  features {}
  subscription_id = "024da94d-4ab2-429f-9b42-a588276a504b"
}

variable "resource_group_name" {
  default = "hub1_rg"
}

variable "location" {
  default = "East US"
}

variable "vnet_name" {
  default = "hub1_vnet"
}

variable "subnet_name" {
  default = "hub1_pub_subnet"
}

# Create Resource Group
resource "azurerm_resource_group" "rg" {
  name     = var.resource_group_name
  location = var.location
}

# Create Virtual Network
resource "azurerm_virtual_network" "vnet" {
  name                = var.vnet_name
  address_space       = ["172.1.0.0/24"]
  location            = var.location
  resource_group_name = azurerm_resource_group.rg.name
}

# Create Subnet
resource "azurerm_subnet" "subnet" {
  name                 = var.subnet_name
  resource_group_name  = azurerm_resource_group.rg.name
  virtual_network_name = azurerm_virtual_network.vnet.name
  address_prefixes     = ["172.1.0.0/27"]
}

# Public IP
resource "azurerm_public_ip" "vm_public_ip" {
  name                = "linux-vm1-public-ip"
  location            = var.location
  resource_group_name = azurerm_resource_group.rg.name
  allocation_method   = "Static"
  sku                 = "Basic"
}

# NIC
resource "azurerm_network_interface" "vm_nic" {
  name                = "linuxserver1-nic"
  location            = var.location
  resource_group_name = azurerm_resource_group.rg.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.subnet.id
    private_ip_address_allocation = "Dynamic"
    public_ip_address_id          = azurerm_public_ip.vm_public_ip.id
  }
}

# Linux VM
resource "azurerm_linux_virtual_machine" "vm" {
  name                            = "LinuxServer1"
  resource_group_name             = azurerm_resource_group.rg.name
  location                        = var.location
  size                            = "Standard_B1ls"
  admin_username                  = "azureuser"
  admin_password                  = "Testuser@2025"
  disable_password_authentication = false

  network_interface_ids = [azurerm_network_interface.vm_nic.id]

  os_disk {
    caching              = "ReadWrite"
    storage_account_type = "Standard_LRS"
  }

  source_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "18.04-LTS"
    version   = "latest"
  }
}
```


![20_Terraform_Resource_Creation_3](https://github.com/user-attachments/assets/8b90bc19-9e2f-4821-a74c-8df076d56855)


3. Get the subscription id by navigating to Subscriptions directory

![20_Terraform_Resource_Creation_4](https://github.com/user-attachments/assets/c31345d3-82fd-44a9-a7b9-8c817e927177)

   
4. Save and exit the editor.

---

### **4. Create a Terraform Output File**
1. Create a file named `output.tf`:
   ```bash
   vi output.tf
   ```

![20_Terraform_Resource_Creation_4_1](https://github.com/user-attachments/assets/fcd76c0e-22fd-412c-a92d-3c69b30d34d0)


2. Add the following in the output.tf file:
   
```hcl
output "resource_group_name" {
  value       = azurerm_resource_group.rg.name
  description = "The name of the resource group"
}

output "virtual_network_name" {
  value       = azurerm_virtual_network.vnet.name
  description = "The name of the virtual network"
}

output "public_subnet_name" {
  value       = azurerm_subnet.subnet.name
  description = "The name of the public subnet"
}

output "linux_vm_name" {
  value       = azurerm_linux_virtual_machine.vm.name
  description = "The name of the Linux virtual machine"
}

output "linux_vm_public_ip" {
  value       = azurerm_public_ip.vm_public_ip.ip_address
  description = "The public IP address of the Linux VM"
}

output "linux_vm_private_ip" {
  value       = azurerm_network_interface.vm_nic.ip_configuration[0].private_ip_address
  description = "The private IP address of the Linux VM"
}

output "linux_vm_nic_id" {
  value       = azurerm_network_interface.vm_nic.id
  description = "The ID of the network interface attached to the Linux VM"
}

```


---


### **5. Initialize Terraform**
1. Run the following command to initialize Terraform and download the Azure provider:
   ```bash
   terraform init
   ```

![20_Terraform_Resource_Creation_5](https://github.com/user-attachments/assets/21803c5a-2210-4116-8456-56b23853e903)

---

### **6. Plan the Deployment**
1. Generate an execution plan to preview the resources Terraform will create:
   ```bash
   terraform plan
   ```

![20_Terraform_Resource_Creation_6_1](https://github.com/user-attachments/assets/dd4d3ca9-e128-44d2-98ee-3bba2f5a35ab)

![20_Terraform_Resource_Creation_6_2](https://github.com/user-attachments/assets/2b100597-9a39-438f-8a25-0ba4983b7456)


---

### **7. Apply the Configuration**

1. Deploy the resources to Azure:
   ```bash
   terraform apply
   ```

2. Confirm the deployment by typing `yes` when prompted.

![20_Terraform_Resource_Creation_7_1](https://github.com/user-attachments/assets/dab05f96-606e-4ef8-8b9c-bd1c3c65792e)

![20_Terraform_Resource_Creation_7_2](https://github.com/user-attachments/assets/720bdd3c-be06-4578-8a26-604233395861)

---

### **8. Verify the Resources**

Check the created resources in the Azure Portal:
   
![20_Terraform_Resource_Creation_8_1](https://github.com/user-attachments/assets/48a23c5b-302d-4cc7-999d-eda0a77a12ba)

![20_Terraform_Resource_Creation_8_2](https://github.com/user-attachments/assets/7127fd4a-e072-4298-8def-d1a887c2b848)

![20_Terraform_Resource_Creation_8_3](https://github.com/user-attachments/assets/77d23dba-fbf2-4c7b-ba6c-41382cccd0c5)

---

### **9. Clean Up Resources**
1. To delete the resources created by Terraform, run:
   ```bash
   terraform destroy
   ```
2. Confirm the destruction by typing `yes` when prompted.

![20_Terraform_Resource_Creation_9_1](https://github.com/user-attachments/assets/3b383a28-e0e0-4f29-8fea-48532920e5f3)

![20_Terraform_Resource_Creation_9_2](https://github.com/user-attachments/assets/d854b326-0203-4f9f-8c61-f5e37e85e862)

---

### **Key Notes**
- **Azure Cloud Shell** comes pre-installed with Terraform, so no additional setup is required.
- Always use `terraform plan` to preview changes before applying them.
- Store your Terraform files in a version control system like **GitHub** for better management.

This step-by-step guide ensures you can quickly create and manage Azure resources using Terraform in Azure Cloud Shell.
