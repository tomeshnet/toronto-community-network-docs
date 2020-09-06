# EdgeRouter X and EdgeRouter X-SFP

[EdgeRouter X](https://www.ui.com/edgemax/edgerouter-x/) and [EdgeRoute X-SFP](https://www.ui.com/edgemax/edgerouter-x-sfp/) will be both identified in this document as ER-X.  We use version 1.x. 2.x has NOT been tested however preliminary results seem to indicate no immediate issues.

To enter configuration mode, SSH into the ER-X and enter the command `configuration`.

To apply the configuration use `commit` and then to save it permanently use `save`.

Configurations can also be entered using the GUI found on port 80. Select `Config Tree` from menu. Tree is a hierarchical representation of the commands below.
For example `set system hostname XXXX` would be `system` branch, `hostname` field, `XXX` would be the value.

## Configure hostname

```
set system host-name SN1R1
```

## Enable Hardware Acceleration

```
set system offload hwnat enable
```

## Ethernet ports

Delete the default DHCP configuration on `ETH1`.

Configure each port with a unique IPv4 and IPv6 IP address. Use a `/24` subnet for IPv4 and `/64` for IPv6.

### Example
```
delete interfaces ethernet eth1 address dhcp
set interfaces ethernet eth1 address 100.64.10.1/24
set interfaces ethernet eth1 address fd54:4f4d:5348:400a::1/64
set interfaces ethernet eth2 address 100.64.11.1/24
set interfaces ethernet eth2 address fd54:4f4d:5348:400b::1/64
set interfaces ethernet eth3 address 100.64.12.1/24
set interfaces ethernet eth3 address fd54:4f4d:5348:400c::1/64
set interfaces ethernet eth4 address 100.64.13.1/24
set interfaces ethernet eth4 address fd54:4f4d:5348:400d::1/64
```

## Add DHCP

Enable a DHCP subnet for each network you defined in the previous section.

Enable the DHCP server by setting `disabled` as false.


`set service dhcp-server shared-network-name XXXXXXXX`  
Create a DHCP named definition. Use the IP subnet as the name.

`set service dhcp-server shared-network-name XXXXXXXX subnet XXX.XXX.XXX.0/24`    
Create a subnet definition in the named definition. The rest of the commands must be prefixed with this command line, in place of the `...`.

Set the following items.

`... default-router xxxx.xxxx.xxxx.1`  
Define the default route that will be used.

`... dns-server 1.1.1.1`   
Define the DNS server that will be used.

`... domain-name tcn.tomesh.net`   
Define the domain suffix that will be used. The clients will attempt to add this to the end of any domain names that are used if not found. For example `sn1a1` will try to resolve `sn1a1.tcn.tomesh.net`

`... lease 600`  
Define the length of the lease in seconds.

`... start xxx.xxx.xxx.127 stop xxx.xxx.xxx.254`  
Define the first and last IP address that will be issued.

### Example

```
set service dhcp-server disabled false
delete service dhcp-server shared-network-name
set service dhcp-server shared-network-name 100.64.10.0 authoritative enable
set service dhcp-server shared-network-name 100.64.10.0 subnet 100.64.10.0/24 default-router 100.64.10.1
set service dhcp-server shared-network-name 100.64.10.0 subnet 100.64.10.0/24 dns-server 1.1.1.1
set service dhcp-server shared-network-name 100.64.10.0 subnet 100.64.10.0/24 domain-name to.mesh
set service dhcp-server shared-network-name 100.64.10.0 subnet 100.64.10.0/24 lease 600
set service dhcp-server shared-network-name 100.64.10.0 subnet 100.64.10.0/24 start 100.64.10.127 stop 100.64.10.254

set service dhcp-server shared-network-name 100.64.11.0 authoritative enable
set service dhcp-server shared-network-name 100.64.11.0 subnet 100.64.11.0/24 default-router 100.64.11.1
set service dhcp-server shared-network-name 100.64.11.0 subnet 100.64.11.0/24 dns-server 1.1.1.1
set service dhcp-server shared-network-name 100.64.11.0 subnet 100.64.11.0/24 domain-name to.mesh
set service dhcp-server shared-network-name 100.64.11.0 subnet 100.64.11.0/24 lease 600
set service dhcp-server shared-network-name 100.64.11.0 subnet 100.64.11.0/24 start 100.64.11.127 stop 100.64.11.254

set service dhcp-server shared-network-name 100.64.12.0 authoritative enable
set service dhcp-server shared-network-name 100.64.12.0 subnet 100.64.12.0/24 default-router 100.64.12.1
set service dhcp-server shared-network-name 100.64.12.0 subnet 100.64.12.0/24 dns-server 1.1.1.1
set service dhcp-server shared-network-name 100.64.12.0 subnet 100.64.12.0/24 domain-name to.mesh
set service dhcp-server shared-network-name 100.64.12.0 subnet 100.64.12.0/24 lease 600
set service dhcp-server shared-network-name 100.64.12.0 subnet 100.64.12.0/24 start 100.64.12.127 stop 100.64.12.254

set service dhcp-server shared-network-name 100.64.13.0 authoritative enable
set service dhcp-server shared-network-name 100.64.13.0 subnet 100.64.13.0/24 default-router 100.64.13.1
set service dhcp-server shared-network-name 100.64.13.0 subnet 100.64.13.0/24 dns-server 1.1.1.1
set service dhcp-server shared-network-name 100.64.13.0 subnet 100.64.13.0/24 domain-name to.mesh
set service dhcp-server shared-network-name 100.64.13.0 subnet 100.64.13.0/24 lease 600
set service dhcp-server shared-network-name 100.64.13.0 subnet 100.64.13.0/24 start 100.64.13.127 stop 100.64.13.254
```

## L2TP Tunnel

L2TP Tunnels is used to connect back to the an [exit node](exit-node.md).  

All commands are prefixed with `set interfaces l2tpv3 l2tpeth0` where `l2tpeth0` is a numbered interface starting from 0.

Between the EXIT NODE configuration and the CLIENT node the following values are filled 

```
- destination-port <> source-port
- session-id <> peer-session-id
- tunnel-id <> peer-tunnel-id
- remote-ip <> local-ip
```

`local-ip` must be an IP address defined on the router. This means if the device is behind a NAT, this will be a local IP, and not the public IP. Also, if the device is behind NAT, sometimes the `source-port` needs to be added to the router's port forward settings.

Configure each port with a unique IPv4 and IPv6 address. Use a `/30` subnet for IPv4 and `/126` for IPv6. Check with the exit node you are using to confirm to the numbering selected.

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

Babeld does not come standard on ER-X.  Use the deb package located at https://github.com/darkdrgn2k/RouterX-Babeld-Package and install it using `dpkg -i` on the router. Review the instructions on [the README](https://github.com/darkdrgn2k/RouterX-Babeld-Package) to allow package to survive firmware upgrades.

All commands below are to be prefixed with `set protocols babeld`

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

## Gateway Loop issues

The paradox of the gateway and routing babeld

- DHCP provides the default route to exit to the internet
- Tunnel is established by using the default route
- BABELD receives the IP address from the neighbour on the other side of the tunnel
- The default route gets replaced with the EXIT NODE
- All internet traffic is rerouted through the tunnel
- Since the tunnel uses internet traffic it is also rerouted
- Tunnel collapses  since its now feeding into itself


### Workaround 1 - Static IP/Route

- Define static IP on `ETH0` (WAN FACING PORT)
- Remove default gateway
- Create route out to the gateway for only the exit node

```
delete interfaces ethernet eth0 address dhcp
set interfaces ethernet eth1 address 192.168.2.254/24    

delete protocols static route 0.0.0.0/0
set protocols static route 199.195.250.209/32 next-hop 192.168.2.1
```

### Workaround 2 - Route Tables

Route tables allow the use of the default route (0.0.0.0) on the node to still point to the local internet connection. Babeld is instructed to place all the installed routes into a new routing table. A rule is installed to use this routing table for all packets coming in from interfaces we use babeld on. Then finally routes for the local node are added manually since babeld will not install them since they are local. Rules and routes must be installed in both the IPv4 and IPv6 stacks.

```
set protocols babeld export-table 10
```

Append the following lines to `/etc/rc.local` for each interface that babeld will route though. `-6` indicates it is an IPv6 address.

```
ip [-6] rule add iif <INT> table 10
```

Append the following lines for each IP address range your node is responsible for. `-6` indicates it is an IPv6 address.
```
ip [-6] route add <IPv4>/<CDIR> dev <INT> table 10
```

#### Example
```
ip rule add iif eth4 table 10
ip -6 rule add iif eth4 table 10
ip rule add iif l2tpeth63 table 10
ip -6 rule add iif l2tpeth63 table 10

ip route add 100.64.20.0/24 dev eth4 table 10
ip -6 route add fd54:4f00::/24 dev eth4 table 10
ip route 100.127.2.252/30 dev l2tpeth63 table 10
ip -6 route add fd74:6f6d:7368:7f02::fc/126 dev l2tpeth0 table 10
```

## OpenVPN - Management Tunnel

Write certificates (provided by OpenVPN server)

```
cat <<"EOF">/config/auth/SN1R1.crt
-----BEGIN CERTIFICATE-----
<<SANITIZED>>
-----END CERTIFICATE-----
EOF
cat <<"EOF">/config/auth/SN1R1.key
-----BEGIN PRIVATE KEY-----
<<SANITIZED>>
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

Configure OpenVPN named vtun99. 

```
set interfaces openvpn vtun99 mode client
set interfaces openvpn vtun99 protocol udp
set interfaces openvpn vtun99 remote-host 198.98.49.249
set interfaces openvpn vtun99 remote-port 4141
set interfaces openvpn vtun99 tls ca-cert-file /config/auth/ca.crt
set interfaces openvpn vtun99 tls cert-file /config/auth/SN1R1.crt
set interfaces openvpn vtun99 tls key-file /config/auth/SN1R1.key
```

If needed define static route (workaround 2)

```
set protocols static route 199.195.250.208/32 next-hop 192.168.40.1
```
# Appendix A - Babeld - Cross-Compile

Babeld can be cross compiled to work on ER-X as follows

```
sudo apt-get install gcc-mipsel-linux-gnu
git clone git://github.com/jech/babeld.git
cd babeld
make CC='mipsel-linux-gnu-gcc -static'
scp babeld admin@<IPADDRESS>:/tmp
```
