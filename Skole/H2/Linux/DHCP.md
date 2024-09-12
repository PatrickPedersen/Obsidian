---
tags:
  - H2
  - Linux
  - DHCP
  - Skole
---
Bruger [[Master]] som grundlag

### Technical
IPv4 Config:
Manual
Addresse: 10.0.1.2
Gateway: 10.0.1.1
DNS Server: 10.0.0.2
Search Domain: denlange.local

## Opsætning
Husk at sætte search domain på DHCP til "denlange.local". Ellers vil dns opslag fejle.

#### dhcpd.conf
Ændrer følgende linjer:
- option domain-name "denlange.local";
- option domain-name-servers dns.denlange.local;
Slet linjerne mellem "log-facility local7" of "This is a very ..."
Ændrer basic subnet declaration til:
- subnet 10.0.1.0 netmask 255.255.255.0
- range 10.0.1.10 10.0.1.254
- option routers dg.denlange.local
Slet alle linjer efter subnet decleration.