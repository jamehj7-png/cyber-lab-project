Kali = attack machine


Ubuntu = monitoring / services machine


Windows Server = AD, DNS, DHCP


Windows Client = domain user machine


pfSense = firewall / gateway
Type this in Kali:
sudo apt update
What this does
This command tells Kali to refresh the list of available packages from its software repositories.
It does not install anything yet.
 It just checks online: “what software versions are available now?”
Why this matters
Before installing tools, you want Kali to know the latest package information. Otherwise, some installs can fail or use old package info.
What you should see
You will see lines like:
Hit:


Get:


Reading package lists... Done


If it finishes without error, that step is good.
Step 2: Upgrade installed packages
Then run:
sudo apt upgrade -y
What this does
This updates the software already installed on Kali to the newest available versions.
Why this matters
It makes the system cleaner and avoids problems caused by outdated packages.
What -y means
It automatically answers yes when Kali asks:
 “Do you want to continue?”
 
Step 3: Install SSH so you can use PuTTY
Now run:
sudo apt install openssh-server -y
What this does
It installs the SSH server on Kali.
Why this matters
SSH lets you connect to Kali remotely from your Windows machine using:
PuTTY


Windows Terminal


PowerShell


That means you can copy and paste commands easily instead of typing everything inside the VM.

Step 4: Start the SSH service
Run:
sudo systemctl start ssh
What this does
It starts the SSH service right now.
Why this matters
Installing SSH is not enough by itself. The service must be running so the machine can accept remote connections.

Step 5: Enable SSH at boot
Run:
sudo systemctl enable ssh
What this does
It tells Kali to start SSH automatically every time the VM boots.
Why this matters
Without this, SSH may stop working again after a reboot.
Step 6: Check SSH status
Run:
sudo systemctl status ssh
What this does
It checks whether the SSH service is running.
What you want to see
Look for:
active (running)
That means SSH is working.
To exit that screen, press
Step 7: Find Kali’s IP address
Run:
ip a
What this does
It shows the network interfaces and IP addresses of Kali.
Why this matters
You need the IP address to connect with PuTTY.
For example, you might see:
192.168.10.101


That is the address you put into PuTTY.
Step 8: Test SSH port
Run:
ss -tulnp | grep :22
What this does
It checks if port 22 is open and listening.
Why this matters
SSH normally uses port 22. If this command shows a line, SSH is listening correctly.

What you are doing overall right now
With these first commands, you are:
updating Kali


preparing it correctly


enabling remote administration


making command copy/paste easier


That is a normal first step in a real lab.

Do these now in this exact order
sudo apt update
sudo apt upgrade -y
sudo apt install openssh-server -y
sudo systemctl start ssh
sudo systemctl enable ssh
sudo systemctl status ssh
ip a
ss -tulnp | grep :22
After that, the next step will be installing the first security tool: Nmap, and I’ll explain exactly what it 
does too.

Step 9: Install Nmap
Run this on Kali:
sudo apt install nmap -y
What Nmap is
Nmap is a network scanning tool.
It helps you discover:
which machines are alive on the network


which ports are open


which services are running


sometimes the service version too


Why this matters in your lab
You have:
DC


client


Ubuntu


Kali


With Nmap, you can check:
is the DC reachable?


is DNS open?


is LDAP open?


is SMB open?


is SSH open on Ubuntu or Kali?


This is one of the most basic and important tools in a practical lab.
Step 10: Check that Nmap installed correctly
Run:
nmap --version
What this does
It confirms that Nmap is installed and working.
You should see a version number.
Step 11: First test scan against the Domain Controller
Run:
nmap 192.168.10.10
Replace the IP if your DC has another address.
What this does
This does a basic scan of the target machine.
It checks common ports and shows which ones are:
open


closed


filtered


What you may see on a DC
On a Windows Domain Controller, common open ports are often:
53 DNS


88 Kerberos


135 RPC


139 NetBIOS


389 LDAP


445 SMB


464 Kerberos password


636 LDAPS


3268 Global Catalog


That’s normal.
Step 12: Better scan with service detection
Run:
nmap -sV 192.168.10.10
What -sV means
It tries to detect the service version running on open ports.
So instead of only seeing:
port 53 open


You may see something like:
port 53 open, Microsoft DNS


Why this is useful
This gives you more practical information for:
documentation


troubleshooting


security analysis


What you are doing now
You are starting reconnaissance.
That means:
identifying services


understanding your target


learning how machines communicate


That is exactly what a real admin or security analyst does.
Run these now
sudo apt install nmap -y
nmap --version
nmap 192.168.10.10
nmap -sV 192.168.10.10


Step 9: Install Nmap
Run this on Kali:
sudo apt install nmap -y
What Nmap is
Nmap is a network scanning tool.
It helps you discover:
which machines are alive on the network


which ports are open


which services are running


sometimes the service version too


Why this matters in your lab
You have:
DC


client


Ubuntu


Kali


With Nmap, you can check:
is the DC reachable?


is DNS open?


is LDAP open?


is SMB open?


is SSH open on Ubuntu or Kali?


This is one of the most basic and important tools in a practical lab.
Step 10: Check that Nmap installed correctly
Run:
nmap --version
What this does
It confirms that Nmap is installed and working.
You should see a version number.
Step 11: First test scan against the Domain Controller
Run:
nmap 192.168.10.10
Replace the IP if your DC has another address.
What this does
This does a basic scan of the target machine.
It checks common ports and shows which ones are:
open


closed


filtered


What you may see on a DC
On a Windows Domain Controller, common open ports are often:
53 DNS


88 Kerberos


135 RPC


139 NetBIOS


389 LDAP


445 SMB


464 Kerberos password


636 LDAPS


3268 Global Catalog


That’s normal.
Step 12: Better scan with service detection
Run:
nmap -sV 192.168.10.10
What -sV means
It tries to detect the service version running on open ports.
So instead of only seeing:
port 53 open


You may see something like:
port 53 open, Microsoft DNS


Why this is useful
This gives you more practical information for:
documentation


troubleshooting


security analysis


What you are doing now
You are starting reconnaissance.
That means:
identifying services


understanding your target


learning how machines communicate


That is exactly what a real admin or security analyst does.
Run these now
sudo apt install nmap -y
nmap --version
nmap 192.168.10.10
nmap -sV 192.168.10.10


Nice — next we do SMB enumeration.
That means checking Windows file-sharing services and learning more about the DC.
Step 13: Install smbclient
Run:
sudo apt install smbclient -y
What smbclient is
It’s a tool to connect to SMB shares on Windows machines.
Why it matters
Windows uses SMB for:
shared folders


admin shares


some domain-related communication


With it, you can see whether the DC exposes shares like:
NETLOGON


SYSVOL


IPC$


Those are very common on a Domain Controller.

Step 14: Check it installed
Run:
smbclient --version
This just confirms the tool works.

Step 15: List shares on the DC
Run:
smbclient -L //192.168.10.10 -N
What this does
-L = list shares


//192.168.10.10 = target machine


-N = no password


Why this matters
This checks whether anonymous share listing is allowed.
On many systems it may fail, and that’s okay.
 If it works, you may see shares like:
ADMIN$


C$


IPC$


NETLOGON


SYSVOL


On a DC, NETLOGON and SYSVOL are especially important.

Step 16: Install enum4linux-ng
Run:
sudo apt install enum4linux-ng -y
What enum4linux-ng is
This is a Windows/SMB enumeration tool.
What it can do
It helps gather information such as:
machine name


domain name


SMB details


users or groups sometimes


operating system hints


shares


policies


Why it matters
It gives you a better picture of the Windows server, especially in an AD lab.

Step 17: Check it installed
Run:
enum4linux-ng --help
That confirms it’s available.

Step 18: Scan the DC with enum4linux-ng
Run:
enum4linux-ng 192.168.10.10
What this does
It performs SMB/Windows enumeration against the DC.
What you may learn
You may see:
hostname


domain/workgroup name


SMB signing info


accessible shares


operating system details


Why this is useful
This is practical for:
understanding the AD environment


documenting the DC


simulating internal recon



What you are doing in simple words
Right now you are moving from:
“Is the server alive?”
 to


“What Windows services and domain information can I learn from it?”


That’s a very real next step in a cyber lab.

Run these now in this order
sudo apt install smbclient -y
smbclient --version
smbclient -L //192.168.10.10 -N
sudo apt install enum4linux-ng -y
enum4linux-ng --help
enum4linux-ng 192.168.10.10


Nice — next we do SMB enumeration.
That means checking Windows file-sharing services and learning more about the DC.
Step 13: Install smbclient
Run:
sudo apt install smbclient -y
What smbclient is
It’s a tool to connect to SMB shares on Windows machines.
Why it matters
Windows uses SMB for:
shared folders


admin shares


some domain-related communication


With it, you can see whether the DC exposes shares like:
NETLOGON


SYSVOL


IPC$


Those are very common on a Domain Controller.

Step 14: Check it installed
Run:
smbclient --version
This just confirms the tool works.

Step 15: List shares on the DC
Run:
smbclient -L //192.168.10.10 -N
What this does
-L = list shares


//192.168.10.10 = target machine


-N = no password


Why this matters
This checks whether anonymous share listing is allowed.
On many systems it may fail, and that’s okay.
 If it works, you may see shares like:
ADMIN$


C$


IPC$


NETLOGON


SYSVOL


On a DC, NETLOGON and SYSVOL are especially important.

Step 16: Install enum4linux-ng
Run:
sudo apt install enum4linux-ng -y
What enum4linux-ng is
This is a Windows/SMB enumeration tool.
What it can do
It helps gather information such as:
machine name


domain name


SMB details


users or groups sometimes


operating system hints


shares


policies


Why it matters
It gives you a better picture of the Windows server, especially in an AD lab.

Step 17: Check it installed
Run:
enum4linux-ng --help
That confirms it’s available.

Step 18: Scan the DC with enum4linux-ng
Run:
enum4linux-ng 192.168.10.10
What this does
It performs SMB/Windows enumeration against the DC.
What you may learn
You may see:
hostname


domain/workgroup name


SMB signing info


accessible shares


operating system details


Why this is useful
This is practical for:
understanding the AD environment


documenting the DC


simulating internal recon



What you are doing in simple words
Right now you are moving from:
“Is the server alive?”
 to


“What Windows services and domain information can I learn from it?”


That’s a very real next step in a cyber lab.

Run these now in this order
sudo apt install smbclient -y
smbclient --version
smbclient -L //192.168.10.10 -N
sudo apt install enum4linux-ng -y
enum4linux-ng --help
enum4linux-ng 192.168.10.10

