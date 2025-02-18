# SOC Home Lab Setup

## 📌 Project Overview
This repository documents the setup of a Security Operations Center (SOC) home lab using Oracle VirtualBox, SIEM tools, firewalls, network analyzers, and penetration testing tools. The lab is designed for cybersecurity learning, threat detection, and incident response practice.

## 🛠 Tools & Their Purpose
- **Oracle VirtualBox** – Virtualization platform to create and manage virtual machines (VMs).
- **SIEM (Security Information and Event Management)** – Collects and analyzes security logs for threat detection.
- **Firewalls (e.g., pfSense, iptables)** – Controls and monitors network traffic.
- **Network Analyzers (e.g., Wireshark, Zeek)** – Captures and analyzes network packets.
- **Penetration Testing Tools (e.g., Kali Linux, Metasploit, Nmap)** – Tests network and system security.

---

## 🔧 Step-by-Step Setup

###  Install Oracle VirtualBox
1. Download [VirtualBox](https://www.virtualbox.org/) for your OS.
2. Run the installer and follow the on-screen instructions.
3. Install the VirtualBox Extension Pack for additional features.

###  Set Up Virtual Machines
1. Open VirtualBox and click **New**.
2. Choose a VM name, and OS type, and allocate RAM & storage.
3. Attach an ISO image (e.g., Ubuntu, Windows, Kali Linux).
4. Complete the OS installation inside the VM.

### Splunk Deployment

1. Install the essentials
sudo apt update>
sudo apt install vim  -y >
sudo apt install net-tools -y >
sudo apt install open-vm-tools -y >
sudo apt install docker -y >
sudo apt install docker-compose -y

2. Download Splunk via wget>
wget -O splunk-9.4.0-6b4ebe426ca6-linux-amd64.tgz "https://download.splunk.com/products/splunk/releases/9.4.0/linux/splunk-9.4.0-6b4ebe426ca6-linux-amd64.tgz"

3. Install Splunk in /opt directory>
sudo tar -xvf plunk-9.4.0-6b4ebe426ca6-linux-amd64.tgz -C /opt

4. Change IP of Splunk Server>
ifconfig - Get IP Address>
sudo vim /opt/splunk/etc/system/local/web.conf>
Press i for insert mode>
[settings]>
httpport = 8000>
enableSplunkWebSSL = true >
mgmtHostPort = <new_ip>:8000

5. type :wq! to save changes

6. sudo vim /opt/splunk/etc/system/local/server.conf >
Press i for insert mode >
[sslConfig] >
enableSplunkdSSL = true >
sslVersions = tls1.2 >
bindip = <new_ip>
type :wq! to save changes

7. sudo vim /opt/splunk/etc/splunk-launch.conf >
Press i for insert mode >
SPLUNK_BINDIP=<new IP> >
type :wq! to save changes

8. Start Splunk >
cd /opt/splunk/bin >
sudo ./splunk start --accept-license >
sudo /opt/splunk/bin/splunk enable boot-start

9. Change Firewall Logs to allow port 8000 >
sudo ufw allow 8000 >

### Zeek NetFlow

1. Configure interface and set host IP >
sudo vim /opt/zeek/etc/node.cfg >
press i for insert mode >
Change localhost to <IP> >
Change interface to iface 

2. Configure network cidr to monitor >
sudo vim /opt/zeek/etc/networks.cfg >
press i for insert mode >
At the very end of the file:

(your chosen IP address)/24 >  

type :wq! to save changes >

3. Add Zeek Binary Path to bashrc >
sudo vim ~/.bashrc >
At the very end of the file >
press i for insert mode >

4. export PATH=$PATH:/opt/zeek/bin >

type :wq! to save changes >

source ./bashrc

5. Configure logs to parse in json >
sudo vim /opt/zeek/share/zeek/site/local.zeek >
At the very end of file: >
press i for insert mode >

@load policy/tuning/json-logs >

type :wq! to save changes 

6. Configure Inputs.conf in Splunk >
sudo vim /opt/splunkforwarder/etc/system >
press i for insert mode >

7. [default]
host = zeek

8. [monitor:///opt/zeek/logs/current] >
_TCP_ROUTING = * >
disabled = false >
index = zeek >
sourcetype = bro:json >
whitelist = \.log$ >

type :wq! to save changes

9. Start Splunk >
sudo splunk start --accept-license

10. Add Splunk Forwarder >
sudo splunk add forward-server <ip>:9997 >
Provide username and password >
splunk status


###  Active Directory Setup

1. Domain Controller 60 GB/split virtual disks
2. Install VM Tools
3. Change PC Name to XYZ
4. Open Server Manager
5. Navigate to Manage > Add Roles and Features
Click Next
6. Select "Role-Based or Feature-based installation">
Click Next
7. Select "Select a server from the server pool">
Click Next
8. Under Roles, Select "Active Directory Domain Services"
Click Add Features
Click Next>
Click Next>
Click Next>
Click Install
9. Once install is complete, click Close
At the top right, click on the yellow triangle on the flag
Click "Promote this server to a domain controller"
Select "Add a new forest"
10. In the Root Domain Name box, type the domain of your choice: "WWE.xyz">
Click Next
11. Enter a Password for DSRM>
Click Next>
Click Next>
12. Once NetBIOS domain name loads, click Next
Click Next>
Click Next>
Click Install 
13. Reboot
Login
14. Open Server Manager
15. Navigate to Manage > Add Roles and Features
Click Next
16. Select "Role-Based or Feature-based installation"
Click Next
17. Select "Select a server from the server pool"
Click Next
18. Under Roles, Select "Active Directory Certificate Services"
Click Add Features
Click Next>
Click Next>
Click Next>
Click Next>
19. Check the box "Restart the destination server automatically if required"
Click Yes>
Click Install>
Click Close>
20. At the top right, click on the yellow triangle on the flag
Click "Click Active Directory Certificate Services"
Click Next
21. Check Certification Authority
Click Next
22. Check Enterprise CA
Click Next
23. Select Root CA
Click Next
24. Select "Create a new private key"
Click Next>
Click Next>
Click Next>
25. In the validity period, type 100 Years (you can choose whatever number of years you like)
Click Next>
Click Next>
26. Click Configure
Click Close
27. Reboot
28. Add Users: Tools > Active Directory Users and Computers

###  Deploy DNS Server

1. Open Server Manager
2. Navigate to Manage > Add Roles and Features
Click Next
3. Select "Role-Based or Feature-based installation"
Click Next
4. Select "Select a server from the server pool"
Click Next
5. Check DNS Server
6. Click Add Features
Click Next>
Click Next>
Click Next>
7. Click Install
8. Click Close

### Create Forward and Reverse Lookup Zone

1. Open Server Manager
2. Select DNS
3. Right Click on Server
4. Right Click on Forward Zone
5. Click New Zone
6. Select Primary Zone
7. Click Next
8. Name the Zone "xyz.local.dns"
Click Next>
Click Next>
Click Next>
9. Click Finish
10. Right Click on Reverse Zone
11. Click New Zone
12. Select Primary Zone
Click Next>
13. Select IPv4 Reverse Lookup Zone
Click Next>
14. In Network ID, type first 3 octet of CIDR
Click Next>
Click Next>
Click Next>
15. Click Finish

### Set Advanced Audit Policy Configuration

To See what is turned on: 
run PowerShell as admin > audit-pol /get /category:*

1. Account Logon Events - Tracks user account authentication attempts
Computer Configuration -> Windows Settings -> Security Settings -> Advanced Audit Policy Configuration -> Audit Policies -> Account Logon -> Audit Credential Validation
Enable both Success and Failure

2. Logon Events - Monitors logon and logoff events
Computer Configuration -> Windows Settings -> Security Settings -> Advanced Audit Policy Configuration -> Audit Policies -> Logon/Logoff -> Audit Logon
Enable both Success and Failure

3. Object Access - Tracks access to files, folders, and other objects
Computer Configuration -> Windows Settings -> Security Settings -> Advanced Audit Policy Configuration -> Audit Policies -> Object Access
enable both Success and Failure for the Audit File System and Audit Registry

4. Process Tracking - Logs detailed tracking information for processes
Computer Configuration -> Windows Settings -> Security Settings -> Advanced Audit Policy Configuration -> Audit Policies -> Detailed Tracking -> Audit Process Creation
Enable Success

5. Policy Change - Monitors changes to policies, including audit policies
Computer Configuration -> Windows Settings -> Security Settings -> Advanced Audit Policy Configuration -> Audit Policies -> Policy Change
Enable both Success and Failure for Audit Audit Policy Change

6. Account Management - Tracks changes to user, group, and computer accounts
Computer Configuration -> Windows Settings -> Security Settings -> Advanced Audit Policy Configuration -> Audit Policies -> Account Management
Enable both Success and Failure for Audit User Account Management

### Suricata IPS Install

1. Update System>
sudo apt update && sudo apt upgrade -y

2. Add the Essentials>
sudo apt install net-tools -y>
sudo apt install open-vm-tools -y>
sudo apt install vim -y>
sudo apt install curl -y

3. Add Repo>
sudo add-apt-repository ppa:oisf/suricata-stable

4. Update package list>
sudo apt update

5. Install Suricata>
sudo apt install suricata -y

6. Verify Install
suricata --version

7. Configure IPS mode>
sudo vim /etc/suricata/suricata.yaml

af-packet:
  - interface: ens33>
    threads: auto>
    cluster-id: 99>
    cluster-type: cluster_flow>
    defrag: yes>
    use-mmap: yes>
    checksum-checks: auto>
    copy-mode: ips

8. Set Suricata to Drop Traffic

outputs:
  - fast:
      enabled: yes
  - eve-log:
      enabled: yes
      filetype: regular
      filename: eve.json
      types:
        - alert
	- drop

9. Enable Rulesets>
sudo suricata-update

10. Test Suricata>
sudo suricata -i ens33 -c /etc/suricata/suricata.yaml

11. Check for errors>
sudo tail -f /var/log/suricata/suricata.log

12. Enable Suricata>
sudo systemctl enable Suricata >
sudo systemctl start Suricata >
sudo systemctl status Suricata

13. Run in IPS mode>
sudo suricata -c /etc/suricata/suricata.yaml -q 0 >
sudo suricata -c /etc/suricata/suricata.yaml -i ens33

###  Deploy Firewalls
1. Download **pfSense** or configure **iptables**.
2. Assign network interfaces and set firewall rules.
3. Enable logging to forward data to SIEM.

###  Penetration Testing Setup
1. Install **Kali Linux** in a VM.
2. Update tools `sudo apt update && sudo apt upgrade`.
3. Use **Nmap** for network scanning, **Metasploit** for exploitation.
4. Conduct controlled tests and analyze logs in SIEM.



## 🚀 Next Steps
- Automate log collection using syslog or agents.
- Simulate attacks to test detection capabilities.
- Enhance security monitoring with additional tools.

## 📜 License
This project is for educational purposes only. Use responsibly.


