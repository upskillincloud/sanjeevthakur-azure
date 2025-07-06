# 🌐 Internet Gateway in Azure

This lab demonstrates how to control outbound internet access from an Azure Virtual Machine (VM) by configuring Network Security Group (NSG) rules. Specifically, it shows how to restrict access to only specific IP addresses, such as a particular Google IP.

---

## 🧱 Scenario: Restricting VM's Internet Access to a Specific IP

By default, when a VM is created in a public subnet with an associated public IP, it has unrestricted outbound internet access. To limit this access to a specific IP address, follow the steps below.

---

### 🔹 Step 1: Review Default Network Settings

- **Virtual Network (VNet)**: A VNet is created with a public subnet.
- **Virtual Machine (VM)**: A VM is deployed within this subnet.
- **Network Settings**: By default, the VM can access the internet without restrictions.

📷 *Refer to Screenshot 1: VNet Configuration*

![17_Internet_Gateway_Azure_1](https://github.com/user-attachments/assets/f9a3ea1a-96be-4baa-89d7-46d4ce7ef25e)


📷 *Refer to Screenshot 2: VM Deployment*

![17_Internet_Gateway_Azure_2](https://github.com/user-attachments/assets/d23bb847-3edc-4f0c-987f-05d903da88ca)


📷 *Refer to Screenshot 3: Default Network Settings*

![17_Internet_Gateway_Azure_3](https://github.com/user-attachments/assets/0ee7a214-b082-41e5-8db8-4f7f6a59887b)

---

### 🔹 Step 2: Test Internet Connectivity

- Open a terminal on the VM.
- Ping `google.com` to verify internet connectivity.

```bash
ping google.com
```

📷 Refer to Screenshot 4: Successful Ping to google.com
![17_Internet_Gateway_Azure_4](https://github.com/user-attachments/assets/6ffc3bf0-7475-43c7-b42e-3ff62b1e6836)


Now if we want to allow our VM to access any specific IP of google, we have to overwrite this default route, for that we have to add below port rules in the VM’s Network Settings

1. Firstly we have to Deny Any Default Outbound rule, which will overwrite the default route

![17_Internet_Gateway_Azure_5](https://github.com/user-attachments/assets/727130c5-6e45-436b-9f19-72a9019649a7)

After adding this rule, we can see that now we are not able to ping google.com

![17_Internet_Gateway_Azure_6](https://github.com/user-attachments/assets/be79813a-3d65-4e71-8e79-6b574662956c)


2. Now we have to allow this specific IP of google from which we want the access to internet, add below rule.

![17_Internet_Gateway_Azure_7](https://github.com/user-attachments/assets/5d6459ea-9922-428e-9b16-0ac9f92b8982)


We have added the specific IP of google into the rule and now we can see we able to ping that specific IP

![17_Internet_Gateway_Azure_8](https://github.com/user-attachments/assets/f321d9ed-0631-4e37-a73c-17cdd55562c2)


## Scenario 2: Reaching to VM from Personal Laptop Only

For connecting to the VM from only our personal laptop , then we have to add a rule and give the source to our IP only.

Now we can only connect to VM through our personal laptop only.

![17_Internet_Gateway_Azure_9](https://github.com/user-attachments/assets/eb5830c8-9c87-4cae-bb88-23f9c093ff1b)







