# Task 1 — Local Network Port Scan

- **Author:** NIVAS  
- **Date:** 2025-10-20  
- **Environment:** Windows 11, Nmap (with Npcap), Wireshark  



---

## 1. Objective
The goal of this task was to perform local network reconnaissance to discover open ports and running services on devices within the network. This helps in understanding potential exposure and identifying security risks.

---

## 2. Tools Used
- **Nmap** – for scanning hosts and discovering open ports and services  
- **Wireshark** – for capturing packet exchanges and analyzing SYN/SYN-ACK traffic  
- **Windows PowerShell** – for running commands and saving outputs  
- **VS Code** – for organizing files, managing Git, and preparing the repository  

---

## 3. Methodology
**Step 1 — Network Information**
- Ran `ipconfig` in PowerShell to get the Wi-Fi adapter details: IPv4 address, subnet mask, and default gateway.  
- Determined the network range to scan: `192.168.0.0/24`.

**Step 2 — Ping Sweep**
- Ran:  
  ```powershell
  nmap -sn 192.168.0.0/24 -oN scans/01_ping_sweep.txt
  ```
- Verified which hosts were active on the network. Screenshot: screenshots/02_ping_sweep.png.

**Step 3 — TCP SYN Scan**
- Ran full SYN scan to find open TCP ports:
 ```powershell
 nmap -sS -T4 192.168.0.0/24 -oA scans/03_allformats_syn
  ```
- Screenshot of sample host showing open ports: `screenshots/03_syn_scan_sample_host.png`.

**Step 4 — Service/Version Scan**
- For the main target (router at `192.168.0.1`) ran:
 ```powershell
 nmap -sS -sV -p 22,80,443,139,445,3389 192.168.0.1 -oN scans/wireshark_target_nmap.txt
 ```
- Screenshot showing service/version table: `screenshots/04_svc_192.168.0.1.png`.

**Step 5 — Wireshark Capture**
- Captured packets while running the targeted Nmap scan.
- Applied capture filter: host `192.168.0.1`
- Applied display filters to highlight key packets:
   - SYN packets: `tcp.flags.syn == 1 && tcp.flags.ack == 0` → `screenshots/07_wireshark_syn.png`
   - SYN-ACK replies:`tcp.flags.syn == 1 && tcp.flags.ack == 1` → `screenshots/07_wireshark_synack.png`
- Optional packet detail view: `screenshots/07_wireshark_packet_detail.png`

**Step 6 — OS Detection (Optional)**
- Ran:
 ```powershell
 nmap -O 192.168.0.1 -oN scans/06_os_192.168.0.1.txt
 ```
- Screenshot of OS guess: `screenshots/06_os_192.168.0.1.png`.

## 5. Recommendations
- Close or firewall unnecessary ports.
- Enable HTTPS for router admin interface and restrict to trusted IPs.
- Ensure strong passwords and updated firmware on network devices.
- Regularly scan network to monitor exposure.

## 6. Files Included in this Repository
- `scans/` – All Nmap outputs (TXT, NMAP, XML, GNMAP)
- `screenshots/` – IP config, scan outputs, Wireshark screenshots (all redacted for privacy)
- `pcaps/` – Optional sanitized Wireshark captures
- `README.md` – This document
