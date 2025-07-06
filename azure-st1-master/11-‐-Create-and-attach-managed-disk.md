# 💾 Create and Attach a Managed Disk to an Azure VM

This lab demonstrates how to **create a managed disk** and **attach it** to an existing **Azure Virtual Machine** using the **Azure Portal**.

---

## 🧱 Prerequisites

- An active Azure subscription
- An existing Azure Virtual Machine (VM)
- Appropriate permissions to create disks and modify VMs

---

## 🔹 Step 1: From BastionVM, SSH to PrivateVM. Now Navigate to “Disks” in the Private VM

1. Go to the [Azure Portal](https://portal.azure.com/)
2. Use the top search bar to search for **Disks**
3. Click **+ Add** to create a new managed disk

📷 *Refer to below screenshots*

![11_Create_Attach_Managed_Disk_1](https://github.com/user-attachments/assets/e569efe2-c0d8-4e43-a50d-d03fbe43462b)

![11_Create_Attach_Managed_Disk_2](https://github.com/user-attachments/assets/cb2df1bc-fec3-4344-9884-8b3d07b4be4d)

![11_Create_Attach_Managed_Disk_3](https://github.com/user-attachments/assets/bd53339a-eafb-4e33-889a-bd458f031f47)

![AppserverVM_creation_3](https://github.com/user-attachments/assets/ba57292b-8438-47d6-a4ae-5f53483cdb27)

---

## 🔹 Step 2: Create a New Managed Disk

Fill in the disk creation details:

- **Subscription** and **Resource Group**: Match the VM’s
- **Disk Name**: e.g., `DataDisk1`
- **Region**: Same as the VM’s region
- **Availability Zone**: Optional; align with the VM if needed
- **Size and Performance**: Choose a disk type (Standard HDD, Standard SSD, Premium SSD) and size (e.g., 128 GiB)

Click **Review + Create**, then **C**

![AppserverVM_creation_4](https://github.com/user-attachments/assets/c0ae43c6-e2c1-4bcc-9a62-f19ba4afbdfa)

![AppserverVM_creation_5](https://github.com/user-attachments/assets/4070b91e-93ef-4a69-a9a0-a620825458a1)

![AppserverVM_creation_6](https://github.com/user-attachments/assets/c0f2ab66-30ed-4b9d-bd62-4aa54835403d)

---

**Run lsblk command to check the existing volumes in the server.**

`lsblk`

![AppserverVM_creation_7](https://github.com/user-attachments/assets/f4e0df05-1553-40b1-bd71-7406d4f79482)

---

**Formatting the Disk**

Run “fdisk /dev/sdb” command 

`sudo fdisk /dev/sdb`

2. Type “n” to create a new partition. 
---

**Create a Physical Volume**

1. Run "pvcreate /dev/sdb1" to create the physical volume

`sudo pvcreate /dev/sdb1`

2. Run "pvdisplay /dev/sdb1" to display the physical volume created

`sudo pvdisplay /dev/sdb1`


![AppserverVM_creation_8](https://github.com/user-attachments/assets/e2ef46a3-0436-433c-89f5-41eb0739a49e)

![AppserverVM_creation_9](https://github.com/user-attachments/assets/26cff39b-152b-4306-a4a4-307cea0dd416)
---


**Create a Volume Group**

1. Run "vgcreate vg_u01 /dev/sdb1" to create the volume group

```
sudo vgcreate vg_u01 /dev/sdb1
```

2. Run “vgdisplay vg_u01” to display the volume group created and to see the Number of Physical Extent.

`sudo vgdisplay vg_u01`

![AppserverVM_creation_10](https://github.com/user-attachments/assets/1c0a77b6-dd99-445d-89f8-03881dc1e51c)

![AppserverVM_creation_11](https://github.com/user-attachments/assets/f07b8363-014f-4fc6-bcd8-a6c64ae4c444)
---


**Create a logical volume and map it to our volume group**

1. Run the below commands to create the logical volume

`sudo lvcreate -l 12799 -n lv_u01 vg_u01`

`ls -l /dev/mapper/vg_u01-lv_u01`


2. Logical volume is now created inside our volume group


![AppserverVM_creation_12](https://github.com/user-attachments/assets/a84ed9b7-3e2e-457b-a95b-6c4847a5b4c5)

![AppserverVM_creation_13](https://github.com/user-attachments/assets/6a0a3bfa-325a-44ef-a406-f8712c19f47d)
---


**Format this logical volume using a file system**

`sudo mkfs.ext4 /dev/mapper/vg_u01-lv_u01`

![AppserverVM_creation_14](https://github.com/user-attachments/assets/0c1bf296-69b7-46cd-99b8-0d18ddbb5882)
---


**Create a Directory to mount the logical volume**

1. Run the below commands to mount the logical volume

`mkdir /u01`

`sudo mount -t ext4  /dev/mapper/vg_u01-lv_u01  /u01`

2. Now we are able to see that the logical volume is mounted.

![AppserverVM_creation_15](https://github.com/user-attachments/assets/bf738d24-dcd7-48f9-a2b4-ca8240258049)

![AppserverVM_creation_16](https://github.com/user-attachments/assets/5b4edd93-ee20-4a4b-8244-82df6b012310)

![AppserverVM_creation_17](https://github.com/user-attachments/assets/58da4b22-80cc-41a4-8c11-861235f924f5)
---


**Mount the volume permanently in /etc/fstab or else it will get removed after the server reboots.**

1. Add the line below in /etc/fstab

`sudo vi /etc/fstab`

`/dev/mapper/vg_u01-lv_u01  /u01  ext4  defaults,_netdev,nofail 0 2`


2. Run the command "systemctl daemon-reload" to mount this volume premanently

`mount -a`

![AppserverVM_creation_18](https://github.com/user-attachments/assets/2ea19c56-86d0-4eed-a65e-efe266cb844e)

![AppserverVM_creation_19](https://github.com/user-attachments/assets/66e7bea5-a3f1-4459-9869-5ad84c81382a)

![AppserverVM_creation_20](https://github.com/user-attachments/assets/fcd29aee-b356-457c-ad94-9a2172c2a7e9)
---


Restart the VM and run "lsblk" and "df -h" command to check the new partition

![AppserverVM_creation_21](https://github.com/user-attachments/assets/e7bff75d-c661-4f0b-91f2-33c1a91c2b93)

![AppserverVM_creation_22](https://github.com/user-attachments/assets/539afaa1-5a3a-427c-9c40-76ba621e28ab)

![AppserverVM_creation_23](https://github.com/user-attachments/assets/52c5b988-40fa-4721-8f03-3b9715008aab)
