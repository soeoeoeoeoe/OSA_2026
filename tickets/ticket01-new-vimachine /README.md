# Ticket 1 Create your own Virtual Machine 

## What was asked 

From: Aigerim T. (Office Manager)
Subject: test machine for new dev
Hi, we have someone starting on Monday and he needs a test computer
set up. Nothing special, just so he can try things without breaking
anything. Can you write down how to do it so I can do the next one
myself? Thanks

## What was actually needed

**What the request left out but the job requires**

- The request did not specify which operating systems are required (Linux, Windows, or both).
- No hardware resource details (host RAM, free disk space, CPU cores) or target VM allocations (memory, disk size) were provided.
- The task did not specify that VirtualBox's default isolated NAT mode places each virtual machine inside its own private network namespace, preventing direct VM-to-VM communication.
- It also omitted that Windows Firewall drops inbound ICMP (Echo Request) packets by default.

  **Where the request is wrong**

  - It was assumed that the grayed-out checkbox for VirtualBox Guest Additions indicated a blocked or unselectable component, whereas it actually denotes a mandatory core component that is permanently enabled and protected from accidental de-selection.
  - IP addresses 10.0.2.3 and 10.0.2.4 were probed randomly under the assumption that they belonged to the other active VM. In a standard VirtualBox NAT segment, these addresses are either inactive or do not respond to ICMP ping requests, triggering a Destination Host Unreachable error.
 
    **What I would have asked**
    -  Which network topology was required for the assignment (e.g., NAT Network, Internal Network, or Bridged Adapter), and what specific static IP ranges or DHCP parameters should be assigned to the Windows and Ubuntu nodes.
    -  How to use a github
    -  which operating system requires
    -  Where it runs and with what resources? The request gives no hardware.It was pretty hard to understand a guide because it was not so detailed.
      
      **What I assumed instead**
    - I assumed the default WDDM display driver profile should be used for Windows 11 without legacy Direct3D support, and that inbound ICMPv4 traffic needed to be manually unblocked via an elevated Windows Firewall rule to enable connectivity testing.
    - Which OS does the developer actually work with? It was both windows and ubuntu Linux.
      
      ## 3. What I did
    1.Completed the VirtualBox Guest Additions installation in Windows 11:

       - Kept the default mandatory VirtualBox Guest Additions component selected in the setup wizard.
       - Rebooted the guest OS.
       - Reason: Required to enable dynamic desktop resolution resizing and the bidirectional shared clipboard.
         
    2.Enabled bidirectional shared clipboard integration:
       - Navigated to the VirtualBox top menu: Devices → Shared Clipboard → Bidirectional.
       - Reason: Host-to-guest isolation disables clipboard synchronization by default for security reasons.
    
    3.Diagnosed the virtual network adapter and default gateway:
       - Ran a connectivity check against the hypervisor gateway inside PowerShell:
      ping 10.0.2.
       - Reason: Verified that the virtual NIC was functional and communicating with the hypervisor
    

      **Settings that different from the defaults, and why**
    
    | Settings | Value | Why |
    |---|---|---|
    | Network Adapter Mode (Windows & Ubuntu) | Internal Network (or NAT Network) named labnet | Default NAT mode isolates each VM inside separate network namespaces with the same private IP, preventing them from communicating directly with each other. Switching to a shared internal/NAT network attaches them to a common virtual switch. |
    | Inbound ICMPv4 Traffic (Windows Firewall) | Allow (via custom inbound firewall rule) | Windows Defender Firewall drops inbound ICMP echo requests by default, preventing other hosts from pinging the machine even when the network layer is configured correctly. |
    | Shared Clipboard | Bidirectional | Disabled by default for guest isolation security; enabling it allows copying and pasting commands, text, and scripts between the host OS and the VM. |
    | Drag and Drop | Bidirectional | Disabled by default for security; enabling it allows direct file transfers between the host and the virtual machine window without configuring network shares. |
    | Display Integration / Guest Additions | Installed (WDDM Driver) | Windows 11 defaults to a basic display adapter with a fixed screen size; installing Guest Additions enables dynamic screen resolution autoscaling and mouse integration. |
    
     **One thing that went wrong, and how I resolved it**

    - After installing the VirtualBox Guest Additions package, the Ctrl + C and Ctrl + V keyboard shortcuts did not work when attempting to copy and paste text between the host PC and the Windows 11 guest virtual machine. This initially gave the false impression that the guest additions installation had failed or that the integration drivers were corrupted. In the top application menu of the active virtual machine window, navigated to Devices → Shared Clipboard. And it worked
    - After configuring both the Windows 11 and Ubuntu virtual machines to use a shared NAT Network, they still completely failed to communicate or ping each other. Network diagnostics showed Destination Host Unreachable and No route to host, even though both adapters were configured to use the same network name.The VirtualBox NAT Network service itself had been left in a Paused (or suspended) state within the VirtualBox Global Network Preferences. In VirtualBox, if a NAT Network profile is paused or disabled, its internal virtual DHCP server and switching engine stop forwarding packets entirely. Because the network switch wasn't actively running, the two virtual machines were effectively plugged into a disconnected virtual router, preventing ARP resolution and packet routing between nodes.
   
      ## 4. Evidance

      | file | What it proves |
      |---|---|
      | [evidence1](evidence/lab02osa.png) | Proves that the VirtualBox Guest Additions ISO was mounted as drive D: and that the core installation wizard was successfully launched with mandatory guest integration components pre-selected. |
      | [evidence1](evidence)] | Proves the successful base deployment and first boot of the Windows 11 Enterprise Evaluation guest operating system inside the VirtualBox virtual machine. |
      | [evidence1](evidence/test)] | Proves successful hypervisor gateway reachability |
      | [evidence1](evidence/test)] | Proves that an elevated administrator session successfully registered an inbound Windows Firewall rule allowing incoming ICMPv4 (ping) traffic. |
      | [evidence1](evidence/test)] | Proves successful installation and authentication on Ubuntu Server 26.04.1 LTS, showing the active IP address 10.0.2.4 on interface enp0s3, system resource stats, and the verified lsb_release -a output. |
      | [evidence1](evidence/test)] | evidence/screen ubuntu osa.png — Proves that the primary network interface enp0s3 is operational and in an UP state with the active IPv4 address 10.0.2.4/24 via the output of ip -br a |
      | [evidence1](evidence/test) | evidence/ubuntu 01 osa.png — Proves successful execution of Step 7 guest inspection commands (lsb_release -a, hostname, nproc, free -h, df -h /, ip -br a) while documenting SSH connection failures caused by running host-targeted commands from inside the guest. |
      | [evidence1](evidence/test) | Proove that i did a snapshoot |
      
      ## 5. What I would do differently
      
    - I would look at guide carefully and be more focudes
    - I would ask Aigerim a questions that i had
    - I wouldn't trust something like this to me definentily
    - I would like to work with people who had/have a background 
