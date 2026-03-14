# Phase 2- Evidence Acquisition

## Network Connectivity Verification

Before beginning forensic evidence acquisition, connectivity between the forensic workstation and the target system was verified. 
The Kali Linux forensic workstation (GTCKali) was assigned the private IP address 172.17.0.5. The target system (WindowsTarget) was assigned the private IP address 174.17.0.4. 

Both virtual machines were deployed within the same Azure virtual network to allow communication between systems. 

Network connectivity between the forensic workstation (GTCKali) and the target system (WindowsTarget) was verified using ICMP with the ping command.

Command used:

ping 172.17.0.4

## Artifact Extraction and Analysis

After acquiring the partial forensic disk image, artifact extraction was performed using Bulk Extractor.

Command used: 

bulk_extractor Phase2/kali_disk_image.dd -o Phase2/artifacts

Bulk Extractor scans disk images and extracts forensic artifacts such as email addresses, URLs, network data, and file fragments.

The tool generated multiple artifact files including:

- email.txt
- httplogs.txt
- url.txt
- winprefetch.txt
- telephone.txt
- facebook.txt
- sqlite_carved.txt
- winprefetch.txt

These artifacts provide investigators with potential indicators of user activity and system usage during the simulated incident.

Example artifact review command:
cat Phase2/artifacts/url.txt | head

The extracted artifact files confirm that forensic data can be recovered from teh disk image, demonstrating the effectiveness of artifact extraction
during forensic investigations.


## Azure Disk Snapshot

After the simulated incident activity was performed on the WindowsTarget virtual machine, an Azure disk snapshot was created for forensic analysis.

Snapshot Name: WindowsTarget_EvidenceSnapshot
Resource Group: DFC-GTC
Location: East US 2
Snapshot Time: March 14, 2026

The snapshot preserves the disk state of the WindowsTarget virtual machine following the simulated incident. This allows investigators to analyze the system without altering the original evidence.



# Evidence Identification

The following evidence items were collected during the investigation.

| Evidence ID | Evidence Description | Source |
|-------------|----------------------|--------|
| EVT-001 | Azure VM disk snapshot| WindowsTarget |
| EVT-002 | Network capture (.pcap) | Kali forensic workstation |
| EVT-003 | Partial Kali disk image (.dd) | Kali forensic workstation |
| EVT-004 | Extracted artifacts using Bulk Extractor | Kali forensic workstation |

