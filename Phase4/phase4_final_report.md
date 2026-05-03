**Case Title:** Windows Target Forensic Investigation
**Investigator:** GTCKali
**Date:** May 3, 2026
**Course:** Computer Forensics
**Status:** Final Report

---

## 1. Executive Summary

This report presents the findings of a comprehensive digital forensic investigation conducted on a Windows 10
virtual machine (WindowsTarget) hosted on Microsoft Azure. 
The investigation spanned four phases: project planning, forensic acquisition, evidence analysis, and final reporting.
The primary objective was to apply industry-standard forensic techniques to acquire, preserve, and analyze
digital evidence from a target system while maintaining chain of custody and evidentiary integrity.

Key findings include the discovery of a suspicious double-extension file (`invoice_2026.txt.css`) consistent with
malware delivery techniques, evidence of deliberate file deletion activity, metadata removal behavior, and
network communications with both legitimate Microsoft services and an unidentified external endpoint.
All evidence was acquired from a forensically sound disk image verified by SHA256 hash.

---

## 2. Case Overview

### 2.1 Investigation Objectives
- Perform a forensically sound acquisition of the WindowsTarget disk
- Analyze the acquired image for artifacts of malicious or suspicious activity
- Conduct network traffic analysis on a captured packet file
- Document all findings in a format suitable for evidentiary use

### 2.2 System Information
| Field | Details |
|---|---|
| Target Machine | WindowsTarget (Windows 10 VM) |
| Forensic Workstation | GTCKali (Kali Linux VM) |
| Platform | Microsoft Azure |
| Resource Group | DFC-GTC |
| Subscription | Azure for Students |
| Target Disk | /dev/sda — 127GB |
| Forensic Workstation Disk | /dev/sdb — 256GB |
| Image File | windows_target_image.dd |
| SHA256 Hash | c6efbc77323028c05969cbf0ea132a7e33311be4eef023843dbea71d985f4c69 |
| Imaging Date | May 1, 2026 at 18:42 UTC |
| Bad Sectors | 0 |

### 2.3 Simulated User Activity (Known Ground Truth)
The following activity was simulated on the WindowsTarget prior to imaging, serving as the known ground truth
for this investigation:
- Searched online for malware removal techniques
- Downloaded Norton Antivirus installer
- Downloaded a file named `invoice_2026.txt.css` (simulated malware — double extension technique)
- Created multiple Notepad documents
- Created a folder
- Deleted the documents and folder
- Visited remove-metadata.com

---

## 3. Methodology
This investigation followed standard digital forensic methodology to ensure evidence integrity and repeatability.

### 3.1 Forensic Principles Applied
- **Integrity:** SHA256 hashing used to verify the disk image before and after acquisition
- **Chain of Custody:** All steps logged in dc3dd_log.txt with timestamps
- **Non-Destructiveness:** The original disk (/dev/sda) was never mounted; all analysis was performed on the
image file
- **Documentation:** All findings recorded at time of discovery with supporting artifacts

### 3.2 Tools Used
| Tool | Purpose |
|---|---|
| dc3dd | Forensic disk imaging |
| bulk_extractor | Automated artifact extraction from disk image |
| tshark | Network packet analysis |
| Kali Linux | Forensic workstation OS |
| Azure Portal | VM and disk management |

### 3.3 Phases Overview
- **Phase 1:** Project planning and environment setup
- **Phase 2:** Forensic acquisition of WindowsTarget disk image
- **Phase 3:** Evidence analysis — bulk_extractor and network analysis
- **Phase 4:** Final reporting and case summary (this document)

---

## 4. Phase 2 Findings — Forensic Acquisition

### 4.1 Disk Imaging
The WindowsTarget disk was attached to the Kali forensic workstation as `/dev/sda` without mounting,
preserving its original state. The disk was imaged using `dc3dd`, a forensic imaging tool that provides real-time
hashing and detailed logging.

The imaging process completed successfully on May 1, 2026 at 18:42 UTC with **0 bad sectors** reported,
indicating the disk was in good physical condition and the image is a complete and accurate representation of 
the original.

### 4.2 Hash Verification
SHA256 hash of the acquired image:
c6efbc77323028c05969cbf0ea132a7e33311be4eef023843dbea71d985f4c69

This hash serves as the cryptographic fingerprint of the evidence. Any future analysis of this image can be 
verified against this value to confirm no tampering has occurred.

### 4.3 Acquisition Summary
- **Image file:** `windows_target_image.dd`
- **Image size:** 127GB
- **Method:** dc3dd bit-for-bit imaging
- **Log file:** `dc3dd_log.txt`
- **Result:** Successful — complete forensic image acquired

---

## 5. Phase 3 Findings — Evidence Analysis

### 5.1 bulk_extractor Analysis
`bulk_extractor` was run against the full `windows_target_image.dd` to carve artifacts without mounting the
image. The tool produced 179MB of artifacts stored in the `artifacts/` directory.

#### 5.1.1 Suspicious File — Double Extension Malware
**Finding:** A file named `invoice_2026.txt.css` was found at:
C:\Users\GTCTarget\Downloads\
**Significance:** The double extension technique (`.txt.css`) is a well-documented social engineering method
used by malware authors. The file appears to be a text document but carries a CSS extension, which can be used
to disguise malicious files from casual inspection. This is consistent with the simulated malware download in
the known ground truth.

#### 5.1.2 Application Activity
- Multiple **Notepad.exe** sessions confirmed in artifacts, consistent with the known document creation activity
- **Norton Antivirus** installer artifacts found, consistent with the simulated download
- **Windows Defender** artifacts present — active and updated at time of imaging

#### 5.1.3 Web and Email Artifacts
| Artifact Type | Value |
|---|---|
| Website visited | remove-metadata.com |
| Search activity | Bing search engine activity detected |
| Web service | Gmail access detected |
| Email address | amirjanyan@gmail.com |
| Email address | nsossonko@hotmail.com |
| Email address | yourtech@gmail.com |

**Note on remove-metadata.com:** The deliberate visit to a metadata removal site is behaviorally significant.
A user attempting to remove metadata from files prior to sharing them may be attempting to conceal the origin
or authorship of documents — a potential indicator of anti-forensic intent.

#### 5.1.4 NTFS Change Journal
The NTFS change journal (`ntfsusn_carved`) was successfully carved from the image. This journal records file
system changes including file creation, modification, and deletion events, and provides corroboration for the
known deletion activity simulated on the target.

### 5.2 Network Analysis (tshark)
Network analysis was performed on `investigation_capture.pcap` (672KB, captured March 14, 2026) using tshark.

#### 5.2.1 Capture Summary
| Field | Value |
|---|---|
| Total Frames | 2,501 |
| Total Bytes | 647,770 |
| Capture Date | March 14, 2026 |
| WindowsTarget IP | 172.17.0.5 |

#### 5.2.2 Network Connections Identified
| Destination | IP | Service |
|---|---|---|
| Azure DNS | 168.63.129.16 | DNS resolution |
| Azure Cloud | 20.60.225.33 | Microsoft cloud services |
| Microsoft Services | 40.87.160.0 | Microsoft infrastructure |
| Microsoft Services | 40.87.182.8 | Microsoft infrastructure |
| Investigator Workstation | Redacted | Port 61487 |

#### 5.2.3 Notable DNS Query
A DNS query was observed for:
umsajmjmkkprknzfr3vv.blob.core.windows.net
This is a randomly-named Azure Blob Storage endpoint. While Azure Blob Storage is a legitimate Microsoft
service, the randomized subdomain format is consistent with programmatically generated storage containers,
which can be used for data exfiltration or command-and-control (C2) communication in malware campaigns. 
This warrants further investigation in a real-world scenario.

#### 5.2.4 Protocols Observed
TCP, HTTP, SSH, TLS, DNS, DHCPv6
The presence of **SSH** traffic is notable and should be examined further in a real investigation to determine
whether it represents legitimate administrative access or unauthorized remote control.

---

## 6. Timeline of Events

| Date/Time | Event |
|---|---|
| March 14, 2026 | Network traffic captured (investigation_capture.pcap) |
| Prior to May 1 | Simulated user activity performed on WindowsTarget |
| May 1, 2026 — 18:42 UTC | Forensic disk imaging completed with dc3dd, 0 bad sectors |
| May 1–2, 2026 | bulk_extractor analysis run on windows_target_image.dd |
| May 1–2, 2026 | tshark analysis run on investigation_capture.pcap |
| May 3, 2026 | Phase 3 report finalized |
| May 3, 2026 | Phase 4 final report written |

---

## 7. Conclusions

This investigation successfully demonstrates a complete digital forensic workflow from acquisition through
analysis and reporting. The following conclusions are drawn from the evidence:

1. **Malware delivery technique confirmed:** The `invoice_2026.txt.css` file is consistent with a
double-extension social engineering attack, a method used to disguise malicious files.

2. **Anti-forensic behavior detected:** The visit to `remove-metadata.com` suggests the user was aware of
digital footprints and took steps to reduce traceability — a significant behavioral indicator.

3. **File deletion confirmed:** Notepad documents and a folder were created and subsequently deleted. The NTFS
change journal provides a forensic record of these deletions, demonstrating that deletion does not guarantee
permanent erasure.

4. **Suspicious network activity:** The DNS query to a randomized Azure Blob Storage endpoint and the presence
of SSH traffic are anomalous findings that would warrant deeper investigation in a real case.

5. **Forensic integrity maintained:** The SHA256 hash of the disk image provides cryptographic proof that the
evidence has not been altered. The original disk was never mounted, preserving the chain of custody throughout
the investigation.

---

## 8. Recommendations

- **In a real investigation:** The randomized Azure Blob Storage endpoint
(`umsajmjmkkprknzfr3vv.blob.core.windows.net`) should be subpoenaed to determine ownership and access logs.
- **SSH traffic** in the capture should be decrypted if keys are available to determine the nature of the
remote session.
- **Email accounts** identified (amirjanyan@gmail.com, nsossonko@hotmail.com, yourtech@gmail.com) should be
cross-referenced with the case subjects.
- **Memory forensics** (RAM capture) would complement this disk-based investigation and may reveal running
processes, encryption keys, or network connections not visible in static disk analysis.
- **Registry analysis** of the Windows image could further confirm user activity, installed programs, and USB
device history.

---

## 9. Appendix

### 9.1 Evidence File Hashes
| File | Hash (SHA256) |
|---|---|
| windows_target_image.dd | c6efbc77323028c05969cbf0ea132a7e33311be4eef023843dbea71d985f4c69 |

### 9.2 Project Directory Structure
~/Computer_Forensics_Project/
├── Phase1/
├── Phase2/
│   ├── Forensic_Acquisition_Report_Phase2.md
│   ├── Phase 2 Forensic Acquisition Report.docx
│   ├── windows_target_image.dd
│   ├── dc3dd_log.txt
│   ├── dc3dd.txt
│   └── artifacts/
├── Phase3/
│   ├── phase3_findings.md
│   ├── tshark_analysis.txt
│   ├── network_packets.txt
│   ├── network_timeline.txt
│   ├── url_artifacts.txt
│   ├── domain_artifacts.txt
│   └── Screenshots/
├── Phase4/
│   └── phase4_final_report.md
├── README.md
├── Inital_Proposal.md
└── Tools_Used.md

### 9.3 References
- dc3dd: https://sourceforge.net/projects/dc3dd/
- bulk_extractor: https://github.com/simsong/bulk_extractor
- tshark: https://www.wireshark.org/docs/man-pages/tshark.html
- NIST Digital Forensics: https://www.nist.gov/digital-forensics
