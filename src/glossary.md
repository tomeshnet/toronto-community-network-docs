# Glossary

## Toronto Community Network

The name of the Toronto-based community mesh network initiative started by Toronto Mesh with its collaborators in 2020.

This book documents the current resources of this initiative. For more information, visit [the project page](https://github.com/tomeshnet/toronto-community-network/).

## Toronto Mesh

A community group that helps communities create better networks with open source and peer-to-peer technologies that promote digital literacy and privacy.

The group that started the Toronto Community Network initiative and its steward for the foreseeable future. For more information, visit [tomesh.net](https://tomesh.net).

## Point-to-point (PTP)

A **PTP** link is a connection that only involves two antennas. These kinds of antennas are more powerful and can reach longer distances, but only one other antenna can connect to them. They're usually implemented using highly directional (i.e. high gain) antennas and used to connect two supernodes.

## Point-to-multipoint (PTMP)

**PTMP** antennas support multiple links by allowing many antennas to connect to it. They're usually sector antennas (i.e. moderate gain) installed at a high point. These kinds of links are used to connect homes on the ground, because one antenna may connect to up to hundreds of others pointing to it. These kinds of links are more limited in range than PTP setups.

## Node

A **node** is a device that's connected and accessible on the mesh network. This might be a router and antenna combo, for example, on top of someone's house, or an apartment building. This doesn't include devices connected to the mesh network that are [NATed](https://en.wikipedia.org/wiki/Network_address_translation), so even if your home router is connected to the mesh network, your laptop and phone usually aren't counted as nodes.

## Supernode

A **supernode** is an actively maintained relaying node that supports high bandwidth point-to-multipoint (PTMP) connectivity. Currently all supernodes are managed by Toronto Mesh, but other groups may also operate their own supernodes on the Toronto Community Network.

## Babel

The **Babel routing protocol** is what we use to create the mesh network. It's the software that allows the "meshing" to happen, by routing packets where they need to go.

[Wikipedia](https://en.wikipedia.org/wiki/Babel_(protocol)), [Homepage](https://www.irif.fr/~jch/software/babel/)

## babled

Pronounced "bable-dee", this is a reference implementation of the Bable protocol, and the software used to run Babel on the mesh network. The "d" stands for [daemon](https://en.wikipedia.org/wiki/Daemon_%28computing%29). The code is available [here](https://github.com/jech/babeld).
