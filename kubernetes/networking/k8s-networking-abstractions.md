## Kubernetes Networking Abstraction
### StatefulSet
- StatefulSets require a service, preferably headless, to be responsible for the network identity of the pods, and end users are responsible for creating this service.
- StatefulSets offer functionality for a specific use case in Kubernetes. They should not be used for everyday application deployments.

### Endpoints
- Endpoints help identify what pods are running for the service it powers
- created and managed by services
- a service contains a standard label selector, , which defines which pods are in the endpoints.
- Each endpoint contains a list of ports (which apply to all pods) and two lists of addresses: ready and unready.
- Addresses are listed in `.addresses` if they are passing pod readiness checks. Addresses are listed in `.notReadyAddresses` if they are not. This makes endpoints a service discovery tool, where you can watch an Endpoints object to see the health and addresses of all pods

### EndpointSlices
> Why EndpointSlices?

In a typical cluster, Kubernetes runs kube-proxy on every node. kube-proxy is responsible for the per-node portions of making services work, by handling routing and outbound load balancing to all the pods in a service. To do that, kube-proxy watches all endpoints in the cluster so it knows all applicable pods that all services should route to.
In a big cluster, there can be a thousand of node and tens of thousand of pods. That means a thousand of kube-proxy watching the endpoint. When the address of the pod updated it affects the whole endpoint. The address updates rapidly which cause a great strain on etcd. To resolve this issue endpoint slices are introduced. EndpointSlices is an array of list of address.
A Kubernetes service creates one endpoint for all pods in the service. A service creates multiple endpoint slices, each containing a subset of pods

### K8s Services
- A service in Kubernetes is a load balancing abstraction within a cluster. 
- There are four types of services, specified by the .spec.Type field. 
- The four types are: ClusterIP, NodePort, LoadBalancer, and ExternalName

### NodePort
