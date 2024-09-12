---
tags:
  - H2
  - Linux
  - Skole
---
## Installation:
Installeret VMware Workstation 17 Player (Evaluation)

Hentet Rocky Linux 9 Minimal

### Core Master:
Pakker installeret:
- Nslookup
- Traceroute
- TcpDump
- vim
- chrony
- ntpstat

SELinux Disabled.
FirewallD Disabled.

#### Wheel Sudo Group
Sudo group wheel tilføjet bruger.

#### SSHD_Config
Tilføjet bruger direkte til SSHD_Config. (Allow users "UserId")
Login forsøg sat til 3.

#### GUI Install
Group install Workstation
systemctl set-default graphical.target