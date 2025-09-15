# DNS Server & Client Project

## 📌 Overview
This project demonstrates the fundamentals of the **Domain Name System (DNS)** by implementing both a **DNS client** and a **DNS server** in Python.  

It provides hands-on experience with:
- Querying DNS records locally and from public servers
- Handling and parsing raw DNS packets
- Serving custom DNS records from a local server
- Simulating **encryption-based data exfiltration** through DNS TXT records  

The goal is to highlight a blend of **network security concepts, Python programming, and cryptographic techniques**.

---

## ⚙️ Tools, Languages & Technologies
- **Programming Language**: Python  
- **Libraries & Modules**:
  - [dnspython](https://www.dnspython.org/) → DNS client/server operations
  - `socket` → low-level UDP communication
  - `cryptography` (Fernet, AES) → encrypted DNS TXT record simulation
  - `json`, `base64`, `hashlib` → data formatting & security  
- **Protocols**: DNS, UDP/IP  
- **Testing Tools**:
  - `dig` and `nslookup` for DNS lookups
  - Wireshark for packet inspection  

---

## ✨ Features

### 🖥️ DNS Client
- Queries **both a local DNS server** (localhost) and **public DNS servers** (e.g., Google 8.8.8.8).
- Resolves multiple record types (A, AAAA, MX, TXT, NS).
- Compares responses between local and public DNS servers.
- Outputs results in human-readable format.

### 🌐 DNS Server
- Implements a simple **authoritative DNS server** on `localhost:53`.
- Supports multiple DNS record types:
  - **A Records**: `safebank.com`, `google.com`, `legitsite.com`, `yahoo.com`, `nyu.edu`
  - **AAAA Record**: IPv6 for `nyu.edu`
  - **MX Record**: Mail exchange for `nyu.edu`
  - **NS Record**: Nameserver for `nyu.edu`
  - **TXT Record**: Stores **encrypted secret** (AES/Fernet) to simulate DNS-based exfiltration
- Encrypts sensitive data (`AlwaysWatching`) using AES + Fernet with:
  - Salt = `"Tandon"`
  - Password = NYU email address
- Responds to queries using dnspython’s `dns.message` and `dns.rrset`.

---

## 🔍 Background
DNS is a **distributed naming system** that maps hostnames (e.g., `www.google.com`) to IP addresses. It is essential to Internet functionality but also frequently abused for:
- **DNS spoofing**
- **DNS tunneling for data exfiltration**
- **DNS amplification DDoS**

This project illustrates both **legitimate DNS operations** and a **security use case** (encrypted TXT record exfiltration), making it valuable for cybersecurity testing and education.

---

## 🚀 Quickstart

### 1. Run the DNS Server
```bash
python DNSServer.py
```
### 2. Run the DNS Client
```bash
python DNSClient.py
```
### 3. Example Query
```bash
dig @127.0.0.1 nyu.edu A
```
The server will respond with the locally configured IP address for nyu.edu.
TXT record queries will return an encrypted payload (AlwaysWatching) to simulate covert exfiltration.

### 📜 License
MIT © 2025 Ayushi Shah
