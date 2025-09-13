# Assignment: DNS Server & DNS Client

## Objective
In this assignment, you will explore the **Domain Name System (DNS)** and gain hands-on experience by writing:
1. A **simple DNS client** in Python to test DNS queries locally and against public servers.
2. A **simple DNS server** in Python to handle queries and demonstrate DNS functionality.

Both the client and server will run on **localhost**, giving you a controlled environment to test DNS functionality, encryption-based data exfiltration, and DNS record handling.

---

## DNS Lab Part 1 – DNS Client

### Part 1.1: Introduction
The first part of the lab helps you understand how a **test client** works:
- A test client sends DNS queries to a server.
- The client verifies whether the response matches the expected IP address.
- You will compare results between:
  - A **local DNS server** (running on your machine).
  - A **public DNS server**.

The provided skeleton code uses the **dnspython** library, which offers both high- and low-level access to DNS queries.

---

### Part 1.2: Writing the Test Client
You will complete the skeleton code (`DNSClient.py`) by filling in the missing pieces (`?`). The main steps are:

1. **Import required modules**:  
   Use `dns.resolver` from the dnspython library.

2. **Define DNS servers and query parameters**:  
   - `local_host` → IP reserved for localhost (`127.0.0.1`)  
   - Public DNS server (e.g., `8.8.8.8` for Google DNS)  
   - `question_type` → Typically `"A"` for IPv4 lookups  

3. **Define query functions**:  
   - `query_local_dns_server(domain)` → Queries the DNS server running locally.  
   - `query_dns_server(domain)` → Queries a real public DNS server.  

4. **Perform DNS resolution**:  
   - Create a `dns.resolver.Resolver()` instance.  
   - Set `resolver.nameservers` to the appropriate server’s IP.  
   - Use `.resolve(domain, question_type)` to get results.  
   - Convert the response to text using `.to_text()`.  

5. **Verification**:  
   - Write a comparison function `compare_dns_servers()` to check if the local and public DNS results match.  

6. **Testing**:  
   Call the functions with multiple domain names to verify correctness.

---

## DNS Lab Part 2 – DNS Server

### Part 2.1: DNS Refresher
DNS is a **distributed database** that maps human-readable names (e.g., `www.example.com`) to IP addresses (e.g., `192.0.2.1`). This allows users to access services without remembering raw IPs.

---

### Part 2.2: Writing the Simple DNS Server
In this section, you will implement a **DNS server** (`DNSServer.py`) using **dnspython** and Python sockets.  

The server must:
1. Import necessary modules (`socket`, `dns.message`, `dns.rrset`, cryptographic libraries).
2. **Implement encryption and decryption**:
   - Functions: `generate_aes_key`, `encrypt_with_aes`, `decrypt_with_aes`.
   - Use AES with Fernet for encrypting/decrypting data.
   - Parameters:
     - Salt = `"Tandon"` (encoded as bytes)  
     - Password = your NYU email address registered in Gradescope  
     - Secret data = `"AlwaysWatching"`  

   The encrypted secret will be stored in a **TXT DNS record** (data exfiltration simulation).

3. **Define DNS records dictionary**:
   Add mappings of hostnames to IPs/records:

   - A Records:
     ```
     safebank.com -> 192.168.1.102
     google.com   -> 192.168.1.103
     legitsite.com -> 192.168.1.104
     yahoo.com    -> 192.168.1.105
     nyu.edu      -> 192.168.1.106
     ```
   - For `nyu.edu`, also create:
     - TXT → encrypted secret (`AlwaysWatching`)
     - MX → `10, mxa-00256a01.gslb.pphosted.com.`
     - AAAA → `2001:0db8:85a3:0000:0000:8a2e:0373:7312`
     - NS → `ns1.nyu.edu.`

4. **Server workflow**:
   - Create a UDP socket (`socket.socket`) and bind it to the local IP and port 53 (DNS standard port).  
   - Use `.recvfrom()` to receive DNS queries.  
   - Parse messages with `dns.message.from_wire()`.  
   - Build responses with `dns.message.make_response()`.  
   - Extract the query name and type.  
   - Match against dictionary records.  
   - Set the **AA (Authoritative Answer)** flag.  
   - Send the response back using `.sendto()`.  
   - Continue processing until quit (`q`) is entered.


---

## Deliverables
Submit the following files to Gradescope:

1. **DNS Client (Part 1)** → `DNSClient.py`  
   - Queries both local and public DNS servers  
   - Compares results  

2. **DNS Server (Part 2)** → `DNSServer.py`  
   - Handles A, AAAA, MX, NS, and TXT records  
   - Demonstrates encrypted TXT record data exfiltration  

---

