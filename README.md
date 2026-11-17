# Network-Sensor-Deployment
Deployed an out-of-band network sensor using Suricata, Zeek, and Promiscuous Mode to analyze malicious traffic.
# Enterprise Network Visibility & SPAN Port Triage

## Objective
Designed and deployed a virtualized out-of-band network sensor to passively capture and analyze malicious traffic using industry-standard intrusion detection systems (IDS). 

## Technologies & Tools
* **Hypervisor:** Oracle VirtualBox
* **Operating Systems:** Ubuntu Server 22.04 LTS (Sensor), Kali Linux (Attacker)
* **Security Engines:** Suricata (v8.0.8), Zeek (v9.0.1)
* **Networking:** Netplan, Promiscuous Mode, Out-of-Band (OOB) architecture

## Architecture
* **Attacker Node:** Kali Linux injected aggressive Nmap Stealth SYN scans into the isolated network.
* **Sensor Node:** Ubuntu Server configured with a dual-homed network setup.
  * **Management Interface:** NAT with port forwarding for remote SSH access.
  * **Sniffing Interface (`enp0s8`):** Stealth SPAN interface configured without an IP address, running in promiscuous mode to passively sniff mirrored traffic.

## Implementation Phases

### 1. Sensor Network Configuration
Configured the stealth SPAN interface using Netplan and enabled promiscuous mode at the kernel level to ensure all packets on the wire were captured.
![Phase 1 - Netplan and Promiscuous Mode](Screenshot%202026-11-16%20182022.png)

### 2. IDS Engine Provisioning
Bypassed clipboard auto-formatting constraints by staging repository URLs in variables, successfully installing Suricata and Zeek from their official package repositories, and updating the system PATH.
![Phase 2 - Engine Versions](Screenshot%202026-11-16%20125429.png)

### 3. Engine Configuration & Integration
Modified `/etc/suricata/suricata.yaml` and `/opt/zeek/etc/node.cfg` via the CLI to bind both security engines exclusively to the stealth SPAN interface.
![Phase 3 - Engine Status](Screenshot%202026-11-16%20215116.png)

### 4. Attack Simulation & Telemetry Verification
Overcame auto-managed interface constraints in Kali Linux by manually forcing a dummy IP address and MAC routing. Generated malicious traffic using Nmap (`nmap -sS`). Verified that both engines successfully captured the out-of-band traffic in real-time.
![Phase 4 - IDS Telemetry logs](Screenshot%202026-11-16%20215143.png)
