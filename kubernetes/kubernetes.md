# Kubernetes Notes

## Basic Terminologies :

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

## K8s Architecture :

Kubernetes cluster have 2 types of nodes-
1. **Master Node/Control Plane**
2. **Worker Node**

![alt text](https://github.com/ishtiaqhimel/notes/blob/master/images/K8s-architecture.png?raw=true "Kubernetes Architecture")

### About Control Plane Components :

**API Server**
- Users interact with k8s cluster via API Server in master node
- Client of this api server can be Kubernetes Dashboard(UI) or Kubectl(CLI) or any script

**Scheduler**
- assigns pods to different nodes
- checks the requirements of pods and find a node which meets the requirement to run the pod
- schedules nodes so that the pods are distributed among all the nodes

**Controller Manager**
-  responsible for overall health of the cluster
-  responsible for overall health of the cluster
- Some types of these controllers are:
	- Node controller: Responsible for noticing and responding when nodes go down
	- Job controller: Watches for Job objects that represent one-off tasks, then creates Pods to run those tasks to completion
	- EndpointSlice controller: Populates EndpointSlice objects (to provide a link between Services and Pods)
	- ServiceAccount controller: Create default ServiceAccounts for new namespaces

**Cloud Controller Manager** 
- link cluster into cloud provider's API, and separates out the components that interact with that cloud platform from components that only interact with cluster

**etcd**
- brain of the cluster
- key-value storage system
- stores the current state of the cluster
- stores nodes, controller and all the state related things
- any component of cluster can query etcd

### About Worker Node Components :
**Kubelet**
- an agent that runs on each node in the cluster
- makes sure that containers are running in a Pod
- takes a set of PodSpecs that are provided through various mechanisms and ensures that the containers described in those PodSpecs are running and healthy
- tries to create a new pod if pod fails
- if it can not restart failed pod, master detects a node failure and schedule the pod in another node

**Kube-proxy**
- a network proxy that runs on each node in cluster
- responsible for maintaining network across the cluster
- allow network communication to Pods from network sessions inside or outside of cluster

**Container-Runtime (Docker)**
- runs the container inside the pod