## RSTP (802.1W)
### Port Route:
| Port Types | Beskrivelse |
| -----------|-------------|
| Root port (RP):  | Porten mod vores Root Bridge |
| Designated port (DP):  | Alle andre porte en vores RP. (Undtagen Blocking port) |
| Alternate port:  | Alternativ vej til Root bridge. Tillader hurtigere re-oprettelse af STP til Root bridge |
| Backup port:  | En ekstra forbindelse til samme switch mod Root bridge. Skulle det aktive link gå ned, så vil denne bruges. |

### Port Type:
| Port Roles | Beskrivelse |
| -----------|-------------|
| Edge Port: | Alle porte der ikke forbinder til andre switche |
| Root port: | Forbinder til Root bridge. |
| Point-to-Point port: | Full-duplex forbindelse mellem RSTP enheder. Eftersom der kun kan være to enheder på en full-duplex link, ville dette være den hurtigste måde at finde ud af om vi er forbundet til en anden switch. |

### Root Bridge placement:
- The root bridge to the lowest value
- The secondary root bridge to a value slightly higher than that of the root bridge
- All other switches to a value higher than the secondary root bridge
Default priority: `32,768`
Only jumps in increments of `4096`

`Spanning-tree vlan vlan-id root {primary | secondary}` er det samme som at skrive `24,576` og `28,672`

## STP Protection:
### Root Guard:
Skal placeres på alle porte som aldrig må blive til en root port.

### STP Portfast:
Sikre en hurtig oprettelse af forbindelse ved at springe direkte til forwarding. Hvis en portfast port modtager en BPDU pakke, så vil den automatisk slukke portfast og gå igang med STP.

| Command | Beskrivelse |
| -----------|-------------|
| spanning-tree portfast | Enable portfast on a specific access port |
| spanning-tree portfast | Global command to enable portfast on all access ports |
| spanning-tree portfast disable | Disable on a specific port |
| spanning-tree portfast trunk | Used on trunk links to enable portfast. Should only be used on ports with a single connection |

### BPDU Guard:
Sikkerheds mechanism der lukker for porte konfigureret med STP portfast når den modtager en BPDU.

| Command | Beskrivelse |
| -----------|-------------|
| spanning-tree portfast bpduguard default | Enables on all STP portfast ports |
| spanning-tree portfast bpduguard default {enable | disable} | Enables or disables BPDU Guard on a specific interface |
| show spanning-tree interface interface-id detail | Shows whether BDPU guard is enabled for the specified interface |

BDPU guard er typisk konfigureret mod slut devices.

#### Error Recovery:

| Command | Beskrivelse |
| -----------|-------------|
| errdisable recovery cause bpduguard | Enables recovery of ports that have been shutdown due to BDPU guard |
| errdisable recovery interval time-seconds | The period that the recovery checks for ports to recover (default: 300 seconds) |

#### Loop Guard:
Prevents root ports and alternative ports from becoming designated ports.
Loop Guard placerer porten in i en ErrDisabled state mens BPDUer ikke bliver modtaget eller sendt.