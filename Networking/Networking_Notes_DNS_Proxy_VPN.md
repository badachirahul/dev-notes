# 🌐 Networking Notes -- DNS, Proxy, VPN (Detailed)

------------------------------------------------------------------------

# 1️⃣ DNS -- Domain Name System

## 📌 Why DNS Exists

Humans remember names (google.com)\
Computers work with IP addresses (142.250.183.14)

DNS converts:

    google.com → 142.250.183.14

------------------------------------------------------------------------

## 🔄 DNS Resolution Process

When you type:

    https://www.randomsite.com

### Step 1: Browser Cache
    If found → Use it → STOP ✅
    If NOT found → Go to Step 2

### Step 2: OS Cache
    If found → Use it → STOP ✅
    If NOT found → Go to Step 3

### Step 3: Recursive Resolver
    ISP DNS or VPN DNS
    Then Root → TLD → Authoritative


Resolver then queries:

1.  Root Server → Who handles .com?
2.  TLD Server (.com) → Who handles randomsite.com?
3.  Authoritative Server → Returns actual IP.

IP is returned to your device.

------------------------------------------------------------------------

## 🔐 DNS Security

Normal DNS: - Not encrypted - ISP can see requested domain

Secure DNS: - DNS over HTTPS (DoH) - DNS over TLS (DoT)

If VPN is ON: - DNS request goes through encrypted tunnel.

------------------------------------------------------------------------

# 2️⃣ Proxy Server

## 📌 What is a Proxy?

A proxy is an intermediary server between your device and the internet.

Flow:

    Device → Proxy → Website

## ✅ What Proxy Does

-   Hides your IP address
-   Works at application level (browser specific)

## ❌ Limitations

-   Usually does NOT encrypt traffic
-   Less secure than VPN
-   Does not protect entire device traffic

Website sees Proxy IP instead of your real IP.

------------------------------------------------------------------------

# 3️⃣ VPN -- Virtual Private Network

## 📌 What is a VPN?

VPN creates an encrypted tunnel between your device and a VPN server.

    Device 🔒 ↔ VPN Server

------------------------------------------------------------------------

## 🔐 How VPN Works

### Step 1: Connection Setup

-   Authentication
-   Encryption protocol starts (WireGuard / OpenVPN)
-   Secure tunnel established

### Step 2: Traffic Flow

Without VPN:

    Device → ISP → Website

With VPN:

    Device → ISP → VPN → Website

------------------------------------------------------------------------

## 🔐 What ISP Can See

ISP can see: - You are connected to VPN - VPN server IP - Data usage -
Connection duration

ISP cannot see: - Websites visited - Data content - Passwords

------------------------------------------------------------------------

## 🔓 Where VPN Encryption Ends

Encryption exists only between:

    Device 🔒 ↔ VPN Server

After reaching VPN: - Traffic is decrypted - Then forwarded to website

------------------------------------------------------------------------

# 4️⃣ VPN + HTTPS (Layered Security)

## Case A: Website uses HTTPS

    Device → (VPN Encrypted) → VPN → (HTTPS Encrypted) → Website

Double encryption: - VPN encryption - HTTPS encryption

Highly secure.

------------------------------------------------------------------------

## Case B: Website uses HTTP

    Device → (VPN Encrypted) → VPN → (Plain Text) → Website

After VPN: - Data is not encrypted - VPN provider can see content

------------------------------------------------------------------------

# 5️⃣ Private IP vs Private Network

## Private IP

Used inside local networks: - 192.168.x.x - 10.x.x.x - 172.16--31.x.x

Not directly accessible from internet.

## Private Network

Restricted network environment.

VPN creates a virtual private network, not just a private IP.

------------------------------------------------------------------------

# 🎯 Final Core Concepts

-   DNS converts domain → IP
-   Proxy hides IP (usually no encryption)
-   VPN hides IP + encrypts traffic
-   VPN encryption ends at VPN server
-   HTTPS protects data beyond VPN
-   ISP sees tunnel, not inside data
-   VPN hides IP, not identity
