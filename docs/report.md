# Router as DHCP & DNS with Multiple Subnets Design & Implementation Report

## 1. Project Overview

This project demonstrates how to configure a single router to provide DHCP services for two separate IPv4 subnets and how to configure a dedicated DNS server that services both subnets. The implementation is done in Cisco Packet Tracer. All client hosts in both subnets obtain IP configuration (IP, gateway, DNS) automatically, can communicate across subnets via the router, and can resolve a domain name hosted on the DNS server.


## 2. Tools & Requirements

Software: Cisco Packet Tracer (v7.x / v8.x recommended)

Devices: 1 Router (e.g., 2811), 2 Switches (2960), 4 PCs, 1 Server (Packet Tracer Server)

Cables: Copper straight-through for PC→Switch, Switch→Router links

Knowledge: Basic networking (IP addressing, subnetting, router CLI)

## 3. Topology (logical)
```
   PC1 ---\
           Switch0 --- Router G0/0 --- (Subnet A: 192.168.1.0/24)
   PC2 ---/                              |
                                         |
                                         +--- Server (DNS) 192.168.1.10
                                         |
   PC3 ---\                              |
           Switch1 --- Router G0/1 --- (Subnet B: 192.168.2.0/24)
   PC4 ---/
```

<img width="757" height="532" alt="network-diagram" src="https://github.com/user-attachments/assets/8a83e207-2830-465c-ad7a-f9aefb125f4e" />


## 4. IP Addressing Plan

|Device / Role |	Interface / Hostname |	IP Address	  |     Mask      |	        Notes          |
|--------------|-----------------------|----------------|---------------|-------------------------| 
| Router G0/0  |	Subnet A gateway     |	192.168.1.1	  | 255.255.255.0 | DHCP default-router A   | 
|Router G0/1	|Subnet B gateway	      |192.168.2.1	  |255.255.255.0  |	DHCP default-router B  |
|DNS Server	   |Server (Subnet A)	   |192.168.1.10	  |255.255.255.0  |	DNS A record for domain|
|PC1         	|Client A	            |DHCP allocated  |255.255.255.0	|Example: 192.168.1.11    |
|PC2	         |Client A	            |DHCP allocated  |255.255.255.0	|Example: 192.168.1.12    |
|PC3        	|Client B	            |DHCP allocated  |255.255.255.0	|Example: 192.168.2.11    |
|PC4        	|Client B	            |DHCP allocated  |255.255.255.0	|Example: 192.168.2.12    |


## 5. Configuration Steps (detailed)
### 5.1 Router interface configuration
```text
Open Router → CLI and enter:

enable
configure terminal

interface GigabitEthernet0/0
 description Subnet-A
 ip address 192.168.1.1 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/1
 description Subnet-B
 ip address 192.168.2.1 255.255.255.0
 no shutdown
exit

end
write memory
```
### 5.2 Exclude addresses from DHCP pools

Prevent the router DHCP from assigning the router and server addresses:
```text
configure terminal
ip dhcp excluded-address 192.168.1.1 192.168.1.9
ip dhcp excluded-address 192.168.2.1 192.168.2.9
exit
write memory

7.3 Create DHCP pools on the router
configure terminal

ip dhcp pool SUBNET_A
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 192.168.1.10
 domain-name rnsit.local
exit

ip dhcp pool SUBNET_B
 network 192.168.2.0 255.255.255.0
 default-router 192.168.2.1
 dns-server 192.168.1.10
 domain-name rnsit.local
exit

end
write memory
```
### 5.3 Configure the DNS Server (Packet Tracer Server)

1. Click the Server device → Config (or Services) tab.

2. Under IP Configuration set static IP:

  - IP: 192.168.1.10

  - Subnet: 255.255.255.0

  - Default Gateway: 192.168.1.1

  - Under Services → DNS:

3. Set DNS to ON.

4. Add an A record:

  - Name: www.rnsit.com

  - IP: 192.168.1.10

  - Optionally enable HTTP to serve a simple web page.

### 5.4 Configure PCs to use DHCP

For each PC:

1. Desktop → IP Configuration → choose DHCP → wait for lease.


## 6. Conclusion

The project successfully shows how a single router can be used to provide DHCP services across multiple subnets and how a dedicated DNS server can be centrally used by clients on different networks. This set-up is commonly used in small enterprise and lab environments where a single routing device serves multiple VLANs or physical networks, while centralized services (DNS/DHCP) are hosted on servers.
