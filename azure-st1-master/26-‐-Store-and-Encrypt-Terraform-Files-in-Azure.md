### **Steps to Store Terraform Files in Azure Storage Encrypted with Your Own Customer-Managed Key (CMK) via Azure Portal**

---

#### **1. Create a Resource Group - tf-rg**

![26_Store and Encrypt Terraform Files in Azure_1](https://github.com/user-attachments/assets/a1c68b8e-818f-4419-86b6-1910cc058cd3)

---

#### **2. Create a Key Vault and Generate a Key (CMK)**
- Go to **Key Vaults** and click **Create**.
- Fill in the details (resource group, name, region).
- Click **Review + create** and **Create**.
- After deployment, open the Key Vault.
- Go to **Keys** > **Generate/Import**.
- Choose **Generate**, enter a name, and click **Create**.

![26_Store and Encrypt Terraform Files in Azure_7](https://github.com/user-attachments/assets/1afe7258-f1bc-4db5-8ccc-94a5f8eba965)

![26_Store and Encrypt Terraform Files in Azure_8](https://github.com/user-attachments/assets/7077598e-16eb-4a3c-8422-e0ef04742819)

![26_Store and Encrypt Terraform Files in Azure_9](https://github.com/user-attachments/assets/8939f5e4-f304-456a-ab74-79c61d0266f2)

**Create a Managed Identity and assign "Key Vault Crypto Service Encryption User role". This allows Azure Storage Account to use a Customer Managed Key (CMK) stored in Azure Key Vault for encryption.**

![26_Store and Encrypt Terraform Files in Azure_10](https://github.com/user-attachments/assets/02482698-276f-4e7f-8814-8697566c2896)

![26_Store and Encrypt Terraform Files in Azure_11](https://github.com/user-attachments/assets/ac8e1704-38d1-4472-931a-80030a0d4769)

---

#### **3. Create a Storage Account with Encryption Using Your CMK**
- Go to **Storage accounts** and click **Create**.
- Fill in the details (resource group, name, region).
- Under **Advanced** > **Encryption**, select **Customer-managed keys**.
- Choose **Select from Key Vault**.
- Select your Key Vault and the key you created.
- Complete the wizard and click **Review + create** and **Create**.

![26_Store and Encrypt Terraform Files in Azure_2](https://github.com/user-attachments/assets/c102e0c2-8121-4dbc-91d3-d5ff9b3141b7)

![26_Store and Encrypt Terraform Files in Azure_3](https://github.com/user-attachments/assets/6e603160-5218-4544-9a6d-06f02f342327)

![26_Store and Encrypt Terraform Files in Azure_4](https://github.com/user-attachments/assets/8758dd2f-06e5-4ef7-8969-a77e29d1bc5e)

![26_Store and Encrypt Terraform Files in Azure_5](https://github.com/user-attachments/assets/4a6666c5-da52-4438-ac4e-2ee690889c8d)

![26_Store and Encrypt Terraform Files in Azure_6](https://github.com/user-attachments/assets/1669a642-ef39-491e-958f-1783a78ea758)

![26_Store and Encrypt Terraform Files in Azure_12](https://github.com/user-attachments/assets/a1d9b9c5-5f26-4c52-b44a-760f3e223913)

![26_Store and Encrypt Terraform Files in Azure_13](https://github.com/user-attachments/assets/69943075-aa40-49e5-a5a0-2b72de241e14)

![26_Store and Encrypt Terraform Files in Azure_14](https://github.com/user-attachments/assets/3a076004-11ac-46e4-ac27-84a37c4928d1)

---

#### **4. Create a Blob Container for Terraform Files**
- Open your storage account.
- Go to **Containers** under **Data storage**.
- Click **+ Container**, enter a name (e.g., `terraform`), and set the access level to **Private**.
- Click **Create**.

![26_Store and Encrypt Terraform Files in Azure_15](https://github.com/user-attachments/assets/70ddc151-d1b7-4492-8fc5-42a6dee23a50)

![26_Store and Encrypt Terraform Files in Azure_16](https://github.com/user-attachments/assets/4806db6d-a3c4-4b86-81d9-c0d37e691b32)

![26_Store and Encrypt Terraform Files in Azure_17](https://github.com/user-attachments/assets/31db5b66-d980-42d9-a123-f56637d8055e)

---

#### **5. Assign Permissions to Storage Account for Key Vault Access**
- In the Key Vault, go to **Access policies** > **+ Add Access Policy**.
- Select **Key permissions**: Get, Wrap Key, Unwrap Key.
- Under **Principal**, search for and select **Microsoft Storage** (or the storage account’s managed identity if using one).
- Click **Add**, then **Save**.

![26_Store and Encrypt Terraform Files in Azure_18](https://github.com/user-attachments/assets/c37911c9-66b6-49e0-b28f-cc8362eb0136)

**We are getting this error because the Azure Storage Account is currently set to restrict access through its network rules (firewall settings), and our IP address(49.43.27.252) is not whitelisted.**

To fix this error

- Go to the **Storage Account** in Azure Portal.
- Select **Networking → Firewalls and virtual networks**.
- Under **Firewall**, click **+ Add your client IP address**.
- Click **Save**.

![26_Store and Encrypt Terraform Files in Azure_19](https://github.com/user-attachments/assets/ba3dcb8d-8c7b-459c-8ab8-e0e683a24f35)

![26_Store and Encrypt Terraform Files in Azure_20](https://github.com/user-attachments/assets/7b3e924d-41eb-4b21-b53f-2933526df099)

---

#### **6. Upload Terraform Files**
- Open the container you just created.
- Click **Upload**.
- Select your Terraform files (`.tf`, `.tfvars`, etc.) and click **Upload**.

![26_Store and Encrypt Terraform Files in Azure_21](https://github.com/user-attachments/assets/de233302-337c-4638-83ba-3f3f9ef7cc1d)

![26_Store and Encrypt Terraform Files in Azure_22](https://github.com/user-attachments/assets/6e9ecca0-8bcb-4bf9-adb2-6502ba85be96)

---

### **Summary Diagram**

```plaintext
+-------------------+      +-------------------+      +-------------------+
|   Key Vault       |----->| Storage Account   |----->|   Blob Container  |
| (CMK generated)   |      | (CMK encryption)  |      | (Terraform files) |
+-------------------+      +-------------------+      +-------------------+
```

---

**Result:**  
Your Terraform files are now stored in an Azure Storage Account, encrypted at rest with your own customer-managed key (CMK) from Azure Key Vault.
