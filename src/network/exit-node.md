# Exit Node Configuration

Instructions are for the DEBIAN 10 operating system.

## Babeld Insatllation

Latest version of babeld can be found currently at 
http://node2.e-mesh.net/deb/repos/apt/debian/pool/main/b/babeld/

### /etc/babeld.conf


Redistribute default gateway to network

```
redistribute  ip ::/0 le 64 metric 256
redistribute  ip 0.0.0.0/0 le 24 metric 256
```


## Enable NAT

Create `/etc/rc.local` and `chmod +x` it

Add the following content to the file

```
#!/bin/bash
ip6tables -t nat -F POSTROUTING
iptables -t nat -F POSTROUTING
ip6tables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
exit 0
```

## L2TP tunnels

Layer 2 tunneling protocol files are used to allow gateways to connect over the internet to the exit node.

<insert script refrence here>