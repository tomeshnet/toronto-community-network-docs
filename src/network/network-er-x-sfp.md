# EdgeRouter X and EdgeRouter X-SFP

[EdgeRouter X](https://www.ui.com/edgemax/edgerouter-x/) and [EdgeRoute X-SFP](https://www.ui.com/edgemax/edgerouter-x-sfp/) will be both identified in this document as ER-X. Document is writte for Firmware 2.x.

## Device Specific Instructions

In shell to enter configuration mode, enter the command `configuration`.

To apply the configuration use `commit` and then to save it permanently use `save`.

Configurations can also be entered using the GUI found on port 80. Select `Config Tree` from menu. Tree is a hierarchical representation of the commands below. For example `set system hostname XXXX` would be `system` branch, `hostname` field, `XXX` would be the value.

## Configure hostname
*NOTE:  Hostname of device should follow the appropriate syntax defined in the TCN Standards*

```
set system host-name SN1R1
```

## Disable analytic reports
Disable sending crash reports to Ubiquti

```
set system analytics-handler send-analytics-report false
set system crash-handler send-crash-report false
```

## Configured DNS and NTP Server
Configure Date,Time, DNS and NTP services required for basic operations of the ER-X.
```
set system time-zone America/Toronto

set service dns forwarding name-server 10.10.10.10
set service dns forwarding listen-on 53

delete system ntp
set system ntp
set system ntp server 10.10.10.123 prefer
set system ntp server 206.108.0.131
set system ntp server 1.ubnt.pool.ntp.org
set system ntp server 2.ubnt.pool.ntp.org
set system ntp server 3.ubnt.pool.ntp.org
set system ntp server 4.ubnt.pool.ntp.org
```
ER-X does not have an internal clock so if working offline you must set the date and time manually to something recent otherwise there will be issues with date related things like certificates.

`date --set="1 Jan 2021 18:00:00"`

## Enable Hardware Acceleration

```
set system offload hwnat enable
```

## Configure ethernet ports

Delete the default DHCP configuration on `eth1`.
```
delete interfaces ethernet eth1 address dhcp
```

Configure each port with a unique IPv4 and IPv6 IP address. Use a `/24` subnet for IPv4 and `/64` for IPv6.
```
set interfaces ethernet eth1 address 100.64.10.1/24
set interfaces ethernet eth1 address fd54:4f4d:5348:400a::1/64
```

### Example
*Note: eth0 should be configured independantly since it is the port that is being used for configuration.*
```
delete interfaces ethernet eth0
delete interfaces ethernet eth1 address dhcp

set interfaces ethernet eth0 address 192.168.2.254/24
set interfaces ethernet eth1 address 100.64.10.1/24
set interfaces ethernet eth1 address fd54:4f4d:5348:400a::1/64
set interfaces ethernet eth2 address 100.64.11.1/24
set interfaces ethernet eth2 address fd54:4f4d:5348:400b::1/64
set interfaces ethernet eth3 address 100.64.12.1/24
set interfaces ethernet eth3 address fd54:4f4d:5348:400c::1/64
set interfaces ethernet eth4 address 100.64.13.1/24
set interfaces ethernet eth4 address fd54:4f4d:5348:400d::1/64
```

## Enable POE

Enable POE on the ports that will have an antenna attached to it. 
*Note: Avoid plugging in any device not meant to receive passive POE while it is one to avoid damage to your device. This step can be defer until the antennas are ready for use*

Turn on POE on a port 
```
set interfaces ethernet eth1 poe output 24v 
```

Turn off POE on a port
```
set interfaces ethernet eth1 poe output off
```

## Add DHCP (IPv4)

Enable the DHCP server by setting `disabled` as false.
```
set service dhcp-server disabled false
```

Create a DHCP named definition. Use the IP network subnet as the name.
```
set service dhcp-server shared-network-name <IPv4 Network> subnet <IPv4 Network>/<CIDR>
```

Set as authoritative for the subnet
`set service dhcp-server shared-network-name <IPv4 Network> authoritative enable`

The next series of command will be repeated per DHCP `shared-network-name` instance defined above. These commands should be prefixed with `set service dhcp-server shared-network-name <IPv4 Network> subnet <IPv4 Network>/<CIDR>` in place of  `...`.


Define the default route that will be used.
`... default-router xxxx.xxxx.xxxx.1`  

Define the DNS server that will be used.
`... dns-server 10.10.10.10` 

Define the NTP server that will be used
`...ntp-server 10.10.10.123`

Define the domain suffix that will be used. The clients will attempt to add this to the end of any domain names that are used if not found. For example `sn1a1` will try to resolve `sn1a1.core.tcn.tomesh.net`
`... domain-name core.tcn.tomesh.net`   

Define the length of the lease in seconds.
`... lease 600`  

Define the first and last IP address that will be issued.
`... start xxx.xxx.xxx.127 stop xxx.xxx.xxx.254`  

### Example

```
set service dhcp-server disabled false
delete service dhcp-server shared-network-name
set service dhcp-server shared-network-name 100.64.10.0 authoritative enable
set service dhcp-server shared-network-name 100.64.10.0 subnet 100.64.10.0/24 default-router 100.64.10.1
set service dhcp-server shared-network-name 100.64.10.0 subnet 100.64.10.0/24 dns-server 10.10.10.10
set service dhcp-server shared-network-name 100.64.10.0 subnet 100.64.10.0/24 domain-name tcn.tomesh.net
set service dhcp-server shared-network-name 100.64.10.0 subnet 100.64.10.0/24 ntp-server 10.10.10.123
set service dhcp-server shared-network-name 100.64.10.0 subnet 100.64.10.0/24 lease 600
set service dhcp-server shared-network-name 100.64.10.0 subnet 100.64.10.0/24 start 100.64.10.127 stop 100.64.10.254

set service dhcp-server shared-network-name 100.64.11.0 authoritative enable
set service dhcp-server shared-network-name 100.64.11.0 subnet 100.64.11.0/24 default-router 100.64.11.1
set service dhcp-server shared-network-name 100.64.11.0 subnet 100.64.11.0/24 dns-server 10.10.10.10
set service dhcp-server shared-network-name 100.64.11.0 subnet 100.64.11.0/24 domain-name tcn.tomesh.net
set service dhcp-server shared-network-name 100.64.11.0 subnet 100.64.11.0/24 ntp-server 10.10.10.123
set service dhcp-server shared-network-name 100.64.11.0 subnet 100.64.11.0/24 lease 600
set service dhcp-server shared-network-name 100.64.11.0 subnet 100.64.11.0/24 start 100.64.11.127 stop 100.64.11.254

set service dhcp-server shared-network-name 100.64.12.0 authoritative enable
set service dhcp-server shared-network-name 100.64.12.0 subnet 100.64.12.0/24 default-router 100.64.12.1
set service dhcp-server shared-network-name 100.64.12.0 subnet 100.64.12.0/24 dns-server 10.10.10.10
set service dhcp-server shared-network-name 100.64.12.0 subnet 100.64.12.0/24 domain-name tcn.tomesh.net
set service dhcp-server shared-network-name 100.64.12.0 subnet 100.64.12.0/24 ntp-server 10.10.10.123
set service dhcp-server shared-network-name 100.64.12.0 subnet 100.64.12.0/24 lease 600
set service dhcp-server shared-network-name 100.64.12.0 subnet 100.64.12.0/24 start 100.64.12.127 stop 100.64.12.254

set service dhcp-server shared-network-name 100.64.13.0 authoritative enable
set service dhcp-server shared-network-name 100.64.13.0 subnet 100.64.13.0/24 default-router 100.64.13.1
set service dhcp-server shared-network-name 100.64.13.0 subnet 100.64.13.0/24 dns-server 10.10.10.10
set service dhcp-server shared-network-name 100.64.13.0 subnet 100.64.13.0/24 domain-name tcn.tomesh.net
set service dhcp-server shared-network-name 100.64.13.0 subnet 100.64.13.0/24 ntp-server 10.10.10.123
set service dhcp-server shared-network-name 100.64.13.0 subnet 100.64.13.0/24 lease 600
set service dhcp-server shared-network-name 100.64.13.0 subnet 100.64.13.0/24 start 100.64.13.127 stop 100.64.13.254
```

## L2TP Tunnel

L2TP Tunnels is used to connect back to the an [exit node](exit-node.md).  

All commands are prefixed with `set interfaces l2tpv3 l2tpeth0` where `l2tpeth0` is a numbered interface starting from 0.

Between the exit node configuration and the client node the following values are filled:

```
- destination-port <> source-port
- session-id <> peer-session-id
- tunnel-id <> peer-tunnel-id
- remote-ip <> local-ip
```

`local-ip` must be an IP address defined on the router. This means if the device is behind a NAT, this will be a local IP, and not the public IP. Also, if the device is behind NAT, sometimes the `source-port` needs to be added to the router's port forward settings.

Configure each port with a unique IPv4 and IPv6 address. Use a `/30` subnet for IPv4 and `/126` for IPv6. Check with the exit node you are using to confirm to the numbering selected.

Please note that the target ip must be reachable before the config can be saved. This means if a workaround is being used, implement it first.

### Example

```
set interfaces l2tpv3 l2tpeth0 description "To Exit Node NJ"
set interfaces l2tpv3 l2tpeth0 destination-port 5000
set interfaces l2tpv3 l2tpeth0 local-ip 192.168.40.55
set interfaces l2tpv3 l2tpeth0 mtu 1412
set interfaces l2tpv3 l2tpeth0 peer-session-id 1000
set interfaces l2tpv3 l2tpeth0 peer-tunnel-id 3000
set interfaces l2tpv3 l2tpeth0 remote-ip 199.195.250.209
set interfaces l2tpv3 l2tpeth0 session-id 2000
set interfaces l2tpv3 l2tpeth0 source-port 6000
set interfaces l2tpv3 l2tpeth0 tunnel-id 4000

set interfaces l2tpv3 l2tpeth0 address 100.127.2.2/30
set interfaces l2tpv3 l2tpeth0 address fd74:6f6d:7368:7f02::2/126
```

## Babeld

Babeld does not come standard on ER-X.  Use the deb package located at https://github.com/tomeshnet/RouterX-Babeld-Package and install it using `dpkg -i` on the router. Review the instructions on [the README](https://github.com/tomeshnet/RouterX-Babeld-Package) to allow package to survive firmware upgrades.

All commands below must be prefixed with `set protocols babeld`.

`denydefault` and `denydefaultlocal` add a "deny all" rules to the end of the configuration. This prevents unwanted IPs from being announced such as the local IP address of the gateway network in case of a gateway.

`local-port` defines the port on the loopback address that will let you interact with babeld. Mainly used for debugging.

`interface ethX`  
Listen to babeld protocol on and peer with anything on this interface.

```
filter [name] action allow
filter [name] if [if]
filter [name] type redistribute
```
These 3 lines makeup a rule called [name]. This rules will allow redistribution of all IPs on the [if] interface.


### Example

```
set protocols babeld local-port 999
set protocols babeld denydefault true
set protocols babeld denydefaultlocal true
set protocols babeld interface l2tpeth0
set protocols babeld interface eth1
set protocols babeld interface eth2
set protocols babeld interface eth3
set protocols babeld interface eth4

set protocols babeld filter eth1 action allow
set protocols babeld filter eth1 if eth1
set protocols babeld filter eth1 type redistribute

set protocols babeld filter eth2 action allow
set protocols babeld filter eth2 if eth2
set protocols babeld filter eth2 type redistribute

set protocols babeld filter eth3 action allow
set protocols babeld filter eth3 if eth3
set protocols babeld filter eth3 type redistribute


set protocols babeld filter eth4 action allow
set protocols babeld filter eth4 if eth4
set protocols babeld filter eth4 type redistribute

set protocols babeld filter l2tpeth0 action allow
set protocols babeld filter l2tpeth0 if l2tpeth0
set protocols babeld filter l2tpeth0 type redistribute
```

## DDNS for L2TP Gateway Nodes

L2TP Tunnels require known static points `ip` addresses to connect to. Many internet providers provide dynamic `ip` addresses. Although `ip` addresses do not change often, if they do the connection to the mesh may be lost.

To solve this issue `ddns` is used to update a hostname anytime the ip changes. Due to the way dns works, there may be up to a 5 min delay in re-establishing the links. 

The `ddns` server that is being used is Hurricane Electric's `dns.he.net`. Hostname, login and password are set in the `dns.he.net` control panel and are unique per device. Access to DNS cannot be guaranteed when the link is down, IP addresses are resolved and hard coded.

Example below is for `sn1r1`. 

Bypass default route pointing at the mesh exit node, and send traffic directly over the local internet connection for the two services.
```
set protocols static route 184.105.242.3/32 next-hop 192.168.2.1
set protocols static route 184.105.242.4/32 next-hop 192.168.2.1
```

Configure the `ddns` service.
```
set service dns dynamic interface eth0 service dyndns host-name sn1r1.pub.tcn.tomesh.net
set service dns dynamic interface eth0 service dyndns login sn1r1.pub.tcn.tomesh.net
set service dns dynamic interface eth0 service dyndns password <PASSWORD>
set service dns dynamic interface eth0 service dyndns server 184.105.242.3
```

By default `ddns` will use the IP address of `eth0` which will be the internal IP address. Configure `ddns` to use Hurricane Electric's `checkip.dns.he.net` get the public IP address.

```
set service dns dynamic interface eth0 web 184.105.242.4
set service dns dynamic interface eth0 web-skip "Your IP address is : "
```             

## Gateway Loop issues

The paradox of the gateway and routing babeld:

- DHCP provides the default route to exit to the Internet
- Tunnel is established by using the default route
- Babeld receives the IP address from the neighbour on the other side of the tunnel
- The default route gets replaced with the exit node
- All Internet traffic is rerouted through the tunnel
- Since the tunnel uses Internet traffic it is also rerouted
- Tunnel collapses, since it is now feeding into itself


### Workaround 1 - Static IP/Route

**Description**
The default route to the internet is removed, and a static route for the destination IP Addresses created.

**Pro**
- Easy to understand
- Easy to implement
- Access to mesh from local device
- Cannot use DHCP for LAN configuration (Must be static)

**Con**
- When mesh is offline, only manually routed IP addresses are available
- Manually routed IP Addresses always exit out the local internet and not will not be available from the mesh if routing passes through device

**Process*

- Define static IP on `ETH0` (WAN facing port)
- Remove default gateway
- Create route out to the gateway for only the exit node

```
delete interfaces ethernet eth0
set interfaces ethernet eth0 address 192.168.2.254/24

delete protocols static route 0.0.0.0/0
set protocols static route 199.195.250.209/32 next-hop 192.168.2.1
```

### Workaround 2 - Route Tables

**Description**

Multiple route tables are used to store the two networks. Local internet's default route (0.0.0.0) remains in the global route table. Babeld is instructed to place all the installed routes into a new separate routing table including the mesh's default route. A rule is used to route all packets coming in from mesh facing interfaces through this new route table. Routes for the local node's IP addresses are added manually since Babeld will not install these. Rules and routes must be installed in both the IPv4 and IPv6 stacks.

**Pro**
- Static routes to the Internet are not required
- Access to Internet form local node is available when mesh is not online
- Does not break routing for individual IP addresses

**Con**
- Harder to understand
- Harder to setup
- No way to access mesh network from local device

**Process**

Configure Babeld to place all mesh routes into seperate route table.
```
set protocols babeld export-table 10
```

Create a  shell script at `/config/scripts/post-config.d/mesh.sh` and mark it executable 
```
echo "#!/bin/bash" >  /config/scripts/post-config.d/mesh.sh
chmod +x /config/scripts/post-config.d/mesh.sh
```

Populate the script with the following for each interface that Babeld will route though. `-6` indicates it is an IPv6 address.

```
ip [-6] rule add iif <INT> table 10
```

Configure interface based routes in table 10 for each interface and ip that the above interfaces are responsible for. 
````
set protocols static table 10 interface-route <IPv4>/<CDIR> next-hop-interface <INT>
set protocols static table 10 interface-route6 <IPv6>/<CDIR> next-hop-interface <INT>
````

*Reference: The above configuration lines are the equivalent of `ip [-6] route add <IPv4>/<CDIR> dev <INT> table 10`*

#### Example

Create `/config/scripts/post-config.d/mesh.sh`
```
#!/bin/sh
ip rule add iif eth4 table 10
ip -6 rule add iif eth4 table 10
ip rule add iif l2tpeth63 table 10
ip -6 rule add iif l2tpeth63 table 10
```

Configuration
```
set protocols babeld export-table 10
set protocols static table 10 interface-route 100.64.20.0/24 next-hop-interface eth4
set protocols static table 10 interface-route fd54:4f00::/24 next-hop-interface eth4
set protocols static table 10 interface-route 100.127.2.252/30 next-hop-interface l2tpeth63
set protocols static table 10 interface-route fd74:6f6d:7368:7f02::fc/126 next-hop-interface l2tpeth63
```
  
## OpenVPN - Management Tunnel

OpenVPN is used to set up a management tunnel to allow an alternative means of accessing the router from outside of the mesh network, in case the mesh fails.

For OpenVPN to work correctly it must have the correct date time. Before the device establishes a connection to the mesh for the first time the date/time may be incorrect. To configure the date/time manually:

```
date -s `2020-11-11 12:00:00`
```

Write certificates (provided by OpenVPN server). For example:

```
cat <<"EOF">/config/auth/VPN1.crt
-----BEGIN CERTIFICATE-----
[REDACTED]
-----END CERTIFICATE-----
EOF
cat <<"EOF">/config/auth/VPN1.key
-----BEGIN PRIVATE KEY-----
[REDACTED]
-----END PRIVATE KEY-----
EOF
cat <<"EOF"> /config/auth/ca.crt
-----BEGIN CERTIFICATE-----
MIIEszCCA5ugAwIBAgIJAIiwPYS5mr9AMA0GCSqGSIb3DQEBCwUAMIGXMQswCQYD
VQQGEwJDQTELMAkGA1UECBMCT04xEDAOBgNVBAcTB1Rvcm9udG8xFTATBgNVBAoT
DFRvcm9udG8gTWVzaDEPMA0GA1UECxMGdG9tZXNoMQ8wDQYDVQQDEwZ0b21lc2gx
DzANBgNVBCkTBnRvbWVzaDEfMB0GCSqGSIb3DQEJARYQaGVsbG9AdG9tZXNoLm5l
dDAeFw0yMDA3MjAyMDEwNTNaFw0zMDA3MTgyMDEwNTNaMIGXMQswCQYDVQQGEwJD
QTELMAkGA1UECBMCT04xEDAOBgNVBAcTB1Rvcm9udG8xFTATBgNVBAoTDFRvcm9u
dG8gTWVzaDEPMA0GA1UECxMGdG9tZXNoMQ8wDQYDVQQDEwZ0b21lc2gxDzANBgNV
BCkTBnRvbWVzaDEfMB0GCSqGSIb3DQEJARYQaGVsbG9AdG9tZXNoLm5ldDCCASIw
DQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMkWwPiHTd9T1a82HXAJao771ZRJ
1438yx6Ovm8mt+Z6MZ0r87BZ/pQ1TjcAgPNoV2TfnXkj8I1x2hoOub/EUu+WAYtf
54Ozjse1nUEiXB8WHNTh1VsYqcDlna16Js9/jcK+WkQvolf9bHZgwzJxqV62Yt+P
0pFPoIFsU4otdNCBKHmPF3UvEhq7Xa+BY3mtfFf6AIz/cjuS4sODz/jiNV9FfEbg
0aW6/NOuM/TbUy7Cu3ubuf5ESMnI20jEBhln+ksCkzqLANegdMRp8d7WZxYkvdgb
CYX9hRL83hQe2jhwijYw0DXvR5CqUwJXg8ERuRoxtpoNx00nuuk2dVY3gwUCAwEA
AaOB/zCB/DAdBgNVHQ4EFgQUzuAyZEfBiPvL7oHQWAfkfMOC8O0wgcwGA1UdIwSB
xDCBwYAUzuAyZEfBiPvL7oHQWAfkfMOC8O2hgZ2kgZowgZcxCzAJBgNVBAYTAkNB
MQswCQYDVQQIEwJPTjEQMA4GA1UEBxMHVG9yb250bzEVMBMGA1UEChMMVG9yb250
byBNZXNoMQ8wDQYDVQQLEwZ0b21lc2gxDzANBgNVBAMTBnRvbWVzaDEPMA0GA1UE
KRMGdG9tZXNoMR8wHQYJKoZIhvcNAQkBFhBoZWxsb0B0b21lc2gubmV0ggkAiLA9
hLmav0AwDAYDVR0TBAUwAwEB/zANBgkqhkiG9w0BAQsFAAOCAQEAjdLedME06tE3
WZgZlA20TdlMThdV67hkKBZbGUhlIqJuaykSi2Fp8zVg5CxgGNOp5GJRQ2z1rMtr
dPSIV6lYxfFgGTqfBRyw4dMyfDxA2XO3mMlTJzVNYcE1zXJp75ZpLxUEhcPNllYa
l481UxG2SQdQh9UPrtKJxf3KSTvRJufzxrh6i3iPmZRzLhPt7Vc+aCYjUFq+EJ/z
q1ACKn0pA8H3YbbdhA/RPQh3CRKiuE4kdq3BP4YYjW8xysrEU3TKfXHz4BY1OTPl
JxRDuteL3nTPh22VfcY/j6OeVtGKg+BGJv4xRDEb0a0xx58Ki+9R6TPotNuHYj1L
clmLw8d8Ew==
-----END CERTIFICATE-----
EOF
```

Configure OpenVPN tunnel named vtun99. 

```
set interfaces openvpn vtun99 mode client
set interfaces openvpn vtun99 protocol udp
set interfaces openvpn vtun99 remote-host 198.98.49.249
set interfaces openvpn vtun99 remote-port 4141
set interfaces openvpn vtun99 tls ca-cert-file /config/auth/ca.crt
set interfaces openvpn vtun99 tls cert-file /config/auth/VPN1.crt
set interfaces openvpn vtun99 tls key-file /config/auth/VPN1.key
```

If needed for the babeld workaround 2, a static route can be defined:

```
set protocols static route 198.98.49.249/32 next-hop 192.168.2.1
```
# Appendix A - Babeld - Cross-Compile

Babeld can be cross compiled to work on ER-X as follows:

```
sudo apt-get install gcc-mipsel-linux-gnu
git clone git://github.com/jech/babeld.git
cd babeld
make CC='mipsel-linux-gnu-gcc -static'
scp babeld admin@<IPADDRESS>:/tmp
```
