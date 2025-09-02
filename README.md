# Setting-up-IIS-and-FTP-for-testing-in-windows7-for-testing-nessus



---

🖥 Step 1 — Enable IIS (Internet Information Services)

1. Log in to your Windows 7 VM.


2. Open Control Panel → Programs → Turn Windows features on or off.


3. Scroll down to Internet Information Services (IIS).


4. Expand it and check these boxes:

Web Management Tools → IIS Management Console

World Wide Web Services → Application Development + Common HTTP Features

FTP Server → FTP Service + FTP Extensibility



5. Click OK and let Windows install the components.




---

🌐 Step 2 — Verify IIS Web Server

1. Open a browser in your Windows 7 VM.


2. Type http://localhost in the address bar.


3. If IIS is installed, you’ll see the default IIS Welcome Page.



This proves the IIS web server is running.


---

📂 Step 3 — Configure FTP Server in IIS

1. Open IIS Manager:

Start → Control Panel → Administrative Tools → Internet Information Services (IIS) Manager.



2. In the left panel, right-click on your computer name → Add FTP Site.


3. Enter:

Site name = TestFTP (or any name)

Physical path = create a folder like C:\FTPTest and select it.



4. Click Next → Binding and SSL Settings:

IP Address = leave as default (All Unassigned).

Port = 21 (default FTP port).

SSL = “No SSL” (since this is just for lab testing).



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
