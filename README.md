
Cybersecurity & Ethical Hacking Lab
An isolated penetration-testing lab built on VMware Workstation, Kali Linux, and Windows virtual machines.
VMware Kali Windows Status
________________________________________
Table of Contents
1.	Overview
2.	Lab at a Glance
3.	Architecture
4.	Prerequisites
5.	Setup Guide
6.	IP Addressing Plan
7.	Connectivity Verification
8.	Snapshot Strategy
9.	Troubleshooting
10.	Skills Demonstrated
11.	Roadmap
12.	Disclaimer
13.	Author
14.	References
________________________________________
Overview
This project documents the design and deployment of a self-contained cybersecurity practice environment. The lab provides a safe, repeatable space to learn network configuration, ethical hacking techniques, and penetration testing without touching production systems.
All machines run as virtual machines on a private NAT network, so experiments stay contained and the environment can be rolled back to a known-good state at any time using snapshots.
Key outcomes
●	Isolated lab network with a custom 10.0.0.0/24 NAT subnet
●	Kali Linux attacker machine configured and verified
●	Windows target machines ready for security testing
●	Verified connectivity between all machines
●	Snapshot-based rollback workflow for safe experimentation
Lab at a Glance
Component	Details
Hypervisor	VMware Workstation (Pro / Player)
Attacker machine	Kali Linux
Target machines	Windows 10 / 11 / 7, Android VM (optional)
Network type	NAT (VMnet8)
Subnet	10.0.0.0/24 (mask 255.255.255.0)
Address range	10.0.0.2 – 10.0.0.99
Utilities	7-Zip (archive extraction)
Architecture
flowchart TB
    Host["Host Machine<br/>VMware Workstation"] --> NAT["NAT Network<br/>10.0.0.0/24"]
    NAT --> Kali["Kali Linux<br/>Attacker VM"]
    NAT --> Win["Windows VM<br/>Target"]
    NAT --> Android["Android VM<br/>Target (optional)"]
All virtual machines sit on the same NAT segment, which allows them to reach each other and, through the host, the Internet, while remaining isolated from the physical LAN.
Prerequisites
●	A host machine with virtualization enabled in BIOS/UEFI (Intel VT-x or AMD-V)
●	VMware Workstation installed
●	7-Zip for extracting downloaded VM archives
●	Kali Linux VMware image and Windows VM images
●	Administrator rights on the host (required to edit virtual networks)
Setup Guide
Phase 1: Kali Linux
1. Install 7-Zip
Install 7-Zip to extract the downloaded virtual machine archives (for example, the Kali Linux .7z image).
2. Install VMware Workstation
Install VMware Workstation and restart the host if prompted.
 
3. Configure the NAT network
In VMware, go to Edit → Virtual Network Editor → Change Settings, then configure the NAT network:
Setting	Value
Network	VMnet8 (NAT)
Subnet IP	10.0.0.0
Subnet mask	255.255.255.0
Apply the changes.
 
4. Import Kali Linux
Extract the Kali Linux archive with 7-Zip, then open the .vmx file in VMware via File → Open.
5. Configure Kali networking
Set the Kali network adapter to VMnet8 (NAT) so it joins the lab network, then confirm it received an address in the 10.0.0.0/24 range.
 
6. Take a baseline snapshot
Create a snapshot via VM → Snapshot → Take Snapshot so the machine can be restored before any security experiments.
IP Addressing Plan
Machine	Role	Example IP
Kali Linux	Attacker	10.0.0.2
VM 2	Target	10.0.0.10
VM 3	Target	10.0.0.7
VM 4	Target	10.0.0.16
VM 5	Target	10.0.0.9
VM 6	Target	10.0.0.11
These are example addresses from the lab guide. Actual addresses may differ depending on the VMware configuration.
Connectivity Verification
Confirm that machines can reach each other using ICMP:
ping 10.0.0.X
Replace 10.0.0.X with the address of the target VM. Successful replies confirm the virtual network is working. Note that Windows firewalls block ping by default, so a timeout may mean the target firewall is on rather than a network fault.
 
Snapshot Strategy
Snapshots make it safe to break things. Recommended restore points:
Snapshot	When to take it
Fresh install	Right after the OS is installed
Network configured	After IP and NAT settings are verified
Kali configured	After tools and updates are in place
Target ready	After the target machine is prepared
Pre-exploitation	Immediately before any attack exercise


 
Troubleshooting
Problem	Suggested fix
No Internet on Kali 2026.1 or later	Run sudo nmcli connection modify "eth0" ipv4.dad-timeout 0, then reconnect the interface
Internet issues persist	Try 10.0.0.1 as the gateway, as noted in the lab guide
VMs cannot ping each other	Check that every adapter is set to NAT on the same network, and that the guest firewall allows ICMP
Wrong IP range on a VM	Re-check the subnet settings in the Virtual Network Editor and renew the VM's address
Virtual Network Editor is read-only	Click Change Settings and approve the administrator prompt
Skills Demonstrated
●	Virtualization with VMware Workstation
●	Virtual machine deployment and management
●	Kali Linux and Windows VM configuration
●	NAT network design and IPv4 addressing
●	Network troubleshooting and connectivity testing
●	Snapshot-based backup and recovery
●	Preparing environments for ethical hacking and CTF practice
Roadmap
●	[x] Install VMware Workstation and 7-Zip
●	[x] Configure custom NAT network
●	[x] Deploy and configure Kali Linux
●	[x] Configure Windows VMs
●	[x] Verify connectivity and create snapshots
●	[ ] Network reconnaissance labs
●	[ ] Vulnerability assessment labs
●	[ ] Web application security testing
●	[ ] Digital forensics exercises
●	[ ] CTF challenges with additional offline VMs
Disclaimer
This repository is for educational purposes and authorized security testing only. Only test systems and networks that you own or have explicit written permission to assess. The author accepts no responsibility for misuse.
Author
Mustabshra Rehman Cybersecurity & IT Student, Cybersecurity B084 Interests: Cybersecurity, Ethical Hacking, Networking
References
●	Practical Lab Environment Setup for Pentesting, Ethical Hacking & Cybersecurity, NetworkWalks Academy (adapted from VirtualBox to VMware Workstation)

