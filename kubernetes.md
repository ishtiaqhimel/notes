# Kubernetes Notes

### Basic Terminologies :

**Kubernetes**
- an open source orchestrator for deploying containerized applications
- originally developed by Google
- provides with service discovery and load balancing, storage orchestration, automated rollouts and rollbacks, automatic bin packing, self-healing, secret and configuration management 

**Cluster**
- set of nodes running an applications
- consist of one master node and set of worker nodes
- nodes can be either physical computers or virtual machines
- master node controls the state of the cluster

**Node**
- Kubernetes runs workload by placing container into Pods to run on Nodes
- A node may be a virtual or physical machine depending on the cluster

**Pod**
- Smallest unit of K8s
- Abstraction over container
- Pod can group together container images into a single deployment unit
- Usually 1 application per Pod unless there is dependency
- Each Pod gets its own IP address
- New IP address on re-creation 
- Thus another component of K8s called **service** is used

**Sevice**
- abstract way to expose an application running on a set of Pods as a network service
- own permanent IP address
- life cycle of Pod and service are not connected, so if pod dies the service and its IP address will stay

**Ingress** 
- an API object that manages external access to the services in a cluster, typically HTTP
- may provide load balancing, SSL termination and name-based virtual hosting

**ConfigMap**
- external configuration of  application
- store in plain text format

**Secret**
- used to store secret data
- base64 encoded

**Volumes**
- On-disk files in a container are ephemeral
	- one problem is the loss of files when a container crashes, the kubelet restarts the container but with a clean state
	- second problem occurs when sharing files between containers running together in a Pod
- volume abstraction solves both of these problems

**Namespace**
- provide isolation and access control, so that each microservice can control the degree to which other services interact with it
- a way to a Kubernetes user to organize different clusters within just one physical cluster

### K8s Architecture

