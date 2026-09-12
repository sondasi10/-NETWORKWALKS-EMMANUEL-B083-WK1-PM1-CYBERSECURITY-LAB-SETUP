# -NETWORKWALKS-EMMANUEL-B083-WK1-PM1-CYBERSECURITY-LAB-SETUP

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
Phase 2 — Target machines
Downloaded and installed the additional VMs (Windows 10/11/7, Android) on VirtualBox, assigned each a static IP on the NATNetwork, ran ping tests between all machines, and took snapshots of each.
Verification

Connectivity between all VMs was confirmed with ping tests in both directions (Kali → targets and targets → Kali) after each machine was assigned its static IP.

ping 10.0.0.10   # Kali -> Windows 10
ping 10.0.0.2    # Windows 10 -> Kali

(Add your actual ping test screenshots below.)

 Troubleshooting Notes

(Document any issue you personally ran into and how you fixed it — e.g. Kali losing internet after setting a static IP, DAD timeout fix, DNS issues, etc. This section helps others in the batch.)

Example fix for internet connectivity issues on Kali Linux 2026.1+:

sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0<img width="1920" height="1080" alt="Screenshot (140)" src="https://github.com/user-attachments/assets/3f41d5b4-3710-41ea-a202-98d0339f3db2" />
<img width="1920" height="1080" alt="Screenshot (139)" src="https://github.com/user-attachments/assets/2ba1a025-c8c5-4aa6-b3eb-bd4c825a0642" />
<img width="1920" height="1080" alt="Screenshot (138)" src="https://github.com/user-attachments/assets/3bc39d61-2852-4ea2-b714-5899f85acf6e" />
