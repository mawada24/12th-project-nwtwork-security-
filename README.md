# 🔐 Network Security Project - TLS & SSH Implementation

**Course**: CCY3201 - Network Security  
**Lecturer**: Dr. Ayman Adel  
**TA**: Eng. Abdelrhman Solyman  
**Authors**:  
- Nada Ibrahim (221003011)  
- Mawada Ayman (221003907)

---

## 🧾 Table of Contents

1. [Part 1: TLS – HTTPS Web App Using OpenSSL](#part-1-tls--https-web-app-using-openssl)
   - [A. Generate Certificates Using OpenSSL](#a-generate-certificates-using-openssl)
   - [B. Simple TLS Client-Server App](#b-simple-tls-client-server-app)
   - [C. Capture & Decrypt TLS Traffic](#c-capture--decrypt-tls-traffic)
2. [Part 2: SSH Configuration and Testing](#part-2-ssh-configuration-and-testing)
   - [Steps 1–8 for SSH Secure Setup](#steps-1–8-for-ssh-secure-setup)

---

## Part 1: TLS – HTTPS Web App Using OpenSSL

### A. Generate Certificates Using OpenSSL

#### 📍 On the **Server VM**:

1. **Create Root Certificate Authority (CA):**
   ```bash
   openssl genrsa -out rootCA.key 2048
   openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.pem
Create Server Key and CSR:

bash
Copy
Edit
openssl genrsa -out server.key 2048
openssl req -new -key server.key -out server.csr
Sign Server Certificate with Root CA:

bash
Copy
Edit
openssl x509 -req -in server.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out server.crt -days 500 -sha256
Client Certificate Generation:

bash
Copy
Edit
openssl genrsa -out client.key 2048
openssl req -new -key client.key -out client.csr
openssl x509 -req -in client.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out client.crt -days 500 -sha256
B. Simple TLS Client-Server App
📍 On the Server (Linux Mint Server):
Create the HTTPS Server Script:

bash
Copy
Edit
nano https_server.py
Run the HTTPS Server:

bash
Copy
Edit
python3 https_server.py
📍 On the Client (Linux Mint Client):
Connect to the Server Using curl:

bash
Copy
Edit
curl -k https://<server-ip>:<port>
Or open the IP in a web browser.

C. Capture & Decrypt TLS Traffic
Start Wireshark on the Server.

Connect from Client to Server via HTTPS.

Save the Capture File:

Copy
Edit
tls_client_server_221003907_221003011.pcap
Load Session Key into Wireshark:

Go to:
Edit → Preferences → Protocols → TLS

Set:

bash
Copy
Edit
(Pre)-Master-Secret log filename:
/home/mint/https_server.keylog
Part 2: SSH Configuration and Testing
Steps 1–8 for SSH Secure Setup
📍 Step 1: Install SSH
On Server:

bash
Copy
Edit
sudo apt install openssh-server
On Client:

bash
Copy
Edit
sudo apt install openssh-client
📍 Step 2: Generate SSH Keys (Client)
ED25519:

bash
Copy
Edit
ssh-keygen -t ed25519
RSA:

bash
Copy
Edit
ssh-keygen -t rsa -b 2048
📍 Step 3: Add New User on Server
bash
Copy
Edit
sudo adduser id221003907
# Password: mawada123
📍 Step 4: Test Password-Based SSH Login
bash
Copy
Edit
ssh id221003907@192.168.147.131
Password: mawada123

📍 Step 5: Copy Public Key to Server
bash
Copy
Edit
ssh-copy-id -i ~/.ssh/id_ed25519.pub id221003907@192.168.147.131
Then connect:

bash
Copy
Edit
ssh id221003907@192.168.147.131
📍 Step 6: Manually Copy Key to Server (Alt)
bash
Copy
Edit
scp ~/.ssh/id_ed25519.pub id221003907@192.168.147.131:~/
On server:

bash
Copy
Edit
cat id_ed25519.pub >> ~/.ssh/authorized_keys
📍 Step 7: Use PuTTY from Windows
Download PuTTY

Open PuTTYgen → Load your id_rsa

Save as id_rsa_windows.ppk

Connect using PuTTY:

Host Name: 192.168.147.131

Port: 22

Auth → Load private key file: id_rsa_windows.ppk

📍 Step 8: Disable Password Login (Server)
Edit SSH config:

bash
Copy
Edit
sudo nano /etc/ssh/sshd_config
Update:

bash
Copy
Edit
PasswordAuthentication no
Then restart SSH:

bash
Copy
Edit
sudo systemctl restart ssh
✅ Ensure only key-based login works.
📡 Capture SSH traffic using Wireshark during key-pair login.

📸 Screenshots / PCAP
Include:

TLS & SSH Packet Captures (.pcap)

Screenshots for:

Certificate generation

curl/browser access

Wireshark decryption

PuTTY login

SSH key-based access

📝 Notes
Ensure VMs are in the same NAT/Bridged network.

IPs used: 192.168.147.131 (Server).

📂 File Structure (Example)
vbnet
Copy
Edit
project/
├── certs/
│   ├── rootCA.pem
│   ├── server.crt
│   ├── server.key
│   ├── client.crt
│   └── client.key
├── https_server.py
├── tls_client_server_221003907_221003011.pcap
├── id_rsa_windows.ppk
└── README.md
📧 Contact
For any queries, contact us via GitHub Issues or via university email.

yaml
Copy
Edit

---

Let me know if you'd like to add screenshots, improve formatting, or split the instructions into separate files like `SSH.md` and `TLS.md`.
