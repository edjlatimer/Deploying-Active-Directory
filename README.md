# 🏢 Active Directory Installation and Deployment

<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Active Directory Logo" width="300"/>
</p>  

## 📌 Overview  
This project demonstrates how to install and configure **Active Directory Domain Services (AD DS)** in Azure.  
The setup includes:  
- Promoting a **Windows Server 2022 VM** to a Domain Controller  
- Creating a **domain admin account** and organizing resources in **OUs**  
- Joining a **Windows 10 client machine** to the domain  

---

## 🛠️ Technologies Used  
- **Microsoft Azure** (Virtual Machines, Networking)  
- **Windows Server 2022** (Active Directory Domain Services)  
- **Windows 10 (21H2)** (Client Machine)  
- **Active Directory Users and Computers (ADUC)**  
- **Remote Desktop & PowerShell**  

---

## ⚙️ Step-by-Step Implementation  

### 🔹 1. Install Active Directory Domain Services  
 
- Connect to `DC-1` using **Remote Desktop**  
- Once inside the `DC-1` VM, click **Start → Server Manager**.  
- In Server Manager, select **Add Roles and Features**.  

![AD add roles](https://github.com/user-attachments/assets/a1db6bc9-623b-4b01-8046-0ac54fe7fc9d)


- Click **Next** through the wizard until you reach *Server Roles*.  
- Check **Active Directory Domain Services** → click **Next → Install**.  
- Once installation finishes, a yellow flag appears in Server Manager. Click it → **Promote this server to a domain controller**.  
- Choose **Add a new forest** → enter domain name: `mydomain.com`.  
- Set a Directory Services Restore Mode (DSRM) password → click **Next** until **Install**.  
- The server will restart.  

![AD domain services](https://github.com/user-attachments/assets/7c5f88d4-d127-48df-8484-55eb0c1b696e)

![AD server to DC](https://github.com/user-attachments/assets/ae012834-88e1-40b6-9117-33f8c4a90f50)

![AD new forest](https://github.com/user-attachments/assets/830bf2c6-457a-4627-a19b-0b94800156c4)

- Log back in using **domain credentials**: `mydomain.com\labuser`.  


![AD domain login](https://github.com/user-attachments/assets/db65cf9b-24a9-4141-89d9-cf99a11e209b)


 

---

### 🔹 2. Create a Domain Admin User  
  
- From `DC-1`, open **Server Manager → Tools → Active Directory Users and Computers (ADUC)**.  
- Right-click the domain (`mydomain.com`) → **New → Organizational Unit (OU)**.  
  - Name the first OU `_EMPLOYEES`.  
  - Name the second OU `_ADMINS`.  

![AD _EMPLOYEES OU](https://github.com/user-attachments/assets/5461847d-b6a2-4feb-84be-f6eceb389d2b)

![AD _ADMINS OU](https://github.com/user-attachments/assets/73676180-ad02-4f45-85ca-69d93709238a)

- Right-click `_ADMINS` → **New → User**.  
  - First Name: Jane, Last Name: Doe  
  - User Logon Name: `jane_admin`  
  - Password: `**********`  
- After creating the account, right-click `jane_admin` → **Add to a group** → enter `Domain Admins` → click OK.  

![AD jane doe admin](https://github.com/user-attachments/assets/b6ccb19e-5c20-4939-898c-28cd44e53912)

![AD jane doe domain admin](https://github.com/user-attachments/assets/28ad1116-0304-4b07-990e-850056038038)


- Sign out of `DC-1`.  
- Sign back in as `mydomain.com\jane_admin`.  
- From now on, use this account (`jane_admin`) for all domain administration tasks.  

![AD Jane_doe domain login](https://github.com/user-attachments/assets/07729a57-a019-4668-a744-01dfcc968d70)

---

### 🔹 3. Join Client-1 to the Domain  
 
- In the Azure Portal, confirm `Client-1`’s DNS is set to `DC-1`’s **Private IP address**.  
- Restart `Client-1` from the Azure Portal.  
- Connect to `Client-1` via Remote Desktop using the local account (`labuser`).  
- On the desktop, right-click **This PC → Properties**.  
- Under **Computer Name**, click **Change Settings**.  
- Click **Change…** → Select **Domain**.  
- Enter: `mydomain.com`.  
- A login box will appear → enter credentials for `mydomain.com\jane_admin`.  
- If successful, you’ll see “Welcome to the mydomain.com domain”.  
- Restart `Client-1`.  
- On `DC-1`, open **ADUC** → expand the domain → click **Computers**.  
- Verify that `Client-1` appears as a joined computer.  
- Right-click the domain → **New → OU** → name it `_CLIENTS`.  
- Drag and drop `Client-1` into `_CLIENTS`.  

📸 Screenshot Example:  
![Join Client to Domain](https://via.placeholder.com/600x300?text=Join+Client+to+Domain)  

---

## 🎯 Outcome / Learnings  
By completing this project, I:  
- Installed and configured **Active Directory Domain Services** on a Windows Server VM.  
- Created a **domain admin account** and structured resources using OUs.  
- Successfully **joined a client machine** to the Active Directory domain.  
- Strengthened skills in **domain management, user roles, and organizational structure**.  

---
