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

### 1️⃣ Install Oracle VirtualBox
1. Download [VirtualBox](https://www.virtualbox.org/) for your OS.
2. Run the installer and follow the on-screen instructions.
3. Install the VirtualBox Extension Pack for additional features.

### 2️⃣ Set Up Virtual Machines
1. Open VirtualBox and click **New**.
2. Choose a VM name, OS type, and allocate RAM & storage.
3. Attach an ISO image (e.g., Ubuntu, Windows, Kali Linux).
4. Complete the OS installation inside the VM.

### 3️⃣ Install & Configure SIEM
1. Download an open-source SIEM (e.g., **Splunk**, **ELK Stack**, **Wazuh**).
2. Install it inside a dedicated VM.
3. Configure log sources (firewalls, IDS, endpoint logs).
4. Set up alerts and dashboards.

### 4️⃣ Deploy Firewalls
1. Download **pfSense** or configure **iptables**.
2. Assign network interfaces and set firewall rules.
3. Enable logging to forward data to SIEM.

### 5️⃣ Network Traffic Analysis
1. Install **Wireshark** or **Zeek** in a monitoring VM.
2. Capture and inspect network traffic.
3. Export logs to SIEM for correlation.

### 6️⃣ Penetration Testing Setup
1. Install **Kali Linux** in a VM.
2. Update tools (`sudo apt update && sudo apt upgrade`).
3. Use **Nmap** for network scanning, **Metasploit** for exploitation.
4. Conduct controlled tests and analyze logs in SIEM.

---

## 📊 Diagrams
### SOC Home Lab Network Topology
<img src="images/soc_network_topology.png" alt="SOC Home Lab Network Topology" width="600"/>

### Virtual Machine Layout
<img src="images/virtual_machine_layout.png" alt="Virtual Machine Layout" width="600"/>


## 🚀 Next Steps
- Automate log collection using syslog or agents.
- Simulate attacks to test detection capabilities.
- Enhance security monitoring with additional tools.

## 📜 License
This project is for educational purposes only. Use responsibly.


