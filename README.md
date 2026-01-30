# Digital File Extraction Task with Wireshark | Home-Lab

## 🔍 Project Overview

This project focuses on extracting digital files from network traffic using Wireshark packet captures (PCAP files). The lab demonstrates how captured network data can be analyzed to reconstruct files transmitted over the network, a critical skill for digital forensics, incident response, and SOC investigations.

The exercise emphasizes identifying file transfers, understanding network protocols, and safely extracting artifacts for further analysis without executing potentially malicious content.


## 🎯 Why This Matters

Attackers often transfer malicious files over the network during:

- Malware delivery

- Data exfiltration

- Command-and-Control (C2) communication

Being able to extract files from packet captures allows analysts to:

- Recover dropped or transferred files during an attack

- Analyze malware samples in a controlled environment

- Validate data exfiltration incidents

- Support forensic investigations with concrete evidence


## 🛠️ Tools Used

- Wireshark (packet capture and protocol analysis)

- PCAP files (network traffic samples)

- CodeBeautify (base64 to image converter)

- CyberChef

- Isolated analysis environment (virtual machine-Kali Linux)


## 📂 Analysis Focus Areas

- Identifying relevant protocols (HTTP, SSDP etc.)

- Detecting file transfers within network streams

- Reassembling transmitted objects from packet data

- Preserving extracted files for forensic analysis

- Deep analysis on individual files

## ✅ Outcome

This project demonstrates how network traffic can be leveraged to recover digital files and artifacts involved in suspicious or malicious activity. It highlights the importance of network visibility in detecting threats and supports deeper analysis beyond surface-level alerts.

## 📌 Key Takeaway

Network traffic contains valuable forensic evidence. With the right analysis techniques, files transmitted during an attack can be reconstructed and examined without ever touching the original endpoint.

