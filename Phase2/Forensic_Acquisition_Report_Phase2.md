# Phase 2- Evidence Acquisition

## Network Connectivity Verification

Before beginning forensic evidence acquisition, connectivity between the forensic workstation and the target system was verified. 
The Kali Linux forensic workstation (GTCKali) was assigned the private IP address 172.17.0.5. The target system (WindowsTarget) was assigned the private IP address 174.17.0.4. 

Both virtual machines were deployed within the same Azure virtual network to allow communication between systems. 

Network connectivity between the forensic workstation (GTCKali) and the target system (WindowsTarget) was verified using ICMP with the ping command.

Command used:

Ping 172.17.0.4
