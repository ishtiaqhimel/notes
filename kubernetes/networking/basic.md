## Basic Netwoking
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

### TCP/IP
- TCP/IP creates a heterogeneous network with open protocols that are independent of the operating system and architectural differences
- The layers are: Application, Transport, Internet, Physical

