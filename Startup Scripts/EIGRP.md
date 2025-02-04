### Named EIGRP:
```
R1 (config)# router eigrp <name>
R1 (config-router)# address-family ipv6 unicast autonomous-system <as>
R1 (config-router-af)# eigrp router-id <x.x.x.x>

# Passive interface
R1 (config-router-af)# af-interface <interface>
R1 (config-router-af-interface)# passive-interface
R1 (config-router-af-interface)# end

# Summary Address
R1 (config)# router eigrp <name>
R1 (config-router)# address-family ipv6 unicast autonomous-system <as>
R1 (config-router-af)# af-interface <interface>
R1 (config-router-af-interface)# summary-address <network> <mask>
R1 (config-router-af-interface)# end
```

### Classic EIGRP
```
R2 (config)# ipv6 router eigrp <as>
R2 (config-router)# eigrp router-id <x.x.x.x>

# Needs to be done on each interface that should advertise EIGRP
R2 (config)# interface g0/0/0
R2 (config-if)# ipv6 eigrp <as>
R2 (config-if)# exit

# Summary Address
R2 (config)# interface <interface>
R2 (config-if)# ipv6 summary-address eigrp <as> <network> <mask>
```

### EIGRP Authentication
```
R1 (config)# key chain <name>
R1 (config-keychain)# key <id>
R1 (config-keychain-key)# key-string <password>
R1 (config-keychain-key)# end

R1 (config)# router eigrp <name>
R1 (config-router)# address-family ipv6 unicast autonomous-system <as>
R1 (config-router-af)# af-interface <interface>
R1 (config-router-af-interface)# authentication key-chain <name>
R1 (config-router-af-interface)# authentication mode md5
R1 (config-router-af-interface)# end
```

### Show commands
```
show ipv6 route eigrp
show ipv6 eigrp topology all-links
```