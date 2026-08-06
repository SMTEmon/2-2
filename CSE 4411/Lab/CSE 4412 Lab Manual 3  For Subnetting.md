
> [!info] Lab Overview
> The lab focuses on using Cisco Packet Tracer for graphical network simulation and Python for network automation, data analysis, and scripting.

## Lab 01: Exploring a Basic Network Topology

> [!abstract] Objective
> Identify edge/core devices, build a LAN topology, configure IP addressing, and establish routing.

### Router Connection Methods
* **Ethernet (Recommended):** Uses a Copper Straight-Through cable and requires no additional modules.
* **Serial:** Requires powering off the router, inserting an HWIC-2T serial module, and connecting via a DCE/DTE cable.

### Key IOS Commands (Interface & Routing)
* Assign IP: `ip address 192.168.1.1 255.255.255.0`
* Bring interface up: `no shutdown`
* Save Configuration: `copy running-config startup-config`

#### Configure RIP (Routing Information Protocol)
```bash
router rip           # Enables RIP
version 2            # Uses classless RIPv2
network 192.168.1.0  # Advertises the specific LAN
```

## Lab 02: Socket Programming

> [!abstract] Objective
> Implement UDP and TCP client-server applications, handle concurrency, and observe connection states.

### UDP Implementation (Connectionless)
* **Socket Creation:** Uses `socket.AF_INET` for IPv4 and `socket.SOCK_DGRAM` for UDP.
* **Server Binding:** Binding to `'0.0.0.0'` makes the server reachable from any attached network.
* **Interaction:** Relies on `sock.recvfrom(1024)` to wait for data and `sock.sendto(data, addr)` to send echoes.

### TCP Implementation (Connection-Oriented)
* **Socket Creation:** Uses `socket.AF_INET` and `socket.SOCK_STREAM` for TCP.
* **Server Setup:** Requires `server.listen(5)` to queue connections and `conn, addr = server.accept()` to block and wait for an incoming connection.
* **Interaction:** Uses `conn.recv(1024)` to read bytes and `conn.sendall(data)` to return them.

### Concurrency (Multithreading)
* Python enables concurrent client handling by spawning a new thread for each request:
```python
threading.Thread(target=handle_request, args=(data, addr), daemon=True).start()
```

### Observing Network States
* TCP states like `ESTABLISHED`, `TIME_WAIT`, or `CLOSE_WAIT` can be observed in the terminal.
* Windows command: `netstat -an | findstr :6006`
* Linux command: `ss -tn sport = :6006`

## Lab 03: IPv4 Subnetting and Address Planning

### Subnetting Formulas
* **Number of subnets:** $2^{\Delta}$, where $\Delta$ is the number of borrowed bits.
* **Hosts per subnet:** $2^{\text{host bits}} - 2$ (the first address is the network address and the last is the broadcast address).

### Variable Length Subnet Mask (VLSM) Design Tables
*(Based on the 204.15.5.0/24 address block)*

#### Host Requirements Table

| Subnet | Required Hosts |
| ------ | -------------- |
| net A  | 14             |
| net B  | 28             |
| net C  | 2              |
| net D  | 7              |
| net E  | 28             |

#### Resulting VLSM Plan Table

| Subnet | Network Mask | Usable Hosts Range |
| ------ | ------------ | ------------------ |
| net B | `204.15.5.0 /27` (255.255.255.224) | 204.15.5.1 - 204.15.5.30 |
| net E | `204.15.5.32 /27` | 204.15.5.33 - 204.15.5.62 |
| net A | `204.15.5.64 /28` (255.255.255.240) | 204.15.5.65 - 204.15.5.78 |
| net D | `204.15.5.80 /28` | 204.15.5.81 - 204.15.5.94 |
| net C | `204.15.5.96 /30` (255.255.255.252) | 204.15.5.97 - 204.15.5.98 |

### Classless Routing and RIPv2 Configuration
* **Why RIP Version 2?** RIPv1 only supports classful routing. RIPv2 is required for VLSM because it supports classless routing and includes subnet mask data in its updates.
* **Why disable auto-summary?** By default, RIP summarizes routes to their classful boundaries. Using the `no auto-summary` command ensures each subnet is advertised with its exact mask, allowing accurate routing between discontiguous networks.
* **Verification Commands:** 
	* `show ip protocols` to check if active
	* `show ip route` to verify routes
	* `debug ip rip` to monitor live updates
