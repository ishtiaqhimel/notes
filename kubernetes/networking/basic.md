## Basic Netwoking
### Layering
- Layering means decomposing the problem into more manageable components(Layers).
- provides more modular design
- easy to troubleshoot

### Protocols
- a set of rules that governs data communication
- the protocols in each layer governs the activities of the data communication

### OSI Model
- The OSI model is a conceptual framework for describing how two systems communicate over a network
- The OSI model breaks down the responsibility of sending data across networks into layers
- The OSI model description is a complex and exact way of saying networks have layers like cakes or onions.
- The OSI model breaks the responsibilities of the network into seven distinct layers, each with different functions to aid in transmitting information from one system to another
- The layers are: Application, Presentation, Session, Transport, Network, Data Link, and Physical
- Each layer takes data from the previous layer and encapsulates it to make its Protocol Data Unit (PDU)
- *Application Layer*: top layer of the OSI model and is the one the end user interacts with every day
- *Presentation Layer*: allows two systems to use different encodings for data and still pass data between them
- *Session Layer*: responsible for the duplex of the connection, in other words, whether sending and receiving data at the same time
- *Transport Layer*: transfers data between applications, providing reliable data transfer services to the upper layers
- *Network Layer*: implements a means of transferring variable-length data flows from a host on one network to a host on another network while sustaining service quality
- *Data Link Layer*: responsible for the host-to-host transfers on the same network. It defines the protocols to create and terminate the connections between two devices
- *Physical Layer*: is represented visually by an Ethernet cord plugged into a switch. This layer converts data in the form of digital bits into electrical, radio, or optical signals

*How to Remember the layers?*
- `Please Do Not Throw Sausage Pizza Away`

### TCP/IP
- TCP/IP creates a heterogeneous network with open protocols that are independent of the operating system and architectural differences
- The layers are: Application(HTTP, DNS, DHCP, FTP), Transport(TCP, UDP), Internet(IPv4, IPv6, ICMPv4, ICMPv6), Physical(PPP, Frame Relay, Ethernet)
- *Application*: Represents data to user, plus encoding and dialog control
- *Transport*: Supports communication between diverse devices across diverse networks
- *Internet*: Determines the best path through the network
- *Physical*: Controls the hardware devices and media that make up the network

### PDU
- Protocol Data Units(PDUs) are named according to the protocols of the TCP/IP suite: data(application layer), segment(Transport layer), packet(Network layer), frame(Data Link layer), and bits(Physical layer).

