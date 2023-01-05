## K8s Networking Intro
- K8s networking model natively supports multihost cluster networking
- Pods can communicate with each other by default, regardless of which host they are deployed on
- Kubernetes users typically do not create pods directly. Instead, users create a high-level workload, such as a deployment, which manages pods according to some intended spec
- There are several other ways to manage workloads such as ReplicaSets and StatefulSets
- There are also third-party workload types, in the form of custom resource definitions (CRDs)
- The CNI plugin is what manages pod IP addresses and individual container network provisioning.
- The CNI plugin is responsible for assigning pod IP addresses and maintaining a route between all (applicable) pods.

### Node and Pod Network Layout
- The cluster must have a group of IP addresses that it controls to assign an IP address to a pod
- Nodes and pods must have L3 connectivity in this IP address space
- Generally, pods do not have MAC addresses. Therefore, L2 connectivity to pods is not possible. The CNI will determine this for pods
- There are three approaches to structure a cluster's network: isolated, flat and island networks

### Kubelet
- The Kubelet is a single binary that runs on every worker node in a cluster
- responsible for managing any pods scheduled to the node and providing status updates for the node and pods on it
- Kubelet manages a container networking implementation (via the CNI) and a container runtime (via the CRI)
- Kubelet makes an ADD call to the CNI, which tells the CNI plugin to create the pod network

### Pod Readiness and Probes
Pod phases and their descriptions-
- *Pending*: The pod has been accepted by the cluster, but one or more of the containers has not been set up and made ready to run. This includes the time a pod spends waiting to be scheduled as well as the time spent downloading container images over the network.
- *Running*: The pod has been scheduled to a node, and all the containers have been created. At least one container is still running or is in the process of starting or restarting. Note that some containers may be in a failed state, such as in a CrashLoopBackoff.
- *Succeeded*: All containers in the pod have terminated in success and will not be restarted.
- *Failed*: All containers in the pod have terminated, and at least one container has terminated in failure. That is, the container either exited with nonzero status or was terminated by the system.
- *Unknown*: For some reason the state of the pod could not be determined. This phase typically occurs due to an error in communicating with the Kubelet where the pod should be running.

Kubernetes provides probes (health checks) to monitor and act on the state of Pods (Containers) and to make sure only healthy Pods serve traffic.

The Kubelet performs several types of health checks (Probes) for individual containers in a pod: liveness probes (livenessProbe), readiness probes (readinessProbe), and startup probes (startupProbe).

The probes can be exec probes, which attempt to execute a binary within the container, TCP probes, or HTTP probes.

- When a container’s readiness probe fails, the Kubelet does not terminate it. Instead, the Kubelet writes the failure to the pod’s status
- If the liveness probes fail, the Kubelet will terminate the container.
- A startup probe can provide a grace period before a liveness probe can take effect. Liveness probes will not terminate a container before the startup probe has succeeded

### CNI Specification 
Communication between the pods and the Kubelet is made possible by the CNI. There are four operations that a CNI plugin must support-
- ADD: Add a container to the network
- DEL: Delete a container from the network
- CHECK: Return an error if there is a problem with the container’s network
- VERSION: Report version information about the plugin

### CNI Plugins
- The CNI plugin has two primary responsibilities: allocate and assign unique IP addresses for pods and ensure that routes exist within Kubernetes to each pod IP address
- There are two broad categories of CNI network models: flat networks and overlay networks.
- The CNI plugin is also responsible for calling IPAM plugins for IP addressing.
- The CNI spec has a second interface, the IP Address Management (IPAM) interface, to reduce duplication of IP allocation code in CNI plugins.

### Kube-proxy
- kube-proxy is another per-node daemon in Kubernetes, like Kubelet. kube-proxy provides basic load balancing functionality within the cluster.
- Most types of services have an IP address for the service, called the cluster IP address, which is not routable outside the cluster.
- kube-proxy is responsible for routing requests to a service’s cluster IP address to healthy pods.
- kube-proxy has four modes, which change its runtime mode and exact feature set: **userspace**, **iptables**, **ipvs**, and **kernelspace**.

### NetworkPolicy
- NetworkPolicy is a resource type in Kubernetes that contains allow-based firewall rules.
- Users can add NetworkPolicy objects to restrict connections to and from pods.