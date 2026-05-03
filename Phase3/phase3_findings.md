# Phase 3: Forensic Evidence Analysis Report

** Case: Windows Target Forensic Investigation
** Investigator: GTCKali
** Date of Analysis: March 14, May 1-3, 2026
** Evidence Source: windows_target_image.dd (SHA256: c6efbc77323028c05969cbf0ea132a7e33311be4ee
f023843dbea71d985f4c69)

--

## 1. Disk Image Analysis (bulk_extractor)

### 1.1 URL Artifacts
bulk extractor recovered 70MB of URL artifacts from the disk image.
Key findings: 
- Microsoft Windows Update and certificate validation URLs (system-generated)
- Bing search queries including travel and geography searches
- Google APIs OAuth activity (googleapis.com/oauth2)
- Gmail access (mail.google.com)
- Facebook and Twitter URLs (embedded in browser cache)
- YouTube URLs (embedded in browser cache)
- remove-metadata.com (user visited)
- support.microsoft.com/en-us/help/4052623/update-for-windows-defender-antimalware-platform

### 1.2 Suspicious File Found
A file named invoice_2026.txt.css found at:
C:\Users|GTCTarget\Downloads|invoice_2026.txt.css

This file uses a double ectension technique (.txt.css) commonly used by malware to disquise executable 
or malicious content as a harmless text file. This is consistent with the simulated malware 
download performed during the investigation setup

### 1.3 Application Activity
Notepad.exe was found to be referenced a multiple time in the json.txt artifacts,
confirming multiple Notepad sessions were opened by the user GTCTarget. This is consistent 
with the creation of multiple text documents during the simulated user activity.

### 1.4 Email Artifacts
bulk_extractor recovered email addresses including:
- amirjanyan@gmail.com
- nsossonko@hotmail.com
- yourtech@gmail.com
- Various Microsoft system addresses

Most email artifacts are embedded in system files and browser cahce,
not evidence of direct user email activity.

### 1.5 Execitable Artifacts
winpe_carved directory contains carved Windows PE (executable) files recovered from
unallocated disk space, confirming software installation and execution activity on 
the target system.

### 1.6 Windows Defender Activity
URL artifacts reference Windows Defender antimalware platform update pages,
confirming Windows Defender was active on the system.

--

## 2. Network Traffic Analysis (Wireshark/tshark)

### 2.1 PCAP Overview
File: investigation_capture.pcap (672KB)
Captured: March 14, 2026

### 2.2 TCP Communications
Active TCP session observed between:
- 172.17.0.5 (WindowsTarget) <-> [Investigator IP Redacted]:61487 -RDP session (136 frames, 11KB)
- 172.17.0.5 <-> 20.60.255.33 - Azure cloud services
- 172.17.0.5 <-> 40.87.160.0 - Microsoft services
- 172.17.0.5 <-> 40.87.182.8 - Microsoft services
- 172.17.0.5 <-> 168.63.129.16- Azure DNS server (multile connections)

This confirms active communication between the WindowsTarget VM and both the investigator workstation
via RDP and Microsoft Azure cloud services during the investigation period. 

### 2.3 DNS Traffic
DNS queries observed from 172.17.0.5 to Azure DNS ( 168.63.129.16) resolving Azure Blob Storage domains:
- umsajmjmkkprknzfr3vv.blob.core.windows.net

This confirms the WindowsTarget VM was communicating with Azure cloud storage services during
the investigation period.

### 2.4 Network Timeline
All captured network activity occurred on March 14, 2026 between 17:10 and 17:12 UTC, 
consistent with the investigator RDP session.

### 2.5 Protocol Breakdown
Total frames: 2501, Total bytes: 647770
- TCP: 2494 frames
- HTTP: 562 frames
- SSH: 113 frames
- TLS: 9 frames
- DNS: 4 frames
- DHCPv6: 3 frames

HTTP and JSON traffic indicated web browsing activity during the capture period. SSH traffic confirms
secure remote access sessions.

--

## 3. Summary of Findings

| Artifact Type | Finding |
|---|---|
| Suspicious File | invoice_2026.txt.css in Downloads folder |
| Application Use | Multiple Notepad.exe sessions confirmed |
| Browser Activity | Bing searches, Gmail, remove-metadata.com |
| Network Activity | RDP session from [Investigator IP Redacted] on March 14,2026 |
| DNS Activity | Azure Blob Storage queries observed |
| Antivirus | Windows Defender active and uodated |
| Deleted Content | NTFS change journal carved (ntfsusn_carved) |

---

## 4. Tools Used
- bulk_extractor 2.1.1
- dc3dd 7.2.646
- Wireshark/tshark


