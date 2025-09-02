# Setting-up-IIS-and-FTP-for-testing-in-windows7-for-testing-nessus



---

🖥 Step 1 — Enable IIS (Internet Information Services)

1. Log in to your Windows 7 VM.


2. Open Control Panel → Programs → Turn Windows features on or off.

   <img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/60b55227-356a-46d4-8b28-9d24c9386acd" />



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

1. Open IIS Manager:

Start → Control Panel → Administrative Tools → Internet Information Services (IIS) Manager.

<img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/4b63738f-41bc-4c8e-a68d-b79ec8bcf453" />


2. In the left panel, right-click on your computer name → Add FTP Site.
   <img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/c8c12b42-0e22-4081-98e5-65ecf696943d" />



4. Enter:

Site name = TestFTP (or any name)

Physical path = create a folder like C:\FTPTest and select it.
<img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/4c3465db-4535-47bf-900a-84518f38e651" />



4. Click Next → Binding and SSL Settings:

IP Address = leave as default (All Unassigned).

Port = 21 (default FTP port).

SSL = “No SSL” (since this is just for lab testing).
<img width="1022" height="858" alt="" src="https://github.com/user-attachments/assets/da9801ad-75b5-42a2-96c3-cf84d3846afc" />




5. Authentication and Authorization:

Authentication = Basic.

Authorization = “Specified users” → enter your Windows username.

Permissions = Read + Write.



6. Finish setup.




---

🛠 Step 4 — Test FTP Server

1. Open Command Prompt on Windows 7.


2. Run:

ftp localhost


3. Log in using the username + password you configured.


4. You should be able to upload/download files to C:\FTPTest.




---

🔍 Step 5 — Confirm Networking for Assignment

Make sure Kali and Metasploitable are on the same NAT network as Windows 7 (as per Step 3 in your assignment).

From Kali, try:

nmap -p 21,80 <Windows7-IP>

→ You should see FTP (21) and HTTP (80) open.

Nessus will later detect the IIS version and FTP service vulnerabilities.



---
