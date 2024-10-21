
# VLAN Network Security Project - Documentation

## Project Overview
This project demonstrates how VLAN segmentation enhances network security by isolating traffic and restricting unauthorized communication between network nodes. The experiment involves running Nmap host scans, setting up an HTTP server, and testing connectivity between different VLANs to showcase the security benefits of VLAN segmentation.

---

## Experiment Steps

### 1. Nmap Host Scan from the Client Node
To identify all active hosts on the network, a Nmap scan was performed from the Client Node targeting the 10.0.1.0/24 subnet. The command used:

```bash
nmap 10.0.1.0/24 -sn
```

The result of the scan showed that four hosts were active within the subnet:
- **Client**: 10.0.1.22
- **Server**: 10.0.1.20
- **Attacker 1**: 10.0.1.23
- **Attacker 2**: 10.0.1.21

MAC addresses were also displayed to confirm the presence of these hosts.

### 2. Nmap Host Scan from Attacker 1 Node
Following the scan from the Client node, a Nmap host scan was repeated from the Attacker 1 node using the same command:

```bash
nmap 10.0.1.0/24 -sn
```

This scan successfully identified the same four reachable hosts in the 10.0.1.0/24 subnet, confirming that Attacker 1 had full visibility of the network similar to the Client.

### 3. Running the HTTP Server on the Server Node
The HTTP server script was started on the Server node using the following command:

```bash
./start.sh
```

This successfully launched the HTTP server, which began listening on `10.0.1.20` at port `8000`.

### 4. Running the run_curl.sh Script on the Client Node
After starting the HTTP server, the `run_curl.sh` script was run on the Client Node to interact with the server. The command used was:

```bash
./run_curl.sh
```

The response from the server confirmed a successful connection with the message `<b>Legit Server</b>`, showing that the client connected to the server at `10.0.1.20:8000`.

### 5. Running the run_curl.sh Script from the Attacker 1 Node
To test if Attacker 1 could access the HTTP server, the `run_curl.sh` script was run from Attacker 1 using the following command:

```bash
./run_curl.sh
```

The script successfully connected to the server and returned the same message as the Client node, indicating that Attacker 1 had the same access to the HTTP server.

---

## VLAN Configuration

### 1. Removing IP Address Assignments
For each node (Server, Client, Attacker 1, and Attacker 2), all existing IP address assignments were removed. This was done to ensure a clean network configuration for the VLAN setup.

### 2. Adding VLAN Interfaces to Nodes
VLAN interfaces were created on the Client, Server, Attacker 1, and Attacker 2 nodes using the following command:

```bash
ip link add link eth0 name eth0.<vlan> type vlan id <vlan_id>
```

- **VLAN 10**: Assigned to the Client and Server nodes
- **VLAN 20**: Assigned to Attacker 1 and Attacker 2 nodes

### 3. Assigning IP Addresses to the VLAN Interfaces
IP addresses were assigned to the newly created VLAN interfaces using the following command:

```bash
ifconfig <vlan> <ip_address>/24 up
```

Each node was assigned the corresponding IP address for its VLAN, and the configuration was verified with the `ifconfig` command.

---

## Post-VLAN Configuration Testing

### 1. Nmap Host Scan from Client Node After VLAN Configuration
After the VLAN interfaces were configured and IP addresses assigned, a Nmap host scan was performed from the Client Node targeting the 10.0.1.0/24 subnet. The command used:

```bash
nmap -sn 10.0.1.0/24
```

The scan detected only 2 hosts in the network, confirming the expected VLAN behavior, as the Client and Server were now isolated in VLAN 10.

### 2. Nmap Host Scan from Attacker 1 After VLAN Configuration
Similarly, a Nmap scan was repeated from Attacker 1. The command used:

```bash
nmap -sn 10.0.1.0/24
```

Attacker 1 was only able to detect Attacker 2, which is in the same VLAN (VLAN 20). Neither the Client nor Server nodes were visible, demonstrating the isolation provided by the VLANs.

---

## Further Testing with HTTP Server

### 1. Running the HTTP Server on the Server Node
The HTTP server was once again started on the Server node using the following command:

```bash
./start.sh
```

The server began listening on `10.0.1.20:8000` within VLAN 10.

### 2. Running the run_curl.sh Script from Client Node
After starting the server, the `run_curl.sh` script was run from the Client node:

```bash
./run_curl.sh
```

The connection was successful, with the message "Legit Server" returned from the server, as expected since both nodes are within the same VLAN.

### 3. Running the run_curl.sh Script from Attacker 1 Node
The script was then run from Attacker 1:

```bash
./run_curl.sh
```

The connection failed with the message "Failed to connect: No route to host". This was because Attacker 1 and the Server are in different VLANs (VLAN 20 and VLAN 10, respectively), and no routing was configured between these VLANs.

---

## Conclusion
This project demonstrates the effectiveness of VLANs in segmenting network traffic and enhancing security. The VLAN setup isolated the Client and Server in VLAN 10, while Attacker 1 and Attacker 2 were placed in VLAN 20. This isolation prevented the attackers from accessing devices in the other VLAN, illustrating how VLANs can be used to control and secure network communication.
