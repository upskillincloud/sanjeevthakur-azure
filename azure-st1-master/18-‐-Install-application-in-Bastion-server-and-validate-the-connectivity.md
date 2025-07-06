## Create Resource Group

Create a Resource Group for the Application Gateway.

![18_Application_Gateway_Azure_1](https://github.com/user-attachments/assets/b444a87b-b4ec-4652-8c4b-d3abf12500df)



## Create Virtual Network
Create a Virtual Network for the Application Gateway. Also create two Subnets of VNet

![18_Application_Gateway_Azure_2](https://github.com/user-attachments/assets/8b7ab1ac-ae52-4c8c-90e2-c2d40f6c148e)



## Create Virtual Machine
Create a Virtual Machine for the Application Gateway. 

![18_Application_Gateway_Azure_3](https://github.com/user-attachments/assets/68138ff9-0e23-438c-9331-c3a343d73a67)


## After creating the VM, we need to follow the below steps :
Install and Enable httpd service in VM.

Create an index file (/var/www/html/index.html)

Enable firewalld service in VM.

Add port 80 in firewall.

![18_Application_Gateway_Azure_4](https://github.com/user-attachments/assets/85cb7f88-88fb-4b32-939d-d8995baa4dba)
![18_Application_Gateway_Azure_5](https://github.com/user-attachments/assets/e2f3b4cf-a00f-4361-947e-1d5b5c229d01)
![18_Application_Gateway_Azure_6](https://github.com/user-attachments/assets/720ec100-2abe-4225-8331-5b9373877ee9)
![18_Application_Gateway_Azure_7](https://github.com/user-attachments/assets/a73410ae-57f7-481e-9e61-a2d79a9bdb5a)
![18_Application_Gateway_Azure_8](https://github.com/user-attachments/assets/44184820-dfcf-4db7-9bd4-506c2ac1d663)
![18_Application_Gateway_Azure_9](https://github.com/user-attachments/assets/1a8aee7f-80f8-4c1a-bef8-201c42db3336)
![18_Application_Gateway_Azure_10](https://github.com/user-attachments/assets/f467c5d6-296c-4550-a9a0-68cf62602f33)

```bash
   apt install firewalld -y
```

```bash
   systemctl enable firewalld
```

```bash
   systemctl start firewalld
```

```bash
   firewall-cmd --state
```

```bash
   firewall-cmd --permanent --add-port=80/tcp
```

```bash
   firewall-cmd --reload
```

```bash
   firewall-cmd --list-ports
```

```bash
   systemctl enable firewalld
```

![18_Application_Gateway_Azure_11](https://github.com/user-attachments/assets/54309d77-f8cd-46e4-8bde-4f7d22a58851)
![18_Application_Gateway_Azure_12](https://github.com/user-attachments/assets/5cee2de0-940c-49c3-aafa-366fe9e2088a)
![18_Application_Gateway_Azure_14](https://github.com/user-attachments/assets/4e75e11e-5a47-409e-a01f-8a9e8bfb677f)


