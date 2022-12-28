## Linux Networking
### **Network Interface**
- A network interface is a hardware component that connects a device to a network. It allows the device to send and receive data over the network and communicate with other devices on the same network.
- There are several types of network interfaces, including:
    - **Ethernet interface**: This is a common type of network interface found in computers, routers, and other networking devices. It uses twisted pair or fiber optic cables to transmit data over a network.
    - **Wireless interface**: This type of network interface allows a device to connect to a wireless network using radio frequency (RF) signals. Common examples include Wi-Fi and Bluetooth.
    - **Modem interface**: A modem interface is a type of network interface that allows a device to connect to the internet using a telephone line or other communication channel.
    - **Virtual interface**: A virtual interface is a software-based network interface that is created and managed by the operating system. It allows a single physical network interface to be divided into multiple virtual interfaces, each with its own unique network configuration.
- We can see a list of all network interfaces and their configurations with `ifconfig`
- The **loopback interface** is a special interface for same-host communication. `127.0.0.1` is the standard IP address for the loopback interface. The loopback interface is commonly abbreviated as `lo`.
- In K8s, a network interface is a virtual network interface that is created and managed by the Kubernetes networking subsystem. It allows a container, pod, or other Kubernetes resource to communicate with other resources on the same network or on a different network. K8s uses a *container network interface (CNI)* plugin to manage the network interfaces of pods and other resources.

### **Bridge Interface**
- A bridge interface is a type of network interface that connects two or more separate networks together and allows devices on one network to communicate with devices on the other network. A bridge interface acts as a bridge between the two networks, forwarding data packets between them.
- A bridge interface can be implemented in both hardware and software. In hardware, a bridge interface is often a standalone device that is connected to both networks using Ethernet or other cables. In software, a bridge interface can be created using the operating system's networking stack, allowing a single device to act as a bridge between two networks.
- Bridge interfaces are often used to connect two LANs (Local Area Networks) together, allowing devices on both networks to communicate with each other. They can also be used to connect a LAN to a WAN (Wide Area Network), such as the internet.
- In K8s, Bridges allow pods, with their individual network interfaces, to interact with the broader network via the node’s network interface.

Bridges can be managed and created using `ip` and `brctl` command. Lets create bridge interface and connecting veth pair $-$
- To see details about brctl options </br> `$ brctl`
- To show the bridge </br> `$ brctl show`
- To add bridge (need to be super user, use 'sudo' before command or 'sudo su' command) </br> `$ brctl addbr br0`
- To create veth pair </br>
```
$ ip link add name vth1 type veth peer vth_1
$ ip link add name vth2 type veth peer vth_2
$ ip -br -c link show type veth
```

```text
The command "ip link add name vth1 type veth peer vth_1" is used to create a virtual Ethernet interface (veth) in the Linux operating system. A veth interface is a virtual network interface that is created and managed by the operating system's networking stack. It allows two separate network namespaces to communicate with each other by providing a virtual link between them.

The "ip link add" command is used to create a new network interface, and the "name" option specifies the name of the new interface. In this case, the name of the interface is "vth1". The "type veth" option specifies that the interface is a veth interface, and the "peer vth_1" option specifies the name of the peer interface that is created in the other network namespace.

Once the veth interface is created, it can be used like any other network interface. For example, it can be assigned an IP address and used to communicate with other devices on the network.
```
- To create ip namespaces
```
$ ip netns add ns1
$ ip netns add ns2
$ ip netns ls
```
- To put interfaces in specific namespaces
```
$ ip link set dev vth_1 netns ns1
$ ip -n ns1 -c -br link show
$ ip -n ns1 link set dev vth_1 up
$ ip -n ns1 address add 192.168.10.1/24 dev vth_1
$ ip -n ns1 -br -c address show
(do same for vth_2 and add address 192.168.10.2/24)
```
- To attach veth to our bridge
```
$ ip link set dev vth1 master br0
$ ip link set dev vth2 master br0
```
- To up the interfaces
```
$ ip link set dev vth1 up
$ ip link set dev vth2 up
```
- To up the bridge
```
$ ip link set dev br0 up
$ ip -br -c link show
```
- To ping one namespace to other </br>
`$ ip netns exec ns1 ping 192.168.10.2`

**About veth**

Virtual Ethernet (veth) is a type of virtual network interface in the Linux operating system. It is a virtual version of a physical Ethernet interface and is created and managed by the operating system's networking stack.

Veth interfaces are often used to allow containers and other resources in different network namespaces to communicate with each other. When you create a veth interface, you can specify a "peer" option to create a second veth interface in a separate network namespace. This allows the two veth interfaces to communicate with each other and form a virtual link between the two network namespaces.

### **Packet Handling in Kernel**
The Linux kernel is responsible for translating between packets, and a coherent stream of data for programs.

**Netfilter**

Netfilter is a framework in the Linux kernel that provides packet filtering and manipulation capabilities for the network stack. It is used to control the flow of network traffic in and out of a system, as well as to modify the packets as they pass through the kernel.

Netfilter consists of several different components, including:

1. Hooks: Netfilter provides several points in the network stack (called "hooks") where packet filtering and manipulation can be performed. These hooks are located at different points in the network stack, such as at the input, output, and forward stages of packet processing.

2. Tables: Netfilter uses tables to store rules for packet filtering and manipulation. These rules are organized into chains, which are sequences of rules that are applied to packets in a specific order.

3. Targets: Targets are the actions that are taken when a packet matches a rule in a Netfilter chain. These actions can include accepting the packet, dropping the packet, or modifying the packet.

Netfilter is often used to implement firewall functionality in Linux systems, as well as to perform network address translation (NAT) and traffic shaping. It is a powerful and flexible tool that allows users to control and customize the behavior of the network stack in a Linux system.

**Conntrack**

Conntrack is a system in the Linux kernel that tracks the state of network connections. It is used to keep track of active network connections and the related session data, such as the protocol being used, the source and destination IP addresses and ports, and the connection state (e.g. established, closing, etc.).

Conntrack is used by the Linux kernel to maintain a connection tracking table that contains information about all active network connections. This table is used to manage the flow of traffic between the connections and to ensure that packets are correctly routed to their destination.

Conntrack is an important component of the Linux networking stack, as it allows the kernel to keep track of active connections and ensure that packets are delivered correctly. It is often used in conjunction with other tools, such as iptables, to implement firewall functionality and to perform network address translation (NAT).

To show all current flows $-$
</br>`$ conntrack -L`

**Routing**

Routing is the process of forwarding data packets between networks. It involves the selection of the best path for a packet to follow through a network based on a set of rules or criteria.

To show kernel IP routing table
</br> `$ route`
</br>or,
</br> `$ route -n`

### **High-Level Routing**
- iptables
- IPVS
- eBPF

[Let's read the book!!! :p ]

### Network Troubleshooting Tools
**ping**
- ping is a simple program that sends ICMP ECHO_REQUEST packets to networked
devices.
```
ICMP (Internet Control Message Protocol) is a network protocol that is used to communicate error messages and other information between network devices. It is a layer 3 (network layer) protocol in the OSI model and is often used in conjunction with the IP (Internet Protocol) to provide a range of services, such as error reporting, congestion control, and diagnostic functions.
```
- It is a common, simple way to test network connectivity from one host
to another.
- The basic use of ping is simply `ping <address>`. The address can be an IP address or a domain.
- ping will send a packet, wait, and report the status of that request when a response or timeout happens
- pings to a Kubernetes service will
always fail. Instead, you will need to use telnet or a higher-level tool such as cURL to check connectivity to a service. Individual pods may still be reachable by ping, depending on your network configuration.

**traceroute**
- traceroute shows the network route taken from one host to another
- traceroute displays hosts line by line, starting with the first external machine

**dig**
- dig is a DNS lookup tool
- The general form of a dig command is `dig [options] <domain>`

**telnet**
- telnet is both a network protocol and a tool for using said protocol
- telnet was once used for remote login, in a manner similar to SSH
- SSH has become dominant due to having better security, but telnet is still extremely useful fordebugging servers that use a text-based protocol
- The basic syntax of telnet is `telnet <address> <port>`
- To make full use of telnet, you will need to understand how the application protocol that you are using works. telnet is a classic tool to debug servers running HTTP, HTTPS, POP3, IMAP, and so on

**nmap**
- nmap is a port scanner, which allows you to explore and examine services on your network
- The general syntax of nmap is `nmap [options] <target>`, where target is a
domain, IP address, or IP CIDR

**netstat**
- netstat can display a wide range of information about a machine’s network stack and connections

**netcat**
- netcat is a multipurpose tool for making connections, sending data, or listening on a socket
- It can be helpful as a way to “manually” run a server or client to inspect what happens in greater detail

**Openssl**
</br>[Read the book :3 ]

**cURL**
- cURL is a data transfer tool that supports multiple protocols, notably HTTP and HTTPS
- cURL commands are of the form `curl [options] <URL>`
- See [this](https://www.freecodecamp.org/news/how-to-start-using-curl-and-why-a-hands-on-introduction-ea1c913caaaa/) for more 