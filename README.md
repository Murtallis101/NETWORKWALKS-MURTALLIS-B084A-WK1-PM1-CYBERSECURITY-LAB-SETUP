# NETWORKWALKS-MURTALLIS-B084A-WK1-PM1-CYBERSECURITY-LAB-SETUP
WK1-PM1-CYBERSECURITY-LAB-SETUP

Step 1: Create and Configure the NAT Network in VirtualBox
To ensure all target and attacking machines can talk to each other on the 10.0.0.0/24 subnet while maintaining Internet access, we use a VirtualBox NAT Network.
	Open VirtualBox.
	Go to Tools (top menu or main panel) → Network → NAT Networks.
	Click Create (or +).
	Configure the NAT Network details:
	Network Name: LabNetwork 
	Network CIDR: 10.0.0.0/24
	Enable DHCP: Checked (optional, but convenient)
	Supports IPv6: Unchecked (optional)
	Click Apply.
  
Step 2: Create the Kali Linux Virtual Machine
	In VirtualBox, click New.
	Set the following parameters:
	Name: Kali-Linux
	Folder: Choose your preferred VM installation folder.
	ISO Image: Select the downloaded Kali Linux ISO file.
	Type: Linux
	Version: Debian (64-bit)
	Hardware Resources:
	RAM: Minimum 2048 MB (4096 MB recommended).
	Processors: At least 2 CPUs.
	Hard Disk:
	Disk Size: Minimum 20 GB to 30 GB dynamically allocated.
	Click Finish.


Step 3: Configure VM Settings (Clipboard, Shared Folder, Network)
Before starting the installation, configure the VM settings as specified:
1. Clipboard & Drag and Drop
	Select the Kali-Linux VM and click Settings.
	Navigate to General → Advanced.
	Set Shared Clipboard to Bidirectional.
	Set Drag'n'Drop to Bidirectional.

3. Network Configuration
	Go to the Network tab in Settings.
	Under Adapter 1:
	Attached to: Select NAT Network.
	Name: Choose LabNetwork (the network created in Step 1).
	Expand Advanced:
	Promiscuous Mode: Allow All or Allow VMs.

5. Shared Folders Setup
	Go to the Shared Folders tab.
	Click the Add Shared Folder icon (+).
	Set the following:
	Folder Path: Select or enter your host's downloads folder path (e.g., C:\Users\<YourUsername>\Downloads on Windows or /home/<user>/Downloads on Linux/macOS).
	Folder Name: downloads
	Auto-mount: Checked
	Make Permanent: Checked
	Mount point: /mnt/downloads (or leave blank to automatically mount under /media/sf_downloads).
	Click OK.

Step 4: Install Kali Linux
	Start the VM and select Graphical Install.
	Choose your preferred Language, Location, and Keyboard Layout.
	Hostname & Domain: Set hostname as kali (domain can be left blank).
	User Setup: Create a default non-root user (e.g., username kali, with your password).
	Partitioning: Select Guided - use entire disk, then All files in one partition.
	Complete the installation and reboot when prompted.

Step 5: Configure Static IP Address (10.0.0.2/24)
Once logged into Kali Linux, set the static IP address:
Option A: Via NetworkManager GUI (Recommended)
	Click the Network icon in the top-right panel → Edit Connections...
	Select Wired connection 1 and click the gear icon (Settings).
	Go to the IPv4 Settings tab:
	Method: Select Manual.
	Click Add under Addresses:
	Address: 10.0.0.2
	Netmask: 255.255.255.0 (or 24)
	Gateway: 10.0.0.1
	DNS Servers: 10.0.0.1, 8.8.8.8
	Click Save and re-enable the connection.

