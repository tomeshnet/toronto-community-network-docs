# Exit Node Configuration

*Note: This is an advanced topic. If you are not familiar with the concepts in this document, please familiarize yourself with them first before proceeding.*

Instructions are for installing an exit node on the Debian 10 operating system. 


## Babeld Installation

Install the latest version of babeld on the node. The latest version can be found currently at 
https://repo.tomesh.net/repos/apt/debian/pool/main/b/babeld/

### Redistribute default gateway

Redistribute the default gateway to network, and prevent other exit node announcements from being accepted.

Create or append to the `/etc/babeld.conf` file

```
redistribute ip ::/0 le 64 metric 256
redistribute ip 0.0.0.0/0 le 24 metric 256
in ip 0.0.0.0/0 le 0 deny
in ip ::/0 le 0 deny

```

## Enable NAT

Create `/etc/rc.local` and `chmod +x` it

Add the following content to the file

```bash
#!/bin/bash
ip6tables -t nat -F POSTROUTING
iptables -t nat -F POSTROUTING
ip6tables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
exit 0
```

## L2TP tunnels

Layer 2 tunneling protocol is used to allow gateways to connect over the internet to the exit node.

Server side scripts that support DDNS can be found in the [toronto-community-network](https://github.com/tomeshnet/toronto-community-network/tree/master/network/scripts/l2tp-tunnel-endpoint) repository.
