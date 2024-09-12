---
tags:
  - H2
  - Linux
  - Webserver
  - Skole
---
Bruger [[Master]] som grundlag

### Technical
IPv4 Config:
Manual
Addresse: 10.0.0.5
Gateway: 10.0.0.1
DNS Server: 10.0.0.2
Search Domain: denlange.local

## Opsætning
### FTP
Ret filen "/etc/vsftpd/vsftpd.conf" og ændrer følgende linjer:
- anonymous_enable=no
- ftpd_banner=Welcome to denlange FTP service.
- listen=YES
- listen_ipv6=NO

Tilføj følgende linjer efter "listen_ipv6":
- pasv_enable=Yes
- pasv_min_port=10090
- pasv_max_port=10100
- pasv_address=10.0.0.5

### Firewall
Inde i OPNsense, laver vi en port forward for port 21 og 10090-10100.