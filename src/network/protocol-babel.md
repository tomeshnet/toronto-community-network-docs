# Babel

[Babel](https://www.irif.fr/~jch/software/babel/) is a loop-avoiding distance-vector routing protocol. It does link cost estimation and redistribution of routes from other routing protocols. 

The network uses the [reference implementation](https://github.com/jech/babeld) of Babel called babeld. Updated packages for Debian can be found at the Toronto Mesh [Debian repository](https://repo.tomesh.net/repos/apt/debian/pool/main/b/babeld/). These packages are compiled from source and packaged using scripts in the [mesh-packages](https://github.com/tomeshnet/mesh-packages/tree/master/packages/babeld) GitHub repository.

The package for the EdgeRouter X/SFP with UI support is managed by Toronto Mesh and can be found at https://github.com/tomeshnet/RouterX-Babeld-Package.

Prototype babeld configuration can be generated at http://node2.e-mesh.net/CONF/ for both OpenWRT and Linux.

## When is Babel needed?

Babel is only required when your node routes IPs or a subnet that was not provided by a remote node.

## Babeld console

Depending what port the service started on (`local-port` or `-G` options) you can access babeld's console using on of the following (assuming 999 is the port).

- `nc :: 999`
- `telnet :: 999`

Note that some versions of `nc` do not support IPv6 so that command will not work.


### Dump Command

The command `dump` in the console will list all the currently known data points of babeld.

```
add interface <INT> up true ipv6 <IPv6> ipv4 <IPv4>
```
Indicates that the interfaces `<INT>` will be used to find other babeld nodes. `<IPv6>` and `<IPv4>` are required for routing traffic through the nodes. If one is missing check your interface configuration.

`add interface <INT> up false`  
Indicates the interfaces is assigned to babeld, but are currently not functional (cable not plugged in, or simply down)/

```
add neighbour f3ecb0 address <IPv6> if <INT> reach ffff ureach 0000 rxcost 96 txcost 96 cost
```
Indicates nodes found directly connected to babeld. `<IPv6>` is the local link IP found on the remote node,  `<INT>` is the interface this link was found on. The combination of the two (`<IPv6>%<INT>`) is used to access the link.

`add xroute...metric 256`  
Indicates the routes babeld is announcing from its routing table. `metric 256` is the cost that it is announced as.

`add route ...`  
Indicates routes that babeld has learned about in the network. `installed yes` or `installed no` indicates if this route is actively being used by being installed in the node's route table. Make note of `metric` numbers as they inform if the link will be used or not.
