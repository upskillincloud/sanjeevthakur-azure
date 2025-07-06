**step-by-step guide to send data from an Azure VM to Azure Blob Storage** (an alternative to using Storage Gateway/SGW):

---

### **1. Create an Azure Storage Account and Blob Container**

1. **Go to Azure Portal** > **Storage accounts** > **Create**.
2. Fill in the required details (resource group, name, region, etc.).

![30_Send-data-to-blob-storage-from-instance_1](https://github.com/user-attachments/assets/b9016c21-5e63-497e-bca8-6e0cb19e9e85)

3. Once created, open the storage account.
4. Under **Data storage**, select **Containers**.
5. Click **+ Container**, give it a name (e.g., `mycontainer`), and set access level (private is recommended).

![30_Send-data-to-blob-storage-from-instance_2](https://github.com/user-attachments/assets/99a7ca5a-d059-4716-a87c-e57be698b3a4)
![30_Send-data-to-blob-storage-from-instance_3](https://github.com/user-attachments/assets/f9da81f3-dd86-4d8f-ad0c-add77d458617)
![30_Send-data-to-blob-storage-from-instance_4](https://github.com/user-attachments/assets/759eedac-88f0-42ac-a57e-32c6e2e2d113)

6. Create upload file in VM

![30_Send-data-to-blob-storage-from-instance_6](https://github.com/user-attachments/assets/aae104e0-7a49-4d42-94e0-d1c0b17a79bf)

---

### **2. Get Storage Account Access Key or SAS Token**

1. In the storage account, go to **Access keys** (for key) or **Shared access signature** (for SAS).
2. Copy the **key** or generate and copy a **SAS token**.

![SAS_Key_1](https://github.com/user-attachments/assets/35ac235a-b19e-4065-9936-7546eb8acac0)
![SAS_Key_2](https://github.com/user-attachments/assets/f55339d9-b363-4afc-be0c-13a6928f9d96)


---

### **3. Install AzCopy on the VM**

**Option A: Using AzCopy (Recommended for file transfers)**
- Download and install AzCopy on your VM:  
  [AzCopy Download](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10#download-azcopy)
- Extract and add AzCopy to your system PATH.

![30_Send-data-to-blob-storage-from-instance_7](https://github.com/user-attachments/assets/70086775-6318-4915-ab8b-474d9091d01b)

---

### **4. Upload Data from VM to Blob Storage**

#### **Using AzCopy**

**Command Example (using Access Key):**
```bash
azcopy copy "/path/to/local/file.txt" "https://<storageaccount>.blob.core.windows.net/<container>/<blobname>?<SAS_token>" --recursive=false
```
- Replace `<storageaccount>`, `<container>`, `<blobname>`, and `<SAS_token>` with your values.

```bash
azcopy copy "/root/storageupload/upload.txt" "https://testvmtostorage.blob.core.windows.net/mycontainer/testblob?sv=2024-11-04&ss=bfqt&srt=sco&sp=rwdlacupiytfx&se=2025-05-24T18:40:48Z&st=2025-05-24T10:40:48Z&spr=https&sig=%2FV8lW9F%2FrtdE3VIWaBVy791226OCjJNmPZPn1xROTUc%3D"
```

![SAS_Key_3](https://github.com/user-attachments/assets/2ded04fe-6d93-4ad6-94c2-2962c50eda9d)

---

### **5. Verify Upload**

- Go to the Azure Portal > Storage Account > Container.
- Confirm your file appears in the blob container.

![SAS_Key_4](https://github.com/user-attachments/assets/051d5076-0e89-4487-b2b1-7c10811a2723)

---

### **Summary Table**

| Step | Action                                   | Tool/Command Example                                                                 |
|------|------------------------------------------|--------------------------------------------------------------------------------------|
| 1    | Create Storage Account & Container       | Azure Portal                                                                         |
| 2    | Get Access Key or SAS Token              | Azure Portal                                                                         |
| 3    | Install AzCopy or Azure CLI on VM        | Download from Microsoft                                                              |
| 4    | Upload Data                              | `azcopy copy ...` or `az storage blob upload ...`                                    |
| 5    | Verify Upload                            | Azure Portal                                                                         |

---

**This method is a direct, secure, and efficient alternative to Storage Gateway for sending data from a VM to Azure Blob Storage.**

