### **3.2 DNS Basics**

#### **Domain Name System (DNS)**

The **Domain Name System (DNS)** is a hierarchical, decentralized naming system for devices connected to the internet or private networks. DNS translates human-readable domain names like `www.example.com` into machine-readable IP addresses like `192.0.2.1`, allowing users to access websites without needing to remember complex IP addresses.

- **Example**: When you type `www.google.com` into your browser, DNS translates this domain name into an IP address (e.g., `142.250.190.14`) that identifies Google’s server on the internet. Your browser then connects to that server to load the website.

---

### **How DNS Works**:

1. **User Query**: A user types a domain name, such as `www.example.com`, into their web browser.
2. **DNS Resolver**: The request is sent to a **DNS resolver**, typically provided by the user's ISP, which starts the resolution process.
3. **Root DNS Server**: The resolver queries one of the **root DNS servers** to find out where to look for information about the `.com` domain.
4. **TLD DNS Server**: The resolver is directed to a **Top-Level Domain (TLD)** DNS server (in this case, a `.com` TLD server) to find out where to look for `example.com`.
5. **Authoritative DNS Server**: The TLD server directs the resolver to the **authoritative DNS server** for `example.com`, which stores the actual IP address associated with the domain.
6. **IP Address Returned**: The authoritative DNS server provides the IP address (e.g., `192.0.2.1`) for `www.example.com`.
7. **User Connects to Website**: The resolver returns the IP address to the user’s browser, which then connects to the website’s server and displays the web page.

---

### **DNS Records**

DNS uses different types of records to store information about domain names and their corresponding IP addresses. Each record has a specific function in directing internet traffic.

#### **1. A Record (Address Record)**:
- **Purpose**: Maps a domain name to an IPv4 address.
- **Example**: If the domain `www.example.com` has an IPv4 address of `192.0.2.1`, the DNS A record will store this information.
  - **Record Format**:
    ```
    www.example.com.  IN  A  192.0.2.1
    ```
  - **Explanation**: When a user types `www.example.com`, DNS will resolve it to the IP address `192.0.2.1`.

#### **2. CNAME Record (Canonical Name Record)**:
- **Purpose**: Maps a domain name to another domain name (aliasing). It’s useful when you want multiple domain names to point to the same IP address or resource.
- **Example**: If `blog.example.com` is an alias for `www.example.com`, a CNAME record would be used.
  - **Record Format**:
    ```
    blog.example.com.  IN  CNAME  www.example.com.
    ```
  - **Explanation**: When a user types `blog.example.com`, DNS will resolve it to `www.example.com`, which then resolves to the IP address associated with `www.example.com`.

#### **3. MX Record (Mail Exchange Record)**:
- **Purpose**: Specifies the mail servers responsible for receiving emails for a domain.
- **Example**: If emails for `example.com` are handled by mail servers at `mail1.example.com` and `mail2.example.com`, MX records will be used to prioritize the mail servers.
  - **Record Format**:
    ```
    example.com.  IN  MX  10  mail1.example.com.
    example.com.  IN  MX  20  mail2.example.com.
    ```
  - **Explanation**: Emails sent to `user@example.com` will first attempt to be delivered to `mail1.example.com`. If that fails, it will try `mail2.example.com`.

#### **4. AAAA Record (IPv6 Address Record)**:
- **Purpose**: Maps a domain name to an IPv6 address.
- **Example**: If `www.example.com` has an IPv6 address of `2001:db8::1`, the DNS AAAA record will store this information.
  - **Record Format**:
    ```
    www.example.com.  IN  AAAA  2001:db8::1
    ```
  - **Explanation**: When a user types `www.example.com`, DNS will resolve it to the IPv6 address `2001:db8::1`.

#### **5. NS Record (Name Server Record)**:
- **Purpose**: Specifies the authoritative DNS servers for a domain. These are the servers that respond to queries about the domain.
- **Example**: If `example.com` is managed by the DNS servers `ns1.example.com` and `ns2.example.com`, NS records will be used.
  - **Record Format**:
    ```
    example.com.  IN  NS  ns1.example.com.
    example.com.  IN  NS  ns2.example.com.
    ```

#### **6. TXT Record (Text Record)**:
- **Purpose**: Allows domain owners to store arbitrary text data in DNS records. This is often used for verification purposes or to provide information for services like SPF (Sender Policy Framework) and DKIM (DomainKeys Identified Mail) to help reduce email spam.
- **Example**: A TXT record can include an SPF rule to help verify the authenticity of emails sent from `example.com`.
  - **Record Format**:
    ```
    example.com.  IN  TXT  "v=spf1 include:_spf.example.com ~all"
    ```

---

### **Example of DNS Records for a Domain**

Let’s say the domain `example.com` has the following DNS records:

| Type | Host                | Value                             | Description                                                |
|------|---------------------|-----------------------------------|------------------------------------------------------------|
| **A**    | `example.com`         | `192.0.2.1`                        | IPv4 address of the website.                                |
| **AAAA** | `example.com`         | `2001:db8::1`                      | IPv6 address of the website.                                |
| **CNAME**| `blog.example.com`    | `www.example.com`                  | Blog is an alias for the main site.                         |
| **MX**   | `example.com`         | `mail1.example.com (priority 10)`  | Primary mail server.                                        |
|          |                       | `mail2.example.com (priority 20)`  | Backup mail server.                                         |
| **NS**   | `example.com`         | `ns1.example.com`                  | Primary name server.                                        |
|          |                       | `ns2.example.com`                  | Secondary name server.                                      |
| **TXT**  | `example.com`         | `"v=spf1 include:_spf.example.com ~all"` | SPF record for email authentication.                     |

### **How DNS Records Are Used in Practice**

For instance, if a user types `blog.example.com` in a browser:
1. **CNAME Record**: DNS resolves `blog.example.com` to `www.example.com` because of the CNAME record.
2. **A Record**: DNS then looks up the A record for `www.example.com`, finding the IP address `192.0.2.1`.
3. **Connection Established**: The browser connects to `192.0.2.1`, loading the website.
