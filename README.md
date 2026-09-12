# -NETWORKWALKS-EMMANUEL-B083-WK1-PM1-CYBERSECURITY-LAB-SETUP
NETWORKWALKS-SONDA-B083-WK1-PM1-CYBERSECURITY-LAB-SETUP
Project Overview

This project focuses on setting up a virtual cybersecurity and penetration-testing laboratory using VirtualBox and Kali Linux.

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly.

The lab is configured on a private virtual network (NATNetwork) so that additional machines can be added later and used as targets for authorized security testing.

This lab is built entirely on machines and virtual environments that I own and control. No unauthorized systems were accessed as part of this project.

Tools & Requirements
Item	Detail
Hypervisor	Oracle VirtualBox
Attacker VM	Kali Linux
Target VMs	Windows 10 / Windows 11 / Windows 7 (optional) / Windows Server 2016 (optional) / Android (optional)
Extraction tool	7-Zip
Host PC specs	8GB RAM+, 256GB SSD+, Core i3/i5 or similar
Network Topology

All VMs are attached to a custom NATNetwork so they can reach each other while staying isolated from the host's main network.

NATNetwork range: 10.0.0.0/24

Machine	IP Address
Kali Linux (attacker)	10.0.0.2 /24
Windows 10	10.0.0.10 /24
Windows 11	10.0.0.11 /24
Windows 7 (optional)	10.0.0.7 /24
Windows Server 2016 (optional)	10.0.0.16 /24
Android (optional)	10.0.0.9 /24
Setup Steps
Phase 1 — Base attacker environment
Downloaded and installed 7-Zip.
Downloaded and installed VirtualBox on the host laptop/PC.
Configured VirtualBox network settings — created a NATNetwork in 10.0.0.0/24.
Downloaded and imported the Kali Linux virtual machine into VirtualBox.
Configured Kali Linux's static IP (10.0.0.2/24, gateway 10.0.0.1) on the NATNetwork adapter.
Took a snapshot of the Kali Linux VM after configuration.
