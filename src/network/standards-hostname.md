# Hostname Naming Standards

This is the decided upon official naming convention for all devices and SSID in the network

## Supernode Name Standards

The supernode hostname is `snXyY` and where `X` is assigned by the Network Planning, Design and Operations working group, and `yY` is chosen by the node operator to identify network components within the node. For example `a1` for antenna 1 and `r1` for router 1.

All hostnames will be unique across the mesh.

The supernode devices will use the domain format `operator.tcn.tomesh.net`.  A DNS and reverse DNS entry will be made for each device with such a domain name. For simplicity the devices will also carry an entry with the domain of `tcn.tomesh.net` 

For example a FQDN (fully qualified domain name) will be `sn1a1.core.tcn.tomesh.net` for a device operated by the core team at Toronto Community Network. The node will also answer as `sn1a1.tcn.tomesh.net`

## SSID

### Public SSID 
Public SSID will not extend the Babel protocol. They are standard access points connections for the users to access the mesh.

**Format**
`tomesh.net`

### Mesh SSID 
Mesh SSID are used to extend the mesh network. They have will have Babel running on them.

**Format**

`tomesh`-`(protocol)`[-`(meshid)`] 

**parameters**

`tomesh-` is a constant and never changes

`(protocol)`  is the protocol name the SSID is running. This could be for example `airmaxac`,`80211s`,`adhoc`

`(meshid)` optional, part of the string when SSIDs need to be isolated. PtP and PtMP antennas will use their hostnames

**Example** 

a Toronto Mesh node with hostname of sn1a1 running airmax-ac protocol would be
`tomesh-airmaxac-sn1a1`

a Toronto Mesh node with running 80211s would be
`tomesh-80211s`