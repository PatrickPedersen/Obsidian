### Named EIGRP:
```
R1 (config)# router eigrp <name>
R1 (config-router)# address-family [ip|ipv6] unicast autonomous-system <as>
R1 (config-router-af)# eigrp router-id <x.x.x.x>

# Passive interface
R1 (config-router-af)# af-interface <interface>
R1 (config-router-af-interface)# passive-interface
R1 (config-router-af-interface)# end

# Summary Address
R1 (config)# router eigrp <name>
R1 (config-router)# address-family [ip|ipv6] unicast autonomous-system <as>
R1 (config-router-af)# af-interface <interface>
R1 (config-router-af-interface)# summary-address <network> <mask>
R1 (config-router-af-interface)# end

# Variance
R1 (config)# router eigrp <name>
R1 (config-router)# address-family [ip|ipv6] unicast autonomous-system <as>
R1 (config-router-af)# topology base
R1 (config-router-af-topology)# variance 2
R1 (config-router-af-topology)# end
```

### Classic EIGRP
```
R2 (config)# [ip|ipv6] router eigrp <as>
R2 (config-router)# eigrp router-id <x.x.x.x>

# Needs to be done on each interface that should advertise EIGRP
R2 (config)# interface g0/0/0
R2 (config-if)# [ip|ipv6] eigrp <as>
R2 (config-if)# exit

# Summary Address
R2 (config)# interface <interface>
R2 (config-if)# [ip|ipv6] summary-address eigrp <as> <network> <mask>

# Bandwidth
R2 (config)# interface <interface>
R2 (config-if)# bandwidth <amount>
R2 (config-if)# end
```

### EIGRP Authentication
```
R1 (config)# key chain <name>
R1 (config-keychain)# key <id>
R1 (config-keychain-key)# key-string <password>
R1 (config-keychain-key)# end

R1 (config)# router eigrp <name>
R1 (config-router)# address-family [ip|ipv6] unicast autonomous-system <as>
R1 (config-router-af)# af-interface <interface>
R1 (config-router-af-interface)# authentication key-chain <name>
R1 (config-router-af-interface)# authentication mode md5
R1 (config-router-af-interface)# end
```

### EIGRP Prefix-list
```
R2 (config)# [ip|ipv6] prefix-list <name> seq <number> <deny|permit> <network>
R2 (config)# [ip|ipv6] router eigrp <as>
R2 (config-rtr)# distribute-list prefix-list <name> out <interface>
R2 (config-rtr)# end
```

### Show commands
```
show ipv6 route eigrp
show ipv6 eigrp topology all-links
```