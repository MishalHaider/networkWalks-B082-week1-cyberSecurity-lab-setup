# NETWORKWALKS-B082-week1-cyberSecurity-lab-setup
First Week Project during the internship at "Network Walks".Building the setup for CyberSecurity Lab.


**1. Purpose of the lab (why a sandbox, why isolated network)**

Before jumping into hands-on testing, setting up an isolated virtual environment (a "sandbox") is essential for a few big reasons:
**Safety**: When testing tools, running port scans, or executing exploit scripts, you need a safe container. A sandbox keeps traffic contained so nothing accidentally leaks out onto your home Wi-Fi or local network.
**Control**: Having a dedicated setup lets you play around with static IPs, subnets, and gateways to mirror real-world networks without breaking actual live systems.
**Easy Resets**: If an experiment crashes the OS or mess up system files, we can quickly revert to a clean VirtualBox snapshot and start over in seconds, so our actual OS isnt compromised.

**2. Lab environment details**

Host Machine: Windows 11 (64-bit), 16 GB RAM, Intel i7 Processor
VirtualBox: Oracle VM VirtualBox (v7)
Guest OS: Kali Linux (2026.2)
Network Type: NAT Network
IP Configuration (10.0.0.0/24 Subnet):
Kali VM IP: 10.0.0.2
Subnet Mask: 255.255.255.0 (/24)
Default Gateway: 10.0.0.1
DNS: 8.8.8.8

**3.STEP BY STEP BUILD**


_Step 1: Download & Install Oracle VirtualBox_

**What was done:** Downloaded the VirtualBox installer from virtualbox.org and completed the setup on the host system.
**Why**: VirtualBox acts as our hypervisor—the core software that allows us to build and run isolated virtual machines (like Kali Linux) safely alongside our main operating system.

[Insert Screenshot]: Screenshot of the VirtualBox Manager dashboard right after installation.

_Step 2: Configure VirtualBox NAT Network (10.0.0.0/24)_

**What was done:** Opened VirtualBox Tools / Network Manager, created a new NAT Network, and set the IP network subnet to 10.0.0.0/24.
**Why**: Setting up a custom NAT Network creates an isolated virtual segment for our lab. It allows multiple VMs on this subnet to talk to each other while still securely sharing internet access through the host machine.

<img width="563" height="461" alt="virtualBoxSettings-Network" src="https://github.com/user-attachments/assets/b88c0ed7-746c-4a67-9dbc-3cfe7dfd5e30" />

_Step 3: Download & Import Kali Linux VM_

**What was done:** Downloaded the official pre-built Kali Linux VirtualBox image from kali.org, extracted it using WinRAR, imported it into VirtualBox, and attached its network interface to our custom NAT Network.
**Why**: Importing a pre-built image skips the long manual OS installation, and linking it to our NAT Network ensures it immediately joins the 10.0.0.0/24 lab subnet.

<img width="554" height="434" alt="installation of virtualbox and kali linux" src="https://github.com/user-attachments/assets/d7b40335-446d-4960-915b-5ba709443a52" />

_Step 4: Configure Kali Linux IP Settings (10.0.0.2)_

**What was done:** Started the Kali Linux VM and configured its main network adapter (eth0) with static parameters:
IP Address: 10.0.0.2,Netmask: 255.255.255.0 (/24),Gateway: 10.0.0.1,DNS: 8.8.8.8
(Applied via NetworkManager settings and verified through terminal commands like sudo ifconfig eth0 10.0.0.2 netmask 255.255.255.0 up).
**Why**: Statically assigning 10.0.0.2 gives Kali a fixed, predictable address on the local lab network, which is essential for consistent security testing and tools setup.

<img width="428" height="356" alt="virtualBoxSettings-ForKali" src="https://github.com/user-attachments/assets/de430e30-dc53-48b6-bdb2-63459a83576b" />

_**The Problem**_
**Missing IPv4 Address :**
~After turning the eth0 interface down and back up via terminal commands, NetworkManager dropped the active IP configuration.
Even after manually entering 10.0.0.2 in the GUI settings, NetworkManager failed to bind the IP to the interface, leaving eth0 without an IPv4 address (only an inet6 link-local address was showing).

<img width="328" height="327" alt="prob" src="https://github.com/user-attachments/assets/fc937707-483e-4a23-ba4d-7bc388af12c2" />

_**Solution**_

Bypassing NetworkManager via Terminal: Instead of relying on the GUI settings to negotiate the static IP, we manually forced the IP address, subnet mask, and gateway directly through the command line.

<img width="396" height="350" alt="solved" src="https://github.com/user-attachments/assets/af5e52c8-0367-463a-9418-410554870dd4" />

**The Problem**

No internet connection shown on Kali while fine connection on host machine.

<img width="523" height="205" alt="image" src="https://github.com/user-attachments/assets/501b5f7a-0edf-422d-8624-650c04b75a1a" />


**The Solution**

Tried to ping,showed unreachable,

Then, I switched the IPv4 method from Manual to Automatic (DHCP), so instead of you guessing the IP, your VM would just ask VirtualBox's NAT Network for one automatically.

<img width="451" height="414" alt="image" src="https://github.com/user-attachments/assets/ea026fe1-ba76-4685-a288-6559c394b079" />

_Step 5: Take Snapshot_

<img width="1117" height="628" alt="image" src="https://github.com/user-attachments/assets/e4e2152d-e24c-43e6-b7a5-7de967f21c27" />



_**What I Learned**_

Through building and troubleshooting this lab, I gained hands-on experience in setting up a secure virtual environment for cybersecurity practice. Working through both the setup process and network issues taught me several key concepts:

**Sandbox Safety & Isolation:** I learned why creating a sandbox is essential. Isolating hacking tools and target machines inside a dedicated virtual space ensures that experimental scans, scripts, or exploits never accidentally leak onto live home or public networks.

**NAT vs. NAT Network:** I learned the crucial difference between VirtualBox networking modes. While standard NAT isolates a single machine, a NAT Network creates a shared virtual switch where multiple VMs can talk to each other on the same subnet (10.0.0.0/24) while still safely routing traffic out to the internet.

**Linux Network Configuration & Troubleshooting:** Working directly inside Kali Linux gave me practical experience configuring network parameters. I learned how to set up static IPv4 addresses, subnets, and gateways—both through the graphical interface (nm-connection-editor) and via terminal commands like ifconfig and ip route.

**Fixing Routing & DNS Issues:** When our connection dropped, I learned how critical DNS and routing tables are. I gained firsthand troubleshooting experience using /etc/resolv.conf to set up nameservers like Google DNS (8.8.8.8) and managing default routes so the VM can translate web domains like google.com.

**VM Snapshots & Backups:** I learned why taking clean snapshots (like a "Clean_Lab_State") before running tests is a lifesaver. It gives you an instant recovery point to restore the VM if a script or configuration error breaks system files.

**Lab Documentation:** I realized that keeping detailed notes—documenting commands, screenshots, error codes, and step-by-step solutions—is just as important as running the tools themselves for professional cybersecurity projects.

**Tools & Resources**

VirtualBox: https://virtualbox.org/wiki/Downloads

Kali Linux: https://kali.org/get-kali

(I already had winrar so didn't install 7zip)



**Author**
Mishal Haider

Undergraduate Student | Pakistan


LinkedIn: www.linkedin.com/in/mishal-haider-9b750b37a



**Project Information**
Program Name: Cybersecurity at Networkwalks | Week: 01 | Project: Cybersecurity & Pentesting Lab Setup | Repository: GitHub
