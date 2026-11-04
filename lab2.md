# Lab 2 — A Windows guest, and a network between them

Pair: Diana O. / Inzhu Driver first half: Inzhu Machine: Windows 12 CPU, 15.3 GB RAM, 6 cores, guests Ubuntu Server + Windows 11 Enterprise eval Date: 01.11.2026-03.11.2026

 ## What I did 

1. First of all i downloaded a windows 11 from the site that was in the guide.
2. Then i went to Virtual Machine and created a lab02 VM on windows: 4 vCPU, ~5120 MB, 64 GB thin, UEFI + Secure Boot, vTPM.
3. I setted the network of NAT, NAT Network. Because NAT Network gives each VM a router (they cannot see each other); bridged is often blocked on campus Wi-Fi; host-only has no internet, and NAT Network provides both connection and internet.
4. Ping Ubuntu → Windows timed out because ping sends an ICMP echo request, and Windows Defender Firewall blocks incoming echo requests by default (the network profile of a fresh VM is usually Public, the strictest). Fixed with: New-NetFirewallRule -DisplayName "Allow ping in (ICMPv4)" -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -Action Allow
5. Overcommitment: set ~11 GB + ~11 GB on a 16 GB host.

# Network Diagram

          Internet
             |
 ## Host laptop 192.168.56.1 (campus Wi-Fi)
             |
     ## [VM NAT  10.0.2.0/24]
      /                    \
 ## lab01-ubuntu             ##  lab02-windows
 ## 10.0.2.4                    ##   10.0.2.3

# Result

| | Before | After |
|---|---|---|
| Host RAM in use | 10.2 GB of 15.3 GB (67%) | 15.2 of 15.3 GB (?) We couldn't open the task manager when testing |
| Available | 5 GB | 0 GB (?) |
| Committed / Swap used | 24.8 GB of 32.3 GB committed | 30.0 0f 32.0 GB commited |

# What did not worked the first time 

 - First time when i downloaded windows it worked not like in guide 
 - And our VM's didn't see each other even when they was connected to the one NAT Network (Because they was on pause)
 - Lagged not that hard like was written on the guide |

# Evidence:

[evidence/lab02 osa.jpg](evidence/lab02%20osa.jpg) Starting menu entrence
[evidence/lab2 osa.png](evidence/.gitkeep) The menu of the Windows
[evidence/VirtualBox_lab-windows02_31_10_2026_22_55_41.png](evidence/.gitkeep) Denied acssess to connect with ubuntu on NAT Network 
[evidence/VirtualBox_lab-windows02_31_10_2026_23_05_47.png](evidence/


