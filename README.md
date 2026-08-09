# 🌐 Enterprise Network Infrastructure & Security Design

A comprehensive Cisco Packet Tracer project demonstrating enterprise-grade routing, switching, traffic filtering, management hardening, and WAN connectivity.

---

## 📌 Project Overview
This project models a multi-segment corporate network designed for high availability, security, and controlled traffic flow. It connects multiple organizational subnets (VLANs 10, 20, 30, and 40) across distinct physical zones, enforcing strict access control lists (ACLs) and border address translation (PAT/NAT Overload) for outbound internet access.

---

## 🛠️ Key Technical Features

### 1. Routing & Subnetting
* **Multi-Subnet Architecture:** Segmented into VLAN 10 (Sales), VLAN 20 (Engineering), VLAN 30 (Servers), and VLAN 40 (HR).
* **OSPF Area 0:** Single-area OSPF deployment providing dynamic multi-router path convergence.
* **DHCP & Relay Agents:** Centralized `Server0` serving dynamic IP leases across all subnets using `ip helper-address`.

### 2. Infrastructure Security
* **Layer 2 Security:** 
  * **Port Security:** Configured on access switches with `maximum 1` and `violation shutdown` to prevent unauthorized device connections.
  * **DHCP Snooping:** Active to block rogue DHCP server attacks.
* **Device Hardening:** 
  * Disabled HTTP/Insecure access; enforced **SSH-only (VTY line)** on all routers and switches using **RSA 1024-bit encryption** and local credentials.
  * Encrypted enable passwords (`service password-encryption` / `enable secret`).

### 3. Traffic Filtering (Access Control List)
* **Extended ACL (ACL 101):** Applied outbound on `Router3` (`Gig0/1`).
  * Explicitly permits **VLAN 20** to access **VLAN 30** (Server side).
  * Explicitly permits **DHCP Relay traffic (UDP port 67)** to prevent IP lease disruption.
  * Blocks **VLAN 10** and **VLAN 40** from reaching **VLAN 30**.
  * Permits standard inter-VLAN routing among non-restricted zones.

### 4. Internet Edge & Address Translation
* **PAT (NAT Overload):** Configured on `MainRouter` (`MRC`) using `ip nat inside source list 1 interface Gig0/0 overload`.
* Translates private internal IP ranges (`10.0.0.0/8`, `20.0.0.0/8`, `30.0.0.0/8`, `40.0.0.0/8`) to a single public IP facing the ISP cloud.
