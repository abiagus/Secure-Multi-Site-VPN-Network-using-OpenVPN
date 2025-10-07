# 🛡️ Designing and Testing a Secure Multi-Site Network Using OpenVPN and Cloud-Hosted Linux Servers

![Ubuntu](https://img.shields.io/badge/OS-Ubuntu%2022.04-orange)
![VPN](https://img.shields.io/badge/Tool-OpenVPN-blue)
![Status](https://img.shields.io/badge/Status-Completed-success)

---

## 📘 Project Overview

This project demonstrates the **design, implementation, and testing** of a secure, multi-site Virtual Private Network (VPN) using **OpenVPN** across Linux-based virtual machines.  
It simulates an enterprise-style setup where two remote sites communicate securely over the Internet through an encrypted VPN tunnel.

The network was built and tested using **VMware Workstation** with Ubuntu 22.04 LTS servers.  
Comprehensive system hardening was implemented with `iptables`, `fail2ban`, and **SSH key-based authentication** to minimize vulnerabilities and ensure confidentiality, integrity, and availability.

---

## 🧰 Tools & Technologies

| Category | Tool / Technology | Purpose |
|-----------|------------------|----------|
| VPN | **OpenVPN** | Secure site-to-site encrypted tunnel |
| PKI Management | **Easy-RSA** | Certificate and key generation |
| Firewall | **iptables** | Packet filtering and traffic control |
| Intrusion Prevention | **fail2ban** | Brute-force attack protection |
| Monitoring | **Wireshark**, **nmap**, **iperf3** | Traffic analysis, port scanning, performance testing |
| Virtualization | **VMware Workstation** | Isolated multi-VM testbed |
| OS | **Ubuntu Server/Desktop 22.04 LTS** | Reliable and security-patched Linux environment |

---

## 🧱 Network Architecture

The network simulates two enterprise sites connected via a central VPN server.


     ┌───────────────┐
     │ AdminHost     │
     │ Ubuntu Desktop│
     │ (Monitoring)  │
     └──────┬────────┘
            │ SSH
    ┌───────┴────────┐
    │ VPN Server      │
    │ Ubuntu Server   │
    │ OpenVPN Gateway │
    └──────┬──────────┘
           │ Encrypted Tunnel (UDP 1194)
    ┌──────┴──────────┐
    │ Site-B Client   │
    │ Ubuntu Server   │
    └─────────────────┘





**Network Subnets:**
| Interface | VPN Server | Site-B | AdminHost |
|------------|-------------|--------|------------|
| NAT (ens33) | 192.168.2.20 | 192.168.2.21 | 192.168.2.22 |
| Host-only (ens37) | 192.168.56.10 | 192.168.56.20 | 192.168.56.30 |
| VPN Tunnel | 10.8.0.1 | 10.8.0.2 | — |

---

## ⚙️ Implementation Steps

### 1. Virtual Machine Setup
- Installed **Ubuntu Server** and **Ubuntu Desktop** on three VMs.
- Configured static IPs using **Netplan**.
- Verified connectivity between all VMs via `ping` and `ssh`.

### 2. VPN Server Configuration
- Enabled IP forwarding in `/etc/sysctl.conf`.
- Created a **Certificate Authority (CA)** using Easy-RSA.
- Generated server certificates, keys, and Diffie-Hellman parameters.
- Configured OpenVPN with TLS-Auth (`ta.key`) and AES-256 encryption.
- Defined strict firewall rules via `iptables` (default DROP).

### 3. VPN Client Configuration
- Imported certificates and keys from VPN server.
- Configured client with remote server address and authentication.
- Verified successful connection via `ifconfig` and `ping`.

### 4. System Hardening
- Applied **iptables** rules to restrict open ports (22, 1194 only).
- Enabled **fail2ban** to monitor SSH login attempts and ban attackers.
- Disabled password login in SSH (`PasswordAuthentication no`).
- Verified SSH access via RSA key pairs only.

### 5. Testing & Validation
- **Performance:** `iperf3` tests (TCP/UDP) for throughput and latency.  
- **Reliability:** Automatic tunnel recovery after service reboot.  
- **Security:** `nmap` scan verified only 22 and 1194 open ports.  
- **Encryption:** Verified with Wireshark — no plaintext traffic.

---

## 📊 Key Results

| Test Type | Observation | Outcome |
|------------|--------------|----------|
| TCP Throughput | ~152 Mbps (Single stream) | ✅ Stable and efficient |
| Parallel Streams | ~178 Mbps total (4 streams) | CPU near 100% |
| UDP Jitter | 0.017 ms | ✅ Excellent |
| Tunnel Reconnection | 6–33 sec recovery | ✅ Reliable |
| Port Scan | Only ports 22 & 1194 open | ✅ Hardened |
| Brute-Force Attack | Blocked by fail2ban | ✅ Secure |

---

## 🔒 Security Highlights

-  **2048-bit RSA certificates** for encryption and authentication  
-  **TLS-Auth** key prevents packet injection and DoS  
-  **Firewall isolation** with `iptables` (default DROP policy)  
-  **Passwordless SSH access** using key pairs  
-  **fail2ban** blocks brute-force attacks in real time  
-  **Wireshark verification** confirmed encrypted tunnel traffic  

---

## 🧪 Performance Screenshots & Configurations

All screenshots in the end of the report attached.
- OpenVPN server/client  
- iptables ruleset  
- fail2ban logs  
- iperf3 and nmap results  
- Wireshark captures  

---

## 🧑‍💻 Author

**Name:** Abi Ashok
**Course:** Master's in Computer Networks and Systems Security

---

## ⭐ Acknowledgements

Special thanks to the **University of Hertfordshire** and my supervisor for their continuous guidance and support during this research project.
