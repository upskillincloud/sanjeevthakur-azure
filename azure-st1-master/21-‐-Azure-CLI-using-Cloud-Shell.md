### **Azure CLI**

---

### **Introduction**
The **Azure Command-Line Interface (CLI)** is a powerful tool that allows developers, administrators, and cloud architects to manage Azure resources directly from the command line. It provides a simple and efficient way to automate tasks, deploy resources, and interact with Azure services without using the Azure Portal.

In this article, we’ll explore the basics of Azure CLI, its key features, and provide step-by-step instructions to practice it manually.

---

### **Why Use Azure CLI?**
1. **Efficiency**: Perform tasks faster compared to navigating through the Azure Portal.
2. **Automation**: Automate repetitive tasks using scripts.
3. **Cross-Platform**: Available on Windows, macOS, and Linux.
4. **Integration**: Easily integrates with CI/CD pipelines and other automation tools.
5. **Scripting**: Supports scripting in Bash, PowerShell, and other shells.

---

### **Installing Azure CLI**
Azure CLI can be installed on various platforms. Below are the steps for installation:

#### **1. Windows**
- Download the installer from the [Azure CLI installation page](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows).
- Run the installer and follow the instructions.
- Verify the installation:
  ```bash
  az --version
  ```

#### **2. macOS**
- Install using Homebrew:
  ```bash
  brew update && brew install azure-cli
  ```
- Verify the installation:
  ```bash
  az --version
  ```

#### **3. Linux**
- Install using the package manager:
  ```bash
  curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
  ```
- Verify the installation:
  ```bash
  az --version
  ```

#### **4. Docker**
- Run Azure CLI in a Docker container:
  ```bash
  docker run -it mcr.microsoft.com/azure-cli
  ```

---

### **Getting Started with Azure CLI**

#### **1. Login to Azure**
To start using Azure CLI, log in to your Azure account:
```bash
az login
```
This will open a browser window for authentication. After logging in, the CLI will display your active subscriptions.

#### **2. Set a Default Subscription**
If you have multiple subscriptions, set a default subscription:
```bash
az account set --subscription "<subscription-id>"
```

---

### **Common Azure CLI Commands**

#### **1. Managing Resource Groups**
- **Create a Resource Group**:
  ```bash
  az group create --name MyResourceGroup --location eastus
  ```
- **List Resource Groups**:
  ```bash
  az group list --output table
  ```
- **Delete a Resource Group**:
  ```bash
  az group delete --name MyResourceGroup --yes
  ```

#### **2. Managing Virtual Machines**
- **Create a Virtual Machine**:
  ```bash
  az vm create \
    --resource-group MyResourceGroup \
    --name MyVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
  ```
- **Start a Virtual Machine**:
  ```bash
  az vm start --resource-group MyResourceGroup --name MyVM
  ```
- **Stop a Virtual Machine**:
  ```bash
  az vm stop --resource-group MyResourceGroup --name MyVM
  ```
- **Delete a Virtual Machine**:
  ```bash
  az vm delete --resource-group MyResourceGroup --name MyVM --yes
  ```

#### **3. Managing Storage Accounts**
- **Create a Storage Account**:
  ```bash
  az storage account create \
    --name mystorageaccount \
    --resource-group MyResourceGroup \
    --location eastus \
    --sku Standard_LRS
  ```
- **List Storage Accounts**:
  ```bash
  az storage account list --output table
  ```

#### **4. Managing Azure Kubernetes Service (AKS)**
- **Create an AKS Cluster**:
  ```bash
  az aks create \
    --resource-group MyResourceGroup \
    --name MyAKSCluster \
    --node-count 2 \
    --enable-addons monitoring \
    --generate-ssh-keys
  ```
- **Get AKS Credentials**:
  ```bash
  az aks get-credentials --resource-group MyResourceGroup --name MyAKSCluster
  ```
- **Delete an AKS Cluster**:
  ```bash
  az aks delete --resource-group MyResourceGroup --name MyAKSCluster --yes
  ```

---

### **Steps to access Azure CLI using Cloud Shell**

#### **Step 1: Open Cloud Shell in Azure**

![Azure_CLI_1](https://github.com/user-attachments/assets/d293c767-db26-444b-978f-ee3a1b059002)

![Azure_CLI_2](https://github.com/user-attachments/assets/caf7383a-7c11-4fbf-badc-d549c15d6ed2)

![Azure_CLI_3](https://github.com/user-attachments/assets/493b0e9f-b806-472e-8be5-3c2c12ad1698)

![Azure_CLI_4](https://github.com/user-attachments/assets/c229305d-f41a-4de0-b72c-84a3d47c092d)

![Azure_CLI_5](https://github.com/user-attachments/assets/c26b3199-c638-4b09-8f15-8720aa1f7e14)


#### **Step 2: Login to Azure**
- Use `az login` to authenticate with your Azure account.
- Use `az --version` to display the current version.

![Azure_CLI_6](https://github.com/user-attachments/assets/938c5b4a-1d2d-47b9-9ce9-662cb42bc40a)

![Azure_CLI_7](https://github.com/user-attachments/assets/417be9ed-ca1c-41e5-ac77-1fdc0ab51401)

#### **Step 3: Create a Resource Group**
- Run the following command to create a resource group:
  ```bash
  az group create --name AppGateway_RG --location eastus
  ```
![Azure_CLI_8](https://github.com/user-attachments/assets/f5b2ab95-4487-4674-9c2c-7e64a06ac863)

![Azure_CLI_9](https://github.com/user-attachments/assets/e5774c0e-54c8-4a65-bc0b-2ab5902fd239)


#### **Step 4: Delete Resources**
- Delete the resource group:
  ```bash
  az group delete --name AppGateway_RG --yes
  ```
![Azure_CLI_10](https://github.com/user-attachments/assets/f3a6eda4-5624-4695-a7b7-26a1559487e7)

![Azure_CLI_11](https://github.com/user-attachments/assets/5a997e77-9466-4515-9aa3-766250ad7f7f)

---

### **Best Practices for Using Azure CLI**
1. **Use Automation**: Combine Azure CLI commands with shell scripts for automation.
2. **Use Resource Tags**: Tag resources during creation for better organization and cost tracking.
3. **Leverage Output Formats**: Use `--output` options like `table`, `json`, or `yaml` for better readability.
4. **Secure Credentials**: Avoid hardcoding sensitive information in scripts; use environment variables or Azure Key Vault.

---

### **Conclusion**
Azure CLI is a versatile and powerful tool for managing Azure resources efficiently. By practicing the commands and steps outlined in this article, you can gain hands-on experience and streamline your cloud operations. Whether you're a beginner or an experienced cloud professional, mastering Azure CLI is an essential skill for working with Microsoft Azure.

---

### **Next Steps**
- Explore advanced Azure CLI commands for networking, databases, and serverless services.
- Integrate Azure CLI with CI/CD pipelines using tools like Azure DevOps or GitHub Actions.
- Experiment with scripting in Bash or PowerShell to automate complex workflows.