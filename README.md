# Network Traffic Forensics Analysis

## Objective  
Conducted a detailed forensic analysis of a LAN segment network traffic capture (PCAP) to identify device types, operating systems, user activity, and potential malware indicators on the network. The goal was to answer specific investigative questions based on packet inspection, protocol analysis, and hash verification.

## Skills Demonstrated  
- Network protocol analysis (HTTP, LDAP, Kerberos)  
- Packet filtering and interrogation using Wireshark  
- Identification of operating systems and device types from network data  
- Extraction and analysis of files from PCAP data  
- Use of VirusTotal API for malware hash verification  
- Analytical reasoning to correlate IP addresses, hostnames, MAC addresses, and user accounts

## Tools & Technologies Used  
- Wireshark for packet capture analysis  
- VirusTotal for file hash reputation checking  
- Network protocol filters (HTTP, Kerberos, LDAP) for data extraction  
- MAC address lookup for vendor identification  

## Methodology / Walkthrough  
1. **Initial Protocol and Conversation Analysis**  
   - Examined protocol hierarchy and network conversations in the PCAP to understand traffic distribution and key endpoints.

2. **Device & OS Identification**  
   - Used HTTP User-Agent strings and DHCP info to identify OS types and devices at specific IPs.  
   - Leveraged Kerberos `cnameString` and LDAP queries to map hostnames and user accounts.  
   - Identified device vendor from MAC address using IEEE OUI lookup.

3. **File Extraction and Malware Analysis**  
   - Filtered traffic for HTTP transfers containing executable files using file signature filters.  
   - Extracted suspicious file (`40group.tiff`) masquerading as an image but actually a Windows executable.  
   - Computed SHA256 hash of the extracted file and queried VirusTotal for detection rate.

4. **Post-download Activity Correlation**  
   - Tracked subsequent outbound TCP connections to public IP addresses post file download.  
   - Mapped hostnames and user accounts for remote connections to establish potential command and control or data exfiltration.

## Key Findings  
| IP Address       | Device / OS                         | Additional Info                      |
|------------------|-----------------------------------|------------------------------------|
| 10.11.11.94      | Chromebook (64-bit)                |                                    |
| 10.11.11.121     | Linux (64-bit)                    |                                    |
| 10.11.11.145     | Unknown device, MAC vendor Motorola Mobility LLC (Lenovo) | MAC: bc:ff:eb:bc:2d:98             |
| 10.11.11.179     | macOS Catalina 10.15.1 (Intel Mac)|                                    |
| 10.11.11.195     | Windows 10 (64-bit)                |                                    |
| 10.11.11.200     | Windows host                      | Logged in user: brandon.gilbert    |
| 10.11.11.217     | iPadOS 13.2.2                    |                                    |
| 10.11.11.203     | Windows host (downloaded executable) | Downloaded from `http://acjabogados.com/40group.tiff` |

- **SHA256 Hash of Executable:** `8d5d36c8ffb0a9c81b145aa40c1ff3475702fb0b5f9e08e0577bdc405087e63`  
- **VirusTotal Detection Rate:** *[Insert detection rate]*  
- **Public IP contacted post-download:** `188.95.248.71`  
- **Remote Hostname & User:** `candice.tucker`

  
##Screenshots
![Screenshot 2025-05-29 193816](https://github.com/user-attachments/assets/05fd263f-3106-4aec-bf5c-535f700a37fa)
![Screenshot 2025-05-29 193939](https://github.com/user-attachments/assets/83371247-0581-4492-877f-444583a5f040)

![Screenshot 2025-05-29 194340](https://github.com/user-attachments/assets/e2f24ce7-f510-4a96-9605-17d9a9892e13)
![Screenshot 2025-05-29 194650](https://github.com/user-attachments/assets/263757e3-f84c-4dd9-89ab-ac6a1f03ce6e)
![Screenshot 2025-05-29 194757](https://github.com/user-attachments/assets/ff4f8679-159b-4732-8993-df4f13669e7e)

![Screenshot 2025-05-29 195001](https://github.com/user-attachments/assets/f16a4749-a5ab-474a-9d1d-9bb7c4b7028c)
## Conclusion  

The forensic analysis successfully identified all targeted devices and OSes on the LAN segment. The extraction of a disguised Windows executable file and its associated hash enabled reputation checking via VirusTotal, providing indicators of potential malware presence. Monitoring subsequent network connections revealed suspicious external communication, useful for further incident response.
