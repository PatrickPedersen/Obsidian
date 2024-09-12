---
tags:
  - H2
  - Linux
  - Firewall
  - Skole
---
Bruger [[Master]] som grundlag

## Opsætning

#### VM
Tilføj 2 ekstra netkort for det interne netværk. Net fordelingen ser derfor således ud:
- Wan = em0 - Custom (VMnet0)
- Lan 1 = em1 - Custom (VMnet1)
- Lan 2 = em2 - Custom (VMnet2)

Standard installation af OPNsense.
Login:
- Username: root
- Password: Passw0rd

## Technical
Hostname: OPNsense-FW
Domain: denlange.local
Language: English
Primary DNS: 10.2.0.9
Secondary DNS: 10.2.0.10
Override DNS: Yes
Enable Resolver: Yes
DNSSEC Support/Data: No

Timeserver: Standard
Timezone: Europe/Copenhagen

#### Interfaces
Gå til Interfaces -> Assignments:
1.  DMZ settings:
	1. Enable Interface: True
	2. Description: DMZ
	3. IPv4: Static IPv4
	4. Mac Address: 00:0c:29:28:18:b2
	5. IPv4 Address: 10.0.0.1
	6. Upstream: Auto-Detect
2. LAN settings:
	1. Enable Interface: True
	2. Description: LAN
	3. IPv4: Static IPv4
	4. Mac Address: 00:0c:29:28:18:a8
	5. IPv4 Address: 10.0.1.1
	6. Upstream: Auto-Detect
3. WAN settings:
	1. Enable Interface: True
	2. Description: WAN
	3. IPv4: Static IPv4
	4. IPv6: None
	5. Mac Address: 00:0c:29:28:18:9e
	6. IPv4 Address: 10.233.141.135
	7. Upstream: WAN_GW - 10.233.141.1

!!HUSK - At trykke "Apply Changes" efter interface ændringer

### Regler
#### DMZ
Disse regler SKAL komme i denne rækkefølge.
1. IPv4 Block
	- Action: Block
	- Interface: DMZ
	- Direction: in
	- TCP/IP: IPv4
	- Protocol: any
	- Source: DMZ net
	- Destination: LAN net
2. IPv4 Pass
	- Action: Pass
	- Interface: DMZ
	- Direction: in
	- TCP/IP: IPv4
	- Protocol: any
	- Source: any
	- Destination: any

#### LAN
1. IPv4 Pass
	- Action: Pass
	- Interface: LAN
	- Direction: in
	- TCP/IP: IPv4
	- Protocol: any
	- Source: any
	- Destination: any

#### WAN
1. IPv4 Pass
	- Action: Pass
	- Interface: WAN
	- Direction: in
	- TCP/IP: IPv4
	- Protocol: TCP/UDP
	- Source: any
	- Destination: any
	- Destination:
		- From: SSH
		- To: SSH

#### Port Forward
1. HTTP
	- Interface: WAN
	- TCP/IP: IPv4
	- Protocol: TCP
	- Destination: WAN address
	- Destination port range:
		- From: HTTP
		- To: HTTP
	- Redirect target IP: 10.0.0.5
	- Redirect target port: HTTP
2. HTTP
	- Interface: WAN
	- TCP/IP: IPv4
	- Protocol: TCP
	- Destination: WAN address
	- Destination port range:
		- From: HTTPS
		- To: HTTPS
	- Redirect target IP: 10.0.0.5
	- Redirect target port: HTTPS
3. HTTP
	- Interface: WAN
	- TCP/IP: IPv4
	- Protocol: TCP
	- Destination: WAN address
	- Destination port range:
		- From: FTP
		- To: FTP
	- Redirect target IP: 10.0.0.5
	- Redirect target port: FTP
4. HTTP
	- Interface: WAN
	- TCP/IP: IPv4
	- Protocol: TCP
	- Destination: WAN address
	- Destination port range:
		- From: 10090
		- To: 10100
	- Redirect target IP: 10.0.0.5
	- Redirect target port: 10090

#### SSH Access
Gå til System -> Settings -> Administration. Ruld ned og find Secure Shell.
Ret følgende:
- Enable Secure Shell: true
- Login Group: wheel, admins
- Permit Password Login: true
- Listen Interfaces: WAN

Derefter lav en ny bruger under: System -> Access -> Users
Ret følgende:
- Set username
- Login/shell: sh
- Tilføj gruppe: Admins