## Containers Network
### Container
A container is a lightweight, standalone, and executable package that includes everything an application needs to run, including the application code, libraries, dependencies, and runtime. Containers allow developers to package an application with all of its dependencies and run it in any environment, including on different operating systems and hardware architectures, without worrying about compatibility issues.

Containers are an abstraction of the operating system kernel's features, such as resource isolation, memory management, and namespace management. They enable applications to run in a controlled and predictable environment, making them ideal for deploying and running microservices, which are small, modular, and independently deployable units of software that make up a larger application.

Containers have become popular in recent years because they offer several benefits, including:

- **Portability**: Containers can be easily moved between environments, such as from a developer's laptop to a staging environment to a production environment, without the need to re-configure the application.

- **Efficiency**: Containers use less resources than traditional virtual machines, making them more efficient and faster to start up.

- **Scalability**: Containers can be easily scaled up or down to meet changing demand, making them ideal for use in modern, cloud-native architectures.

- **Isolation**: Containers provide a level of isolation between applications, enabling them to run on the same host without interference or conflicts.

Overall, containers provide a convenient and effective way to package and deploy applications, making it easier to build, test, and deploy software quickly and reliably.

### Container Primitives
- Each of our containers has linux primitives known as control groups and namespaces
- cgroups control access to resources in the kernel for our containers 
- namespaces are individual slices of resources to manage separately from the root namespaces. i.e., the host
- A cgroup controls how much of a resource a container can use, while namespaces control what processes inside the container can see.

#### Control groups
- a cgroup is a Linux kernel feature that limits, accounts for, and isolates resource (CPU, Memory, Network, Disk IO) usage
- what we can use
#### Namespaces
- Namespaces are features of the Linux kernel that isolate and virtualize system resources of a collection of processes
- What we can see (to use)

### Container Network Basics
- Several network modes:
    - None
    - Bridge (default)
    - Host
    - Macvlan
    - IPvlan
    - Overlay
    - Custom
