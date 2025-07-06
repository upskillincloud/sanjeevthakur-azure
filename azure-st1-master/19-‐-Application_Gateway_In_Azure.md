# 🌐 Application Gateway in Azure

This lab demonstrates the steps to set up an **Azure Application Gateway**, including the creation of necessary resources and configuration of a backend web server.

---

## 🧱 Prerequisites

- An active Azure subscription
- Basic understanding of Azure networking components

---

## 🔹 Step 1: Create a Resource Group

1. Navigate to the [Azure Portal](https://portal.azure.com/).
2. Search for **Resource groups** and click **+ Create**.
3. Fill in the required details:
   - **Subscription**: Select your subscription.
   - **Resource Group**: Enter a name (e.g., `AppGatewayRG`).
   - **Region**: Choose a region (e.g., `East US`).
4. Click **Review + Create**, then **Create**.

📷 *Refer to Screenshot: ![18_Application_Gateway_Azure_1](https://github.com/user-attachments/assets/b444a87b-b4ec-4652-8c4b-d3abf12500df)



---

## 🔹 Step 2: Create a Virtual Network and Subnets

1. In the Azure Portal, search for **Virtual networks** and click **+ Create**.
2. Fill in the required details:
   - **Name**: Enter a name (e.g., `AppGatewayVNet`).
   - **Address space**: Define the address space (e.g., `10.0.0.0/16`).
3. Under **Subnets**, create two subnets:
   - **AppGatewaySubnet**: e.g., `10.0.1.0/24`
   - **BackendSubnet**: e.g., `10.0.2.0/24`
4. Click **Review + Create**, then **Create**.

📷 *Refer to Screenshot: ![18_Application_Gateway_Azure_2](https://github.com/user-attachments/assets/8b7ab1ac-ae52-4c8c-90e2-c2d40f6c148e)



---

## 🔹 Step 3: Create a Virtual Machine

1. Navigate to **Virtual machines** and click **+ Create**.
2. Fill in the required details:
   - **Name**: Enter a name (e.g., `BackendVM`).
   - **Region**: Same as the Resource Group.
   - **Image**: Choose a Linux distribution (e.g., `CentOS`).
   - **Size**: Select an appropriate size.
   - **Authentication type**: Choose SSH public key or password.
   - **Inbound port rules**: Allow **HTTP (80)** and **SSH (22)**.
3. Under **Networking**, place the VM in the **BackendSubnet** of `AppGatewayVNet`.
4. Click **Review + Create**, then **Create**.

📷 *Refer to Screenshot: ![19_Application_Gateway_Azure_3](https://github.com/user-attachments/assets/560a1273-2bf7-4f35-acd9-f24a32f06140)


## After creating the VM, we need to follow the below steps :

1. Login to the VM and sudo to root.
2. Install and Enable httpd service in VM.
3. Create an index file (/var/www/html/index.html)
4. Enable firewalld service in VM.
5. Add port 80 in firewall.

![19_Application_Gateway_Azure_4](https://github.com/user-attachments/assets/3cc3ed10-e50e-40bb-994e-33212ef39541)

![19_Application_Gateway_Azure_5](https://github.com/user-attachments/assets/a0dc1280-605c-49dc-8411-134b27a55c96)

![19_Application_Gateway_Azure_6](https://github.com/user-attachments/assets/43160e2a-9f33-4661-aeec-d4f1cd682e1f)

![19_Application_Gateway_Azure_7](https://github.com/user-attachments/assets/2c41fa2d-f6c6-48e5-ae2d-3c73c3a21aba)

![19_Application_Gateway_Azure_8](https://github.com/user-attachments/assets/17418161-0a98-4385-99d2-c4ed3483ded4)

![19_Application_Gateway_Azure_9](https://github.com/user-attachments/assets/21cdb3db-c2c0-4092-aef1-34c340406f44)

![19_Application_Gateway_Azure_10](https://github.com/user-attachments/assets/42569623-49e5-4d4f-9c54-bba9e86c7aa6)

![19_Application_Gateway_Azure_11](https://github.com/user-attachments/assets/4fd5105d-74a0-4a08-9cba-7f7e9a57be82)

![19_Application_Gateway_Azure_12](https://github.com/user-attachments/assets/4eb5d831-d2ac-4280-9ae6-845dd4dc4d9b)

![19_Application_Gateway_Azure_13](https://github.com/user-attachments/assets/bbeac3be-2912-4489-ac88-3a7d5a493c3e)


## Ensure that index.html is accessible on web

![19_Application_Gateway_Azure_14](https://github.com/user-attachments/assets/19d27876-30c8-4268-b63c-43b318ddf2f8)


## Create a Public IP Address
Create a Public IP for the Application Gateway.

![19_Application_Gateway_Azure_15](https://github.com/user-attachments/assets/eee9e238-eab0-43df-a8df-f350d1b74188)



## Create Application Gateway
Now we have have to create the Application Gateway , Fill the Basic Part.

![19_Application_Gateway_Azure_16](https://github.com/user-attachments/assets/ebeadc63-5a85-4ea4-bbea-c99d5e78beb6)


In the Frontends section, select the Public IP

![19_Application_Gateway_Azure_17](https://github.com/user-attachments/assets/b94a18dd-bc1c-4822-845d-20137f349b6a)


In the Backends section, Click on Add a Backend Pool

![19_Application_Gateway_Azure_18](https://github.com/user-attachments/assets/d300fe69-a960-4fe5-b97f-9da9fbee90c9)


Fill the below details and click on Add

![19_Application_Gateway_Azure_19](https://github.com/user-attachments/assets/ee5cca9c-190d-47ac-901c-de3fe7a5ad61)


Now we can see the backend pool is Added.

![19_Application_Gateway_Azure_20](https://github.com/user-attachments/assets/d6a010e4-333e-4fa8-9f2e-64480d2a0b39)


Click on Next, In the Configuration section, we will connect the frontends and backend pools

![19_Application_Gateway_Azure_21](https://github.com/user-attachments/assets/3be569b4-bf11-4240-9359-2376227e94f3)

Click on Add a routing role, Now In Listener section, fill the below details ,

![19_Application_Gateway_Azure_22](https://github.com/user-attachments/assets/9095c8ae-1d19-41dc-8b0a-50a4942de33c)


And in Backend Targets section, select the Backend Target and for Backend Settings, click on Add new

![19_Application_Gateway_Azure_23](https://github.com/user-attachments/assets/42fcadf4-433d-4168-814f-e8d3fa4d52a9)


Fill the below details in Backend settings,

![19_Application_Gateway_Azure_24](https://github.com/user-attachments/assets/fcd68395-5ce6-47f1-b889-7bf9db8bd45b)


Now we have added both the Listener and Backend Targets, Click on Add.

![19_Application_Gateway_Azure_25](https://github.com/user-attachments/assets/8425cbba-16c2-47c7-aa96-c7d416e9c546)


Now the Routing rule is added.

![19_Application_Gateway_Azure_26](https://github.com/user-attachments/assets/960de451-d95e-42c6-b3e2-300ba5d78548)

Click on Next , Review and Create.

![19_Application_Gateway_Azure_27](https://github.com/user-attachments/assets/ea109034-83f3-46bc-8d48-12d222023164)


It will take 5 mins to get created, after that we can see the Application Gateway is created,

![19_Application_Gateway_Azure_28](https://github.com/user-attachments/assets/46e1f68f-fa2f-448c-b17c-2b9db2a327a5)


## Check the Backend Health of the Application Gateway to ensure it is Healthy

![19_Application_Gateway_Azure_29](https://github.com/user-attachments/assets/0f49f2a0-cc4e-43d8-9fac-9391ad717974)


## Copy the Public IP of Application Gateway - http://52.226.176.40/

![19_Application_Gateway_Azure_30](https://github.com/user-attachments/assets/dc249f31-1a6d-4ba5-8fc0-e75155880d08)


## Try to open the link - http://52.226.176.40/index.html

![19_Application_Gateway_Azure_31](https://github.com/user-attachments/assets/66d2ee65-3cf6-4574-9ee8-55b4de28c7f5)



