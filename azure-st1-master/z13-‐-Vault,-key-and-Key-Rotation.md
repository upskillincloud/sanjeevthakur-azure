### Creating a Key Vault
---

From the left-hand menu, select “All Services” then in the “Security” section , select Key Vault and click on Create.

![13_Vault_Key_and_Key_Rotation_1](https://github.com/user-attachments/assets/e1fdcb83-13f3-485d-b6ab-085ab63d488e)


Fill in the required details and click on “Review+Create” and Create.

![13_Vault_Key_and_Key_Rotation_2](https://github.com/user-attachments/assets/f59f9fa9-4d0e-4b72-bb27-efdfdf3be0ec)


We can now see the Key Vault is created,

![13_Vault_Key_and_Key_Rotation_3](https://github.com/user-attachments/assets/021b9be1-8b0d-495c-92d3-d7070e6abdd9)



### Create a Key in the Key Vault
---

In the Azure Portal, Go to “Key Vaults” and select the Key Vault.

In the Key Vault settings, select “Keys” and click on “+Generate/Import”.

![13_Vault_Key_and_Key_Rotation_4](https://github.com/user-attachments/assets/7c4c7ecd-063b-4dbe-80a2-047f27e1694c)

**The error message "The operation is not allowed by RBAC. If role assignments were recently changed, please wait several minutes for role assignments to become effective." indicates that our Azure user/account does not have the required RBAC (Role-Based Access Control) permissions to perform the action (e.g., generate/import keys) in the Azure Key Vault.**

To fix this, 

1. Navigate to Key Vault --> Access Configuration --> Check if it's using Azure RBAC or Vault access policy.

![Vault_Key_and_Key_Rotation_Error_Resolution_1](https://github.com/user-attachments/assets/4ea42ea9-c3a3-497e-b266-43162f9a3be4)


2. As Azure RBAC is being used, we need to assign Key Vault Administrator (Full access) or Key Vault Crypto Officer (for keys only). **we will go with Key Vault Administrator role.**

3. To assign the role, Navigate to Key Vault --> Access control (IAM) --> Click + Add --> Add role assignment --> Select "Key Vault Administrator" --> Assign to your user account, group, or service principal --> Click Save.

![Vault_Key_and_Key_Rotation_Error_Resolution_2](https://github.com/user-attachments/assets/d3025bfe-5f15-4280-9e90-42721a5e998e)

![Vault_Key_and_Key_Rotation_Error_Resolution_7](https://github.com/user-attachments/assets/9b690f17-478b-4921-ab46-764e03e1a71f)


4. RBAC assignments can take 5–10 minutes to take effect. Wait a few minutes before retrying.

5. After few minutes, Navigate to Key Vault settings, select “Keys” and click on “+Generate/Import”. **We do not see the error message now.**

6. Fill the required details and click on “Create”.

![Vault_Key_and_Key_Rotation_Error_Resolution_8](https://github.com/user-attachments/assets/31be0914-4c64-465a-8e02-518f3f38846b)

![Vault_Key_and_Key_Rotation_Error_Resolution_9](https://github.com/user-attachments/assets/74114217-2ee8-4491-84c6-d9ebbe4c7e34)


We can see the key is created.

![Vault_Key_and_Key_Rotation_Error_Resolution_10](https://github.com/user-attachments/assets/386daca9-fff7-47e8-b2b0-db73a18bfdb9)



## Rotate the Key
---

In the Key Vault settings, select “Keys”, and select the key which we want to rotate. Click on “New Version”.

![13_Vault_Key_and_Key_Rotation_6](https://github.com/user-attachments/assets/c7360c1f-073e-451f-a21c-33cecfafa7d7)


Fill the required details and click on “Create”.

![13_Vault_Key_and_Key_Rotation_7](https://github.com/user-attachments/assets/cf99a575-ec19-4d3e-b1af-ca038067d70c)


We can now see the new key is created,

![13_Vault_Key_and_Key_Rotation_8](https://github.com/user-attachments/assets/12ddfa56-72bc-4b69-b81b-13f9d1ab6375)


### Automate Key Rotation
---

We also have an option to automate this key rotation.

For this we have to go into key we want to rotate and click on “Rotation Policy”.

![13_Vault_Key_and_Key_Rotation_9](https://github.com/user-attachments/assets/b6244b25-17c7-4e41-987c-4b660d5e8207)


Select the rotation policy and “Enable” auto rotation.

![13_Vault_Key_and_Key_Rotation_10](https://github.com/user-attachments/assets/a088a653-0d19-447b-927e-53b921a23090)


Click on Save and then the key will be automatically rotated as per the rotation policy.

![13_Vault_Key_and_Key_Rotation_11](https://github.com/user-attachments/assets/8b4194a0-75e9-421a-9d67-4d6585086ca3)