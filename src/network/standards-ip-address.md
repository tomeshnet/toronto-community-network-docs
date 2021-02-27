# IP Standard

The mesh runs a dual stack IPv4 and IPv6 networking. Each device is be assigned a minimum of 2 IP addresses, one of each type.

## IPv4 Subnet CIDR

For IPv4 the following CIDR definition

- /32 - Single Host
- /30 - PtP L2TP etc
- /29 - PtP Antenna links where antennas are in bridge mode but require an IP for administration
- /24 - Antenna Subnets

## IPv6 Subnet CIDR

For IPv6 the following CIDR definition

- /128 - Single Host
- /127 - PtP L2TP etc
- /126 - PtP Antenna links where antennas are in bridge mode but require an IP for administration
- /64 - Antenna Subnets

## Backhaul network
Used for super nodes and core infrastructure

**IPv4 range:** 100.64.0.0/10 

```
Address:   100.64.0.0            01100100.01 000000.00000000.00000000
Netmask:   255.192.0.0 = 10      11111111.11 000000.00000000.00000000
Wildcard:  0.63.255.255          00000000.00 111111.11111111.11111111
=>
Network:   100.64.0.0/10         01100100.01 000000.00000000.00000000 (Class A)
Broadcast: 100.127.255.255       01100100.01 111111.11111111.11111111
HostMin:   100.64.0.1            01100100.01 000000.00000000.00000001
HostMax:   100.127.255.254       01100100.01 111111.11111111.11111110
Hosts/Net: 4194302 
```

**IPv6:** FD54:4f4d:5348::/48

## Node Network
Used for community hubs and non-core devices

**IPv4:** 10.0.0.0/8
**IPv6:** FD74:6F6D:73:86::/48


```
Address:   10.0.0.0              00001010 .00000000.00000000.00000000
Netmask:   255.0.0.0 = 8         11111111 .00000000.00000000.00000000
Wildcard:  0.255.255.255         00000000 .11111111.11111111.11111111
=>
Network:   10.0.0.0/8            00001010 .00000000.00000000.00000000 (Class A)
Broadcast: 10.255.255.255        00001010 .11111111.11111111.11111111
HostMin:   10.0.0.1              00001010 .00000000.00000000.00000001
HostMax:   10.255.255.254        00001010 .11111111.11111111.11111110
Hosts/Net: 16777214              (Private Internet)
```

**Fun Fact:** IPv6 Address is in the `fd::/8 range`. The bytes of `74 6F 6D 73 86`  converted into ASCII spell TOMSH (lower case for backhaul range)