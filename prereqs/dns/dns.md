# **DNS Records Cheat Sheet** 📌

DNS records are used to **map domain names** to IP addresses, mail servers, and other resources. Below is a **quick reference guide** for common DNS record types.

---

## **🔹 Common DNS Record Types (Detailed)**

### **1️⃣ A Record (IPv4 Address Mapping)**
- **Maps a domain to an IPv4 address**.
- Used for directing traffic to a web server.
- Example:
  ```txt
  example.com. 3600 IN A 192.168.1.1
  ```
- **TTL:** `3600` seconds (1 hour) (Can be adjusted for faster propagation).
- **Use Case:** Directs users to the correct web server.
- **Additional Example:**
  ```txt
  api.example.com. 7200 IN A 203.0.113.45
  ```
  - Maps `api.example.com` to `203.0.113.45`, commonly used for API endpoints.

---

### **2️⃣ AAAA Record (IPv6 Address Mapping)**
- **Maps a domain to an IPv6 address**.
- Example:
  ```txt
  example.com. 3600 IN AAAA 2001:db8::ff00:42:8329
  ```
- **Use Case:** IPv6 adoption, next-gen internet compatibility.

---

### **3️⃣ CNAME Record (Canonical Name - Alias)**
- **Creates an alias for another domain**.
- Example:
  ```txt
  www.example.com. 3600 IN CNAME example.com.
  ```
- **Use Case:** If the main domain's IP changes, the CNAME automatically updates.
- **Limitation:** Cannot be# **DNS Records Cheat Sheet** 📌

DNS records are used to **map domain names** to IP addresses, mail servers, and other resources. Below is a **comprehensive reference guide** for common DNS record types, public vs. private DNS, cloud-based considerations, and Kubernetes DNS.

---

## **🔹 Common DNS Record Types (Detailed)**

### **1️⃣ A Record (IPv4 Address Mapping)**
- **Maps a domain to an IPv4 address**.
- Used for directing traffic to a web server.
- Example:
  ```txt
  example.com. 3600 IN A 192.168.1.1
  ```
- **TTL:** `3600` seconds (1 hour) (Can be adjusted for faster propagation).
- **Use Case:** Directs users to the correct web server.
- **Additional Example:**
  ```txt
  api.example.com. 7200 IN A 203.0.113.45
  ```
  - Maps `api.example.com` to `203.0.113.45`, commonly used for API endpoints.

---

### **2️⃣ AAAA Record (IPv6 Address Mapping)**
- **Maps a domain to an IPv6 address**.
- Example:
  ```txt
  example.com. 3600 IN AAAA 2001:db8::ff00:42:8329
  ```
- **Use Case:** Ensures IPv6 compatibility for modern networks.

---

### **3️⃣ CNAME Record (Canonical Name - Alias)**
- **Creates an alias for another domain**.
- Example:
  ```txt
  www.example.com. 3600 IN CNAME example.com.
  ```
- **Use Case:** Automatically updates if the main domain's IP changes.
- **Limitation:** Cannot be used for the root domain (`example.com` → Must use an A or ALIAS record).
- **Additional Examples:**
  ```txt
  shop.example.com. 3600 IN CNAME store.example.com.
  mail.example.com. 3600 IN CNAME ghs.googlehosted.com.
  ```
  - `shop.example.com` is an alias for `store.example.com`.
  - `mail.example.com` points to Google’s mail services.

---

### **4️⃣ MX Record (Mail Exchange - Email Routing)**
- **Routes email to mail servers**.
- Example:
  ```txt
  example.com. 3600 IN MX 10 mail.example.com.
  example.com. 3600 IN MX 20 backupmail.example.com.
  ```
- **Priority:** Lower number = Higher priority.
- **Use Case:** Ensures emails are sent to the correct mail server.

---

### **5️⃣ TXT Record (Text - SPF, DKIM, Verification)**
- **Stores arbitrary text**, often used for email validation (SPF, DKIM, domain verification).
- Examples:
  - SPF Record (Prevents email spoofing):
    ```txt
    example.com. 3600 IN TXT "v=spf1 include:_spf.google.com ~all"
    ```
  - DKIM Record (Email authentication):
    ```txt
    default._domainkey.example.com. 3600 IN TXT "v=DKIM1; k=rsa; p=public-key"
    ```
  - Google Site Verification:
    ```txt
    example.com. 3600 IN TXT "google-site-verification=abcd1234"
    ```
- **Use Case:** Email security and domain ownership verification.

---

### **6️⃣ NS Record (Name Server - Delegation)**
- **Specifies authoritative name servers** for a domain.
- Example:
  ```txt
  example.com. 86400 IN NS ns1.provider.com.
  example.com. 86400 IN NS ns2.provider.com.
  ```
- **Use Case:** Directs DNS queries to the right name servers.

---

### **🔹 Public DNS vs. Private DNS**

| Feature | Public DNS | Private DNS |
|---------|-----------|-------------|
| Scope | Accessible over the internet | Only accessible within a private network |
| Use Case | Websites, web services | Internal applications, private cloud |
| Example Providers | Google DNS, Cloudflare, OpenDNS | AWS Private DNS, Azure Private DNS |

- **Public DNS** is used for resolving domain names on the internet.
- **Private DNS** is used in enterprise networks, Kubernetes clusters, and cloud-based environments.

---

### **🔹 DNS in On-Premises vs. Cloud vs. Kubernetes**

#### **On-Premises DNS Considerations**
- Uses internal DNS servers (e.g., Active Directory DNS, BIND)
- Controls name resolution for internal resources
- Requires network segmentation and security policies

#### **Cloud-Based DNS (AWS, Azure, GCP)**
- Managed DNS services: AWS Route 53, Azure DNS, Google Cloud DNS
- Scalable, high availability, and integrates with cloud resources
- Supports private DNS zones for internal networks

#### **Kubernetes DNS (CoreDNS)**
- Automatically resolves internal service names
- Uses `kube-dns` or `CoreDNS` for service discovery
- Example:
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: my-service
  spec:
    clusterIP: 10.0.0.1
    ports:
      - port: 80
  ```
  - `my-service.default.svc.cluster.local` resolves within the cluster.

---

### **🔹 DNS Troubleshooting Commands**
| **Command** | **Purpose** |
|------------|------------|
| `nslookup example.com` | Query DNS for an IP address |
| `dig example.com` | Get detailed DNS records |
| `dig example.com MX` | Check MX records |
| `host example.com` | Simple DNS lookup |
| `whois example.com` | Get domain registration details |

---

### **🔹 Best Practices for DNS**
- Use **low TTL** (`300-600s`) for frequently changing records.
- Use **CNAME** for flexibility, but avoid for root domains.
- Enable **DNSSEC** for security against spoofing.
- Use **split-horizon DNS** for internal vs. external records.
- Monitor **DNS logs** for suspicious activities.

🚀 **Now you have a fully refined DNS cheat sheet with on-prem, cloud, and Kubernetes considerations!** 🚀

 used for the root domain (`example.com` → Must use an A or ALIAS record).
- **Additional Examples:**
  ```txt
  shop.example.com. 3600 IN CNAME store.example.com.
  mail.example.com. 3600 IN CNAME ghs.googlehosted.com.
  ```
  - `shop.example.com` is an alias for `store.example.com`.
  - `mail.example.com` points to Google’s mail services.

---

### **4️⃣ MX Record (Mail Exchange - Email Routing)**
- **Routes email to mail servers**.
- Example:
  ```txt
  example.com. 3600 IN MX 10 mail.example.com.
  ```
  ```txt
  example.com. 3600 IN MX 20 backupmail.example.com.
  ```
- **Priority:** Lower number = Higher priority.
- **Use Case:** Ensures emails are sent to the correct mail server.

---

### **5️⃣ TXT Record (Text - SPF, DKIM, Verification)**
- **Stores arbitrary text**, often used for email validation (SPF, DKIM, domain verification).
- Examples:
  - SPF Record (Prevents email spoofing):
    ```txt
    example.com. 3600 IN TXT "v=spf1 include:_spf.google.com ~all"
    ```
  - DKIM Record (Email authentication):
    ```txt
    default._domainkey.example.com. 3600 IN TXT "v=DKIM1; k=rsa; p=public-key"
    ```
  - Google Site Verification:
    ```txt
    example.com. 3600 IN TXT "google-site-verification=abcd1234"
    ```
- **Use Case:** Email security and domain ownership verification.

---

### **6️⃣ NS Record (Name Server - Delegation)**
- **Specifies authoritative name servers** for a domain.
- Example:
  ```txt
  example.com. 86400 IN NS ns1.provider.com.
  example.com. 86400 IN NS ns2.provider.com.
  ```
- **Use Case:** Directs DNS queries to the right name servers.

---

### **7️⃣ PTR Record (Reverse DNS - IP to Domain Mapping)**
- **Resolves an IP address back to a domain name**.
- Example:
  ```txt
  1.1.168.192.in-addr.arpa. 86400 IN PTR example.com.
  ```
- **Use Case:** Email servers often use PTR records for spam protection.

---

### **8️⃣ SRV Record (Service Location - Protocol Discovery)**
- **Defines location of specific services (e.g., SIP, LDAP)**.
- Example:
  ```txt
  _sip._tcp.example.com. 86400 IN SRV 10 5 5060 sipserver.example.com.
  ```
  - **Priority:** `10` (Lower number = Higher priority).
  - **Weight:** `5` (Load balancing).
  - **Port:** `5060` (SIP service).
  - **Target:** `sipserver.example.com`.
- **Use Case:** Used by applications to discover services dynamically.

---

### **9️⃣ SOA Record (Start of Authority - DNS Zone Info)**
- **Defines primary name server & administrative details for a domain**.
- Example:
  ```txt
  example.com. 86400 IN SOA ns1.provider.com. admin.example.com. (
    2024010101 ; Serial number
    7200       ; Refresh (2 hours)
    3600       ; Retry (1 hour)
    1209600    ; Expire (14 days)
    86400      ; TTL (1 day)
  )
  ```
- **Use Case:** Ensures correct domain ownership and DNS zone configuration.

---

🚀 **Now you have a complete DNS cheat sheet with expanded A and CNAME record examples!** 🚀


---

## **🔹 TTL (Time to Live) Best Practices**
| **Record Type** | **Recommended TTL** |
|---------------|------------------|
| A / AAAA | 3600 (1 hour) |
| CNAME | 3600 (1 hour) |
| MX | 86400 (1 day) |
| TXT | 3600 (1 hour) |
| NS | 86400 (1 day) |
| PTR | 86400 (1 day) |

- Lower TTL (e.g., `300s`) is useful for **frequent updates**.
- Higher TTL (e.g., `86400s`) **reduces query load** and speeds up resolution.

---

## **🔹 DNS Troubleshooting Commands**
| **Command** | **Purpose** |
|------------|------------|
| `nslookup example.com` | Query DNS for an IP address |
| `dig example.com` | Get detailed DNS records |
| `dig example.com MX` | Check MX records |
| `host example.com` | Simple DNS lookup |
| `whois example.com` | Get domain registration details |

---

## **🔹 When to Use Each DNS Record**
| **Need to...** | **Use This Record** |
|---------------|------------------|
| Point domain to an **IPv4 address** | A |
| Point domain to an **IPv6 address** | AAAA |
| Create an **alias (e.g., www → main domain)** | CNAME |
| Set up **email routing** | MX |
| Store **text-based data (SPF, DKIM, verification)** | TXT |
| Define **authoritative name servers** | NS |
| Set up **reverse DNS lookup (IP → Domain)** | PTR |
| Define **service locations (e.g., SIP, LDAP)** | SRV |

---

🚀 **Now you have a complete DNS cheat sheet!** 🚀
