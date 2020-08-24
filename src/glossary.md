# Glossary

This glossary is used to house key terminology and definitions relevant to mesh networking and Toronot Mesh specific definitions.

## Organizational

### Toronto Community Network

The name of the Toronto-based community mesh network initiative started by Toronto Mesh with its collaborators in 2020.

This book documents the current resources of this initiative. For more information, visit [the project page](https://github.com/tomeshnet/toronto-community-network/).

### Toronto Mesh

A community group that helps communities create better networks with open source and peer-to-peer technologies that promote digital literacy and privacy.

The group that started the Toronto Community Network initiative and its steward for the foreseeable future. For more information, visit [tomesh.net](https://tomesh.net).

## Parts of A Network

### Node

A connection point on a network. A network is formed when two nodes are able to communicate with one another.

A **node** is a device that's connected and accessible on the mesh network. This might be a router and antenna combo, for example, on top of someone's house, or an apartment building. This doesn't include devices connected to the mesh network that are [NATed](https://en.wikipedia.org/wiki/Network_address_translation), so even if your home router is connected to the mesh network, your laptop and phone usually aren't counted as nodes.

### Link

A logical connection between two nodes (ignoring physical infrastructure in the way) or a physical link between two nodes (using ethernet, fiber, wireless equipment, etc.). Links allow nodes to communicate with one another.

### Supernode

A **supernode** is an actively maintained relaying node that supports high bandwidth point-to-multipoint (PTMP) connectivity. Currently all supernodes are managed by Toronto Mesh, but other groups may also operate their own supernodes on the Toronto Community Network.

### Peer

A node on the network that both supplies and consumes network resources.

### Edge

An entry point onto a network. Generally an edge acts as a router which sits between a smaller, more localized subnetwork and a network backbone. 

### Backbone

The core of a network, responsible for connecting and quickly routing traffic/data between other subnetworks.

### Backhaul

The intermediate links between the network’s backbone and edge subnetworks.

## Moving Around A Network

### Hop

One portion of network path between the source and destination. Anytime data flows through another node on the network on the way to its destination, a hop occurs.

### Route/Path

A specified course taken from a starting point to a destination.

### Data Packet

A piece of data packaged up to be sent logically along a network path to a specific destination.

### Ethernet Frame

A piece of data packaged up to be transmitted physically along a network path to a specific destination.

### Protocol

A set of rules or standards for communication between nodes on a network.

### Propagation

The movement of data from one or more sources to one or more discrete destinations through a network, usually to make the data more accessible. 

### Multiplexing

When several different signals (digital and/or analog) are combined to be transferred over one shared medium.

## What Does A Network Look Like?

### Topology

The way in which objects are arranged and how they relate to one another.

### Centralized 

A network topology where all users/clients need to connect to a central server for all communications.

### Decentralized

A network topology where multiple servers are linked together, allowing clients to connect to any server and still be within the same network.

### Distributed

A network topology where all actors connect and communicate with one another, forming a peer-to-peer network. Each actor is both a client and a server.

### Federated

A network topology consisting of smaller, more centralized self-governed organizations (usually one server and many clients) that elect to share data with one another.

## Radio & Wireless

### Point-to-point (PTP)

A **PTP** (Point To Point) link is a connection limited to only two devices. This could be a virtual connection like a VPN or Tunnel, or a physical connection like two routers connected via a fibre optic link, or two antennas configured for P2P. Although most antennas are capable of doing both PTP and PTMP, antennas used for PTP usually are high gain by being directional. This allows them to reach longer distances with less noise.

### Point-to-multipoint (PTMP)

**PTMP** antennas support multiple links by allowing many antennas to connect to it. They're usually sector antennas (moderate gain) installed at a high point. These kinds of links are used to connect homes on the ground, because one antenna may connect to up to hundreds of others pointing to it. These kinds of links are more limited in range than PTP setups.

### dBm

Decibel-milliwatts, the power ratio in decibels per milliwatt used to measure absolute power.

### Gain

A measure of an antenna’s ability to convert input power into radio waves concentrated in a particular connection.

### Radio

A device within a piece of electronic equipment responsible for sending/receiving wireless signals.

### EIRP

Equivalent Isotropically Radiated Power, an IEEE standard for directional radio frequency power. The EIRP is the product of transmitter power and antenna gain for an isotropic antenna, measured in dBi (decibel isotropic).

### Frequency

The rate in which a complete radio wave occurs over a period of time. Typically measured in Hertz (Hz).

### Wavelength

The distance (in meters) of a complete radio wave.

### Amplitude

The distance (in meters) from the  center of a wave to its crest/trough (the edge).

### Band (spectrum)

Radio frequencies lie between 3 kHz and 300 GHz and are split up into different bands set aside to be used for the same purpose.

## Types of Antennas

### Omnidirectional

A type of antenna that radiates radio wave power uniformly in all directions on one plane.

### Isotropic

A type of antenna that radiates power in all directions. No existing physical antenna is actually fully isotropic, though isotropic antennas are also used as reference for gain.

### Yagi

A type of antenna consisting of multiple parallel elements arranged in a line.

### Cantenna

A directional waveguide antenna, made out of an open-ended metal can and designed to be unidirectional.

## Mesh Terminology 

### CJDNS

A layer 2/3 mesh networking protocol and application created by Caleb James DeLisle. CJDNS features encrypted connections, IPV6 addressing, and distributed routing.

### Routing Table

A set of rules that determine where packets will be directed within a network.

### 802.11s

An amendment to the IEEE 802.11 wireless local area network specification defining how wireless devices can communicate in static or ad hoc networks.

### Mesh Point (MP)

An operation mode defined within the 802.11s standard. Mesh Point allows nodes in a network to discover neighbor nodes and keep track of them.

### Auto-Peering

The ability for two peers on a network to automatically link with one another using zero configuration.

### Tunneling/Overlaying

Securely encapsulating communication/traffic from a private network within a larger, public network.

### Babel

The **Babel routing protocol** is what we use to create the mesh network. It's the software that allows the "meshing" to happen, by routing packets where they need to go.

[Wikipedia](https://en.wikipedia.org/wiki/Babel_(protocol)), [Homepage](https://www.irif.fr/~jch/software/babel/)

### babeld

Pronounced "babel-dee", this is a reference implementation of the Babel protocol, and the software used to run Babel on the mesh network. The "d" stands for [daemon](https://en.wikipedia.org/wiki/Daemon_%28computing%29). The code is available [here](https://github.com/jech/babeld).
