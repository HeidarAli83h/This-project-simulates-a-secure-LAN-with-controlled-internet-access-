# 🌐 Network Configuration Project

A practical Cisco Packet Tracer lab featuring **NAT**, **ACL**, and **secure routing** between two routers, a switch, and three PCs. This project demonstrates how to restrict and allow internet access using ACLs and NAT while maintaining full LAN connectivity. 🔐

---

## 📌 Overview

This project simulates a secure LAN with controlled internet access:

- **R1**: Acts as the gateway for the LAN and enforces **Access Control Lists (ACLs)**.
- **R2**: Performs **NAT** to simulate internet access.
- **Switch**: Connects all three PCs to R1.
- **PC1 & PC2**: Have full internet access.
- **PC3**: LAN-only (internet access blocked).

---

## 🗺️ Network Topology

[PC1]      [PC2]      [PC3]
   |          |          |
  +-----------+----------+
         🖧 Switch
             |
          [R1] 🛡️
 (LAN: 192.168.10.1)
 (WAN: 10.0.0.1)
             |
          [R2] 🌍
(LAN: 10.0.0.2, NAT: 203.0.113.2)


---

## 📍 IP Addressing Table

| Device | Interface | IP Address     | Subnet | Role                  |
|--------|-----------|----------------|--------|-----------------------|
| PC1    | NIC       | 192.168.10.10  | /24    | ✅ Internet Allowed    |
| PC2    | NIC       | 192.168.10.20  | /24    | ✅ Internet Allowed    |
| PC3    | NIC       | 192.168.10.30  | /24    | ❌ Internet Blocked    |
| R1     | G0/0      | 192.168.10.1   | /24    | 🏠 LAN Gateway         |
| R1     | G0/1      | 10.0.0.1       | /30    | 🔗 Link to R2          |
| R2     | G0/0      | 10.0.0.2       | /30    | 🌐 Simulated Internet  |
| R2     | G0/1      | 203.0.113.2    | /24    | 🌍 NAT/Public IP       |

---

## ⚙️ Configurations

### 🛡️ R1 - ACL Gateway

**Interfaces**  
- `G0/0`: 192.168.10.1/24 (LAN)  
- `G0/1`: 10.0.0.1/30 (WAN to R2)

**Routing**  
- `ip route 0.0.0.0 0.0.0.0 10.0.0.2`

**ACL**  
- Permit: `192.168.10.10`, `192.168.10.20` to any  
- Deny: everything else  
- Applied outbound on `G0/1`

📄 File: [`R1-config-ACL.txt`](R1-config-ACL.txt)

---

### 🌍 R2 - NAT Router

**Interfaces**  
- `G0/0`: 10.0.0.2/30 (from R1)  
- `G0/1`: 203.0.113.2/24 (simulated public)

**Routing**  
- `ip route 192.168.10.0 255.255.255.0 10.0.0.1`

**NAT Configuration**  
- ACL 1: `permit 192.168.10.0 0.0.0.255`  
- NAT inside: `G0/0`  
- NAT outside: `G0/1`  
- `ip nat inside source list 1 interface G0/1 overload`

📄 File: [`R2-config-NAT.txt`](R2-config-NAT.txt)

---

## 🧪 Test Results

📄 File: [`Test.md`](Test.md)

| Test                          | PC1 | PC2 | PC3 |
|------------------------------|-----|-----|-----|
| 🌍 Ping to `8.8.8.8`         | ✅  | ✅  | ❌  |
| 🏠 Ping to `192.168.10.x`    | ✅  | ✅  | ✅  |

- ✅ **Internet Access**: Only PC1 and PC2 are allowed.  
- ✅ **LAN Connectivity**: All devices can communicate locally.

---
## 📝 Setup Instructions

### 1. Apply Configurations

- Load `R1-config-ACL.txt` on **Router R1**.
- Load `R2-config-NAT.txt` on **Router R2**.

### 2. Connect Devices

- Use the network topology diagram above to connect all devices properly via the switch and routers.

### 3. Test Connectivity

- From each PC, run ping tests to:
  - **8.8.8.8** (for internet access)
  - Other PCs within `192.168.10.0/24` (for LAN)
- Refer to [`Test.md`](Test.md) to validate results.

---

## 🔍 Troubleshooting

### ✅ Connectivity Issues

- Verify **IP addresses**, **subnet masks**, and **default gateways**.
- Ensure ACL on **R1** correctly permits or denies the intended IPs.
- Double-check **NAT configuration** on **R2** for accurate translation.

### 🔌 Physical Connections

- Make sure switch and router interfaces are correctly wired.
- Use `show ip interface brief` to validate interface status.

### 📡 Routing

- On **R1**: Ensure default route to `10.0.0.2` exists.
- On **R2**: Add static route to `192.168.10.0/24` via `10.0.0.1`.

---

## 🤝 Contributing

Contributions are welcome! 🙌

- Open an issue for bugs, questions, or improvements.
- Submit a pull request for fixes or enhancements.
- Follow consistent formatting and test your changes in a simulated lab.

---

## 📜 License

This project is licensed under the **MIT License**.  
See the [`LICENSE`](LICENSE) file for more details.

---

## 🌟 Acknowledgments

- Built and tested in a controlled **Cisco Packet Tracer** environment.
- Inspired by **real-world networking practices** involving ACLs and NAT.
- Created with ❤️HeidarAli>  by a networking enthusiast.

> ⭐ **Star this repo** if you find it helpful.  
> 📧 For questions, open an issue on GitHub.


