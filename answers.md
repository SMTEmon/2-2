		ID: 230042104

x = 04
y = 05
B = 104
K = 20

# Q1
### A
Lan - Network - Prefix - dotted mask - fisrt/last usable - broadcast

| Lan    | Network       | Prefix     | dotted mask     | fisrt/last usable             | broadcast     |
| ------ | ------------- | ---------- | --------------- | ----------------------------- | ------------- |
| Lan- 1 | 10.104.60.0   | 10.104.60. | 255.255.255.192 | 10.104.60.1 - 10.104.60.62    | 10.104.60.63  |
| Lan- 2 | 10.104.60.64  | 10.104.60. | 255.255.255.224 | 10.104.60.65 - 10.104.60.94   | 10.104.60.95  |
| Lan- 3 | 10.104.60.96  | 10.104.60. | 255.255.255.240 | 10.104.60.97 - 10.104.60.110  | 10.104.60.111 |
| Lan- 4 | 10.104.60.112 | 10.104.60. | 255.255.255.240 | 10.104.60.113 - 10.104.60.126 | 10.104.60.127 |

## B
usable host: 
- 2^4 - 2 = 14 > 7




# Q2
![[Pasted image 20260804113549.png]]

SERVER-LAN RIP Metric = 3

# Q3

### LAN 1
![[Pasted image 20260804115244.png]]

### LAN 2
![[Pasted image 20260804115310.png]]
### LAN 3
![[Pasted image 20260804115327.png]]
### LAN 4
![[Pasted image 20260804115344.png]]


![[Pasted image 20260804115447.png]]





# Q5

1. No Pool Excluded Address for default router address:
	1. Line after 12: ip dhcp excluded-address 10.50.60.110
2. Network Subnet Wrong
	1. At Line 14: Should be: network 10.50.60.96 255.255.255.240 
3. default Router Wrong
	1. At Line 15: Should be: default-router 10.50.60.110
4. No no auto-summary line
	1. After line 18: add: no auto-summary
5. Inter- router networks not added
	1. After line 19: add:
		1. network 192.168.200.4
		2. network 192.168.200.8