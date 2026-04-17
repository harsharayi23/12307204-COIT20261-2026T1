# Task 1: Resolving IP Addresses to Hardware Addresses (ARP)

## Aim
To understand how ARP (Address Resolution Protocol) maps IP addresses to MAC (hardware) addresses in a Local Area Network (LAN).

---

##  Setup
- Project Name: `ARP-Basics-<studentid>`
- Devices Used:
  - 4 Linux Hosts (Host A, Host B, Host C, Host D)
  - 1 Ethernet Switch
- Ensure all hosts are assigned IP addresses.

---

##  Steps

### 1. View ARP Table on Host A
Run the following command on Host A:

```bash
ip neigh show
```
### 2. Ping Host B from Host A
ping <HostB-IP>
### 3. View ARP Table Again on Host A
ip neigh show
#### Observation:

A new entry for Host B will appear
It shows:
IP address
MAC address
State (REACHABLE or stale)
### 4. Ping Host A from Host C
ping <HostA-IP>
### 5. View ARP Table Again on Host A
ip neigh show
#### Observation:

Additional entries are added
ARP table updates dynamically as communication occurs
#### Explanation
ARP (Address Resolution Protocol) is used to map an IP address to a MAC address.
When a device wants to communicate:
It sends an ARP request
The destination replies with its MAC address
The mapping is stored in the ARP table
Entries may expire if not used for some time.
### Output
![gns](./images/week6-12307204.01.png)

# Task 2: Default Gateways & Static Routing

##  Aim
To configure default gateways and enable communication between different subnets using static routing.

---

##  Network Setup
- Project Name: `Default-Gateway-<studentid>`

### Devices:
- 4 Linux Hosts (Host A, Host B, Host C, Host D)
- 2 Switches
- 2 Routers

---

##  IP Addressing Scheme

| Device   | Interface | IP Address     |
|----------|----------|----------------|
| Host A   | eth0     | 10.10.10.4/24  |
| Host B   | eth0     | 10.10.10.5/24  |
| Router 1 | eth0     | 10.10.10.1/24  |
| Router 1 | eth1     | 10.10.11.1/24  |
| Router 2 | eth0     | 10.10.12.1/24  |
| Router 2 | eth1     | 10.10.11.2/24  |
| Host C   | eth0     | 10.10.12.6/24  |
| Host D   | eth0     | 10.10.12.7/24  |

---

##  Configuration

###  Host Configuration

#### Host A
```bash
auto eth0
iface eth0 inet static
   address 10.10.10.4
   netmask 255.255.255.0
   gateway 10.10.10.1
   up sysctl net.ipv4.ip_forward=0
```
#### Host B
auto eth0
iface eth0 inet static
   address 10.10.10.5
   netmask 255.255.255.0
   gateway 10.10.10.1
   up sysctl net.ipv4.ip_forward=0
#### Host C
auto eth0
iface eth0 inet static
   address 10.10.12.6
   netmask 255.255.255.0
   gateway 10.10.12.1
   up sysctl net.ipv4.ip_forward=0 
#### Host D
auto eth0
iface eth0 inet static
   address 10.10.12.7
   netmask 255.255.255.0
   gateway 10.10.12.1
   up sysctl net.ipv4.ip_forward=0
#### Router Configuration
Router 1
auto eth0
iface eth0 inet static
   address 10.10.10.1
   netmask 255.255.255.0

auto eth1
iface eth1 inet static
   address 10.10.11.1
   netmask 255.255.255.0
   up sysctl net.ipv4.ip_forward=1   
#### Router 2
auto eth0
iface eth0 inet static
   address 10.10.12.1
   netmask 255.255.255.0

auto eth1
iface eth1 inet static
   address 10.10.11.2
   netmask 255.255.255.0
   up sysctl net.ipv4.ip_forward=1 
### Verify Routing Table
ip route show

### Key Concepts
Default Gateway connects host to other networks
Routers forward packets between subnets
Static routes define manual paths
IP forwarding must be enabled on routers
## Outputs
![gns](./images/week6-12307204.11.png)
![gns](./images/week6-12307204.12.png)
![gns](./images/week6-12307204.13.png)
![gns](./images/week6-12307204.14.png)
![gns](./images/week6-12307204.15.png)
![gns](./images/week6-12307204.16.png)
![gns](./images/week6-12307204.17.png)
### Connectivity Test

From Host A:

ping 10.10.12.6

![gns](./images/week6-12307204.18.png)
