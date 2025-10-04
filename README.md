# Active Directory Installation and Deployment

<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Active Directory Logo" width="300"/>
</p>  

##  Project Overview  
This project demonstrates the **end-to-end deployment of Active Directory Domain Services (AD DS)** in an Azure-hosted lab environment. The goal was to simulate a small enterprise setup by:  

- Deploying a **Windows Server 2022 VM** as the Domain Controller (`DC-1`).  
- Installing **Active Directory Domain Services** and promoting the server to a new forest (`mydomain.com`).  
- Creating **Organizational Units (OUs)** to logically structure users, admins, and client machines.  
- Adding a **domain admin account** and leveraging it for privileged management.  
- Deploying a **Windows 10 client VM (`Client-1`)**, joining it to the domain, and validating connectivity.  
- Enabling **Remote Desktop** access for standard domain users (demonstrating real-world access configuration).  
- Using **PowerShell automation** to bulk-create user accounts for testing domain login and management scalability.  
 

---

##  Technologies Used  
- **Microsoft Azure** (Virtual Machines, Networking)  
- **Windows Server 2022** (Active Directory Domain Services)  
- **Windows 10 (21H2)** (Client Machine)  
- **Active Directory Users and Computers (ADUC)**  
- **Remote Desktop & PowerShell**  

---

##  Step-by-Step Implementation  

###  1. Install Active Directory Domain Services  
 
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

###  2. Create a Domain Admin User  
  
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

###  3. Join Client-1 to the Domain  
 
- In the Azure Portal, confirm `Client-1`’s DNS is set to `DC-1`’s **Private IP address**.  
- Restart `Client-1` from the Azure Portal.

![AD client-1 DNS to dc-1 copy](https://github.com/user-attachments/assets/087272e3-8d3c-4f00-b3c9-5767844c027d)

  
- Connect to `Client-1` via Remote Desktop using the local account (`labuser`).  
- On the desktop, right-click (desktop screen) → **Display Settings → About → Rename this PC (advanced)**.  
- Under **Computer Name**, click **Change Settings**.  
- Click **Change…** → Select **Domain**.  
- Enter: `mydomain.com`.  
- A login box will appear → enter credentials for `mydomain.com\jane_admin`.  
- If successful, you’ll see “Welcome to the mydomain.com domain”.

![AD connect cliet-1 to domain](https://github.com/user-attachments/assets/565436ce-e4f8-45a2-a595-6181d526925d)

![AD client-1 to domain complete](https://github.com/user-attachments/assets/dee9ed1e-9b22-4606-9b5d-33f11e9d18e8)

   
- Restart `Client-1`.  
- On `DC-1`, open **Active Directory Users and Computers** → expand the domain → click **Computers**.  
- Verify that `Client-1` appears as a joined computer.  

![AD client-1 domain verify](https://github.com/user-attachments/assets/9cda34ec-25d9-4b99-8a7c-b38a3ea7cc14)

- Then right-click the domain → **New → OU** → name it `_CLIENTS`.  
- Drag and drop `Client-1` into `_CLIENTS`.  

![AD _CLIENTS OU](https://github.com/user-attachments/assets/fb72b15d-af29-40d6-b555-60034cad02b3)

---

###  4. Setup Remote Desktop for Non-Admin Users  
 
- Log into `Client-1` as `mydomain.com\jane_admin`.  
- Right-click **Start Menu → System → Remote Desktop**.    
- Under **“Select users that can remotely access this PC”** → add **Domain Users**.  
- Now, any authenticated domain user can log into `Client-1`.  


![AD domain users access](https://github.com/user-attachments/assets/9870d767-b864-4e14-b2c9-20c349d745b5)


- *Note: In enterprise environments, this would typically be handled via **Group Policy** for multiple systems.*  


---

###  5. Bulk User Creation with PowerShell  
  
- On `DC-1`, log in as `jane_admin`.  
- Open **PowerShell ISE** as Administrator.  

![AD powershell ISE admin](https://github.com/user-attachments/assets/80c30ee6-6ff6-4d1c-bd40-4c051bc2f543)


- Create a new script file → paste the bulk user creation [Script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into it 


Run the script → observe user accounts being created.

![AD bulk user script](https://github.com/user-attachments/assets/0ace2d67-2668-4295-9f09-e9d862becd78)
![AD script running](https://github.com/user-attachments/assets/810cebc2-661d-4596-88b2-197abd9602b6)

Open ADUC → confirm users appear inside the _EMPLOYEES OU.

![AD bulk user login](https://github.com/user-attachments/assets/3ce65488-4d7e-452d-b3e7-89b8dd456575)

Test logging into Client-1 using one of the new users (e.g., `mydomain.com\bin.jux` with password `Password1`).

![AD bulk user login confirm](https://github.com/user-attachments/assets/859d53e4-850b-48c2-8783-4dcfa99bf8d2)

![AD bulk user login confirm 2](https://github.com/user-attachments/assets/097e2ac9-3f8d-4cef-b49b-83a9478df44a)

---

**Outcome / Learnings**

**By completing this project, I:**

- Installed and configured Active Directory Domain Services
- Created a domain admin account and structured resources with OUs
- Successfully joined a Windows 10 client machine to the AD domain
- Configured Remote Desktop access for non-admin domain users
- Automated bulk user creation using PowerShell scripting
- Gained practical experience with both GUI management tools and automation via 
  scripting
