# 🛡️ Network Traffic Filtering with pfSense

A hands-on firewall configuration and policy enforcement project using **pfSense**, focusing on DNS-based URL blocking, DHCP/DNS control, and custom rule creation to secure and regulate network access.

---

## 🔁 Project Overview

| Feature           | Description                                             |
| ----------------- | ------------------------------------------------------- |
| Platform          | pfSense (Firewall/Router OS)                            |
| Tools Used        | pfBlockerNG, DNSBL, Firewall Rules, DHCP, DNS Resolver  |
| Attack Simulation | Kali Linux (for testing and bypass attempts)            |
| Primary Focus     | Block URLs, enforce access control, and monitor traffic |
| Testing Targets   | ChatGPT, social media, and custom URL block validation  |

---

## 🚀 Objectives

* Deploy pfSense in a **lab environment** with multiple interfaces (WAN, LAN, LAN2)
* Block specific domains (e.g., ChatGPT) using **pfBlockerNG's DNSBL**
* Create **firewall rules** for traffic filtering
* Control DNS/DHCP to prevent policy bypassing
* Test effectiveness using **Kali Linux** as the test machine

---

## 📚 Technologies Used

| Technology   | Purpose                              |
| ------------ | ------------------------------------ |
| pfSense      | Main firewall and router             |
| pfBlockerNG  | DNS-level URL blocking               |
| Kali Linux   | Testing for bypasses and validation  |
| DHCP Server  | Policy-based IP allocation           |
| DNS Resolver | Internal DNS control for all clients |

---

## 🔧 System Requirements

| Component      | Minimum Requirements               |
| -------------- | ---------------------------------- |
| RAM            | 2GB+                               |
| CPU            | Dual-core                          |
| Disk Space     | 10GB+                              |
| Interfaces     | At least 2 NICs for LAN/WAN        |
| Virtualization | VirtualBox / VMware (for test lab) |

---

## 🔹 Installation & Setup

### 1. Install pfSense

* Download pfSense ISO from [official site](https://www.pfsense.org/download/)
* Create VM with 2 NICs (WAN + LAN)
* Boot ISO and complete initial setup

### 2. Configure Interfaces

```bash
# Typical assignments:
WAN → em0 → DHCP or static IP
LAN → em1 → 192.168.1.1/24
```

### 3. Enable DHCP on LAN

* Navigate to **Services → DHCP Server → LAN**
* Set IP Range: `192.168.1.100 - 192.168.1.200`

### 4. Configure DNS Resolver

* Go to **Services → DNS Resolver**
* Ensure it's enabled and **not using external DNS** (to enforce internal policy)

---

## 🔐 URL Blocking with pfBlockerNG

### 1. Install pfBlockerNG

```bash
System → Package Manager → Available Packages → pfBlockerNG-devel → Install
```

### 2. Enable DNSBL

* **Enable pfBlockerNG** under `Firewall → pfBlockerNG → General`
* Go to `DNSBL` tab → Enable → Add domains to blocklist
* Example block list:

```text
chat.openai.com
www.facebook.com
youtube.com
```

### 3. Test DNS Blocking

On **Kali Linux** or test PC:

```bash
nslookup chat.openai.com
# Should return NXDOMAIN or blackhole IP
```

---

## 🔌 Firewall Rule Configuration

### Block All External DNS

```text
Firewall → Rules → LAN
→ Add rule to block: Destination port 53 (UDP/TCP), any IP except pfSense
```

### Allow Internal DNS Only

```text
→ Allow rule for destination = pfSense IP (192.168.1.1), port 53
```

### Allow HTTP/HTTPS

```text
→ Allow port 80 (HTTP) and 443 (HTTPS)
```

---

## 📊 Testing & Validation

| Test Case                             | Result                |
| ------------------------------------- | --------------------- |
| Access ChatGPT via browser            | ❌ Blocked by DNSBL    |
| Access DNS via external server        | ❌ Blocked by firewall |
| Access allowed sites                  | ✅ Success             |
| Switch to external DNS (e.g. 8.8.8.8) | ❌ DNS block enforced  |

---

## 🔒 Security Best Practices

* Always block external DNS (e.g., Google DNS, Cloudflare)
* Use DNS Resolver with forwarding disabled
* Regularly update pfBlockerNG feeds
* Audit firewall logs for evasion attempts

---

## 💡 Future Improvements

* Integrate with **Suricata** for IDS/IPS
* Configure **VPN remote access** with policy-based routing
* Build a **dashboard** using pfSense widgets
* Add logging to remote **SIEM (e.g., Splunk)**

---

## 📄 Files to Upload in GitHub Repo

```
/pfsense-traffic-filter/
├── README.md                      ← This file
├── screenshots/                   ← GUI setup screenshots (pfBlocker, rules, etc.)
├── rules-export.txt               ← pfSense rule backup (optional)
├── dnsbl-blacklist.txt            ← Custom DNSBL domains
├── test-cases.md                  ← Written test cases and results
└── LICENSE                        ← MIT or other license
```

---

## 📢 Badges / Stickers

![pfSense](https://img.shields.io/badge/Firewall-pfSense-blue)
![DNSBL](https://img.shields.io/badge/DNS-DNSBL-red)
![KaliLinux](https://img.shields.io/badge/Test-KaliLinux-lightgrey)
![Automation](https://img.shields.io/badge/Feature-URL_Blocking-success)
![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)

---

## ✨ Author

* Pavana M

> "Secure the edge. Control the flow. Block the threats."

