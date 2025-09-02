# Setting-up-IIS-and-FTP-for-testing-in-windows7-for-testing-nessus



---

🖥 Step 1 — Enable IIS (Internet Information Services)

1. Log in to your Windows 7 VM.


2. Open Control Panel → Programs → Turn Windows features on or off.

   <img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/60b55227-356a-46d4-8b28-9d24c9386acd" />




---



4. Scroll down to Internet Information Services (IIS).


5. Expand it and check these boxes:

Web Management Tools → IIS Management Console

World Wide Web Services → Application Development + Common HTTP Features

FTP Server → FTP Service + FTP Extensibility

<img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/10ad33dd-de68-4969-a022-fb02c0a58546" />




5. Click OK and let Windows install the components.
6. let it restart!




---

🌐 Step 2 — Verify IIS Web Server

1. Open a browser in your Windows 7 VM.


2. Type http://localhost in the address bar.


3. If IIS is installed, you’ll see the default IIS Welcome Page.

   <img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/62567a71-add1-41c8-b76c-eca5108deaec" />




This proves the IIS web server is running.


---

📂 Step 3 — Configure FTP Server in IIS

step3.1
---
Create a new FTP-only user

1. Go to Control Panel → User Accounts → Manage another account.


2. Click Create a new account.
<img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/6f4db228-7f26-4799-8360-988af2c30020" />



4. Name it ftpuser.


5. Select Standard user/ administrator right if you want to give.


6. After it’s created, click it → Create a password.
   

Example: Password123.
<img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/f6498cf8-cc68-4dee-83d2-ac57e73f9294" />






---

1. Open IIS Manager:

Start → Control Panel → Administrative Tools → Internet Information Services (IIS) Manager.

<img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/4b63738f-41bc-4c8e-a68d-b79ec8bcf453" />


2. In the left panel, right-click on your computer name → Add FTP Site.

   <img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/80a9e3eb-e6d8-4619-be06-175c70f9c98b" />


   


4. Enter:

Site name = TestFTP (or any name)

Physical path = create a folder like C:\FTPTest and select it.

<img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/93f659ac-ec80-40fe-8c33-50e924fc7280" />


4. Click Next → Binding and SSL Settings:

IP Address = leave as default (All Unassigned).

Port = 21 (default FTP port).

SSL = “No SSL” (since this is just for lab testing).

<img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/d7fcfb3b-7c5f-4267-b4fc-25763745f238" />






5. Authentication and Authorization:

Authentication = Basic.

Authorization = “Specified users” → enter your Windows username.

Permissions = Read + Write.

<img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/d848e4b0-f28e-4669-9639-ec31ade324ba" />



6. Finish setup.




---

🛠 Step 4 — Test FTP Server

1. Open Command Prompt on Windows 7.


2. Run:

ftp localhost


3. Log in using the username + password you configured.


4. You should be able to upload/download files to C:\FTPTest.


If Issue OF User doesnot exist follow this:
---
      Step 1.1: Confirm FTP service is running

Open Command Prompt and run:

netstat -an | find ":21"

👉 You should see something like:

TCP    0.0.0.0:21    0.0.0.0:0    LISTENING

If you don’t see LISTENING, the FTP site isn’t started — restart it in IIS Manager (Stop → Start).


---

🔹 Step 2.1: Check IIS site bindings

In IIS Manager:

1. Select your FTP site (TestFTP).


2. On the right → click Bindings….


3. Make sure:

IP Address = All Unassigned (or the VM’s IP).

Port = 21.

Host name = (leave blank).



 👉 If you had entered a hostname earlier, Windows Explorer would fail. Hostname must be blank.
                  
 ---
                  
 🔹 Step 3.1: Verify FTP Authentication & Authorization
                  
 1. In FTP Authentication → ensure Basic is enabled.
                  
 2. In FTP Authorization Rules → ensure you have an entry:
                  
 Allow access to: Specified users → ftpuser
                  
 Permissions: Read + Write.
                  
                  
---
🔹 Step 4.1: Fix NTFS folder permissions
                  
 Even if IIS is configured, Windows must allow ftpuser to access the folder.
 
 If your FTP root is C:\FTPTest, open Command Prompt (as Administrator) and run:
                  
icacls C:\FTPTest /grant ftpuser:(OI)(CI)M /T
                  
 This grants Modify (read/write) rights for ftpuser.
                  
 ---
                  
🔹 Step 5.1: Log in with credentials
                  
Now, in Windows Explorer address bar, type:
                  
 ftp://127.0.0.1
                  
 When it asks for login, use:
                  
 Username: ftpuser
                  
Password: P@ssw0rd123 (or whatever you set).
                  
   ⚠ Important:
                  
You must have set a password for ftpuser. IIS FTP will not allow blank passwords.
                  
 If you still get “Windows cannot access this folder”, it usually means IIS is running but either:
                  
Hostname binding isn’t blank, or
                  
 NTFS permissions aren’t correct.
                  
                  
 ---
                  
                  
                  
                  
                  
 ---
 🔍 Step 5 — Confirm Networking for Assignment

 
 Make sure Kali and Metasploitable are on the same NAT network as Windows 7 (as per Step 3 in your assignment).
                  
 From Kali, try:
                  
 nmap -p 21,80 <Windows7-IP>
                  
  → You should see FTP (21) and HTTP (80) open.
 Nessus will later detect the IIS version and FTP service vulnerabilities.



---
