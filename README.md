# NETWORKWALKS-MURTALLIS-B084A-WK1-PM1-CYBERSECURITY-LAB-SETUP
WK1-PM1-CYBERSECURITY-LAB-SETUP

# 🛡️ Virtual Cybersecurity & Penetration Testing Lab Setup

## 📌 Project Overview
This project focuses on setting up a hands-on virtual cybersecurity and penetration testing lab using **VirtualBox** and **Kali Linux**. 

The goal of this lab is to establish a safe, isolated, and repeatable environment for testing security tools, running network scans, performing reconnaissance, and executing vulnerability assessments. 

By building this on a private virtual network, we can easily attach target machines later to simulate real-world attacks and defense scenarios safely.


🎯 Objectives
* Install and configure VirtualBox as our main hypervisor.
* Import and set up Kali Linux as the primary workstation.
* Build a isolated NAT Network for secure inter-VM communication.
* Configure network interfaces and assign a predictable, static IP to Kali Linux.
* Test and verify internet connectivity and internal DNS resolution.
* Take a clean baseline snapshot for instant recovery.
* Document the complete setup process and troubleshooting steps for future reference.



🛡️ Purpose of the Lab
This laboratory provides a controlled environment for cybersecurity learning, experimentation, and ethical security testing. 

Key use cases for this environment include:
* Network reconnaissance & mapping
* Port scanning with Nmap
* Vulnerability scanning and assessment
* Network packet analysis
* Web application security testing
* Exploitation and post-exploitation practice
* Hands-on practice with security tools


⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration |
| :--- | :--- |
| **🖥️ Host OS** | Windows 10 |
| **🧠 Host RAM** | 16 GB |
| **⚡ Processor** | Intel Core i7 |
| **🧰 Hypervisor** | VirtualBox 7.2 |
| **🐉 Security OS** | Kali Linux 2026.2 |
| **🧠 Kali RAM** | 2048 MB |
| **🌐 Virtual Network** | NAT Network |
| **📡 Network Address** | 10.0.0.0/24 |
| **🐧 Kali IP Address** | 10.0.0.2/24 |
| **🚪 Default Gateway** | 10.0.0.1 |
| **🌍 DNS Server** | 10.0.0.1 |
| **🔮 Target VM Range** | 10.0.0.3 – 10.0.0.99 |

---

## 🪜 Cybersecurity Lab Setup Procedure

### Step 1. Install Prerequisites
First, install **7-Zip** to extract compressed archive files (such as `.7z` virtual machine images). Next, download and install **VirtualBox** to manage our virtual infrastructure.
* [7-Zip Download](https://7-zip.org/download.html)
* [VirtualBox Download](https://virtualbox.org/wiki/Downloads)

### Step 2. Create the NAT Network
Rather than using standard NAT (which isolates virtual machines from one another), create a dedicated **NAT Network**. This allows VMs to talk directly to each other while still routing outbound internet traffic.

In VirtualBox, navigate to **Tools > Network > NAT Networks** and configure:
* **Network Name:** `NatNetwork`
* **IPv4 Prefix:** `10.0.0.0/24`
* **DHCP:** Enabled
* **IPv6:** Disabled

### Step 3. Import Kali Linux
Download the pre-built Kali Linux virtual machine from the official website and import it into VirtualBox. 

Under the VM's **Network Settings**, configure:
* **Adapter 1:** Attached to `NAT Network`
* **Name:** `NatNetwork`
* **Adapter Type:** `Intel PRO/1000 MT Desktop`

Allocate **2048 MB RAM** and set up a shared folder if you plan on transferring files easily between your host OS and Kali Linux.

### Step 4. Configure Static Network Settings in Kali
To keep things predictable during future lab exercises, set a static IPv4 address inside Kali Linux:
* Open **Network Connections** in Kali and edit **Wired connection 1**.
* Change the IPv4 method to **Manual**.
* Assign the following parameters:
  * **IP Address:** `10.0.0.2`
  * **Netmask:** `24` (or `255.255.255.0`)
  * **Gateway:** `10.0.0.1`
  * **DNS:** `10.0.0.1`

### Step 5. Take a Baseline Snapshot
Once Kali is running smoothly with network access, shut it down or pause it, then take a VirtualBox snapshot.
* **Snapshot Name:** `Clean Kali - Network Setup`

> 💡 **Why this matters:** This snapshot gives us a known-good recovery point. If a future exploit, test, or misconfiguration breaks the system, we can restore it to this exact state in seconds.

---

## 🔎 Verification & Testing

To confirm everything was set up properly, run the following verification checks:

| Test | Command | Expected Result |
| :--- | :--- | :--- |
| **Check IP Assignment** | `ip a` | Displays assigned IP (`10.0.0.2/24`) |
| **Test Gateway Ping** | `ping -c 4 10.0.0.1` | 0% packet loss |
| **Test Internet Ping** | `ping -c 4 8.8.8.8` | Successful replies from Google DNS |
| **Test DNS Resolution** | `nslookup networkwalks.com` | Resolves domain name to IP |
| **Verify Nmap Setup** | `nmap --version` | Outputs installed Nmap version |

---

## 🐞 Challenges & Troubleshooting

### Issue: Internet Unreachable After Setting Static IP
After switching Kali Linux to a manual/static IP address, internet connectivity stopped working even though local gateway pings were passing.

**Root Cause & Fix:**
This happens when duplicate address detection times out during startup. We resolved this by modifying the connection profile directly via `nmcli`:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
