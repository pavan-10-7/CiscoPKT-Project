# CiscoPKT-Project
## 📖 Overview  
This project demonstrates the design and implementation of a router configured as both **DHCP** and **DNS server** in a multi-subnet environment using **Cisco Packet Tracer**.  
The goal is to enable dynamic IP assignment, DNS resolution, and seamless inter-subnet communication.

---

## ⚙️ Technologies Implemented
- Router configured as **DHCP Server**
- Router configured as **DNS Server**
- Multiple Subnet Design with routing
- Static IPv4 addressing for servers
- Dynamic IP allocation for PCs
- End-to-end connectivity verification using `ping`

---

## 🖥️ Network Topology
![Network Topology](./topology.png)  
*(Replace with your actual diagram image)*

---

## 📋 Features
- [x] DHCP Server assigns IP addresses automatically  
- [x] DNS Server resolves domain names to IPs  
- [x] Multiple subnets connected via router interfaces  
- [ ] NAT/PAT configuration (future enhancement)  
- [ ] ACLs for network security (future enhancement)  

---

## 🗂️ IP Addressing Table  

| Device      | Interface | IP Address     | Subnet Mask   | Notes                |
|-------------|-----------|----------------|---------------|----------------------|
| Router GA   | G0/0      | 192.168.1.1    | 255.255.255.0 | Subnet A Gateway     |
| Router GA   | G0/1      | 192.168.2.1    | 255.255.255.0 | Subnet B Gateway     |
| DNS Server  | NIC       | 192.168.1.10   | 255.255.255.0 | Static Configuration |
| PC1         | NIC       | DHCP Assigned  | 255.255.255.0 | Subnet A             |
| PC2         | NIC       | DHCP Assigned  | 255.255.255.0 | Subnet B             |

---

## 💻 How to Run
```bash
# Clone this repository
git clone https://github.com/pavan-10-7/CiscoPKT-Project.git
```
---

Below is the enterprise network topology implemented in this project:
<img width="757" height="532" alt="image" src="https://github.com/user-attachments/assets/ba82ad34-f6d9-425c-b609-cc88fe2dc9f5" />

