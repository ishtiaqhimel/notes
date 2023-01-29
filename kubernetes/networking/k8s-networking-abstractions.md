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
- There are four types of services, specified by the `.spec.Type` field. 
- The four types are: ClusterIP, NodePort, LoadBalancer, and ExternalName

### ClusterIP
- default type
- each worker node will get a IP range, Ex. 10.2.1.x, 10.2.2.x, 10.2.3.x etc
- To see pod IP `kubectl get pod -o wide`
- how service know which pods to forward the request to? 
    - pods are identified via "selectors"
    - key value pairs
    - labels of pods
    - random label names
    - let we have 3 replicas then svc matches all 3 replica
    - registers as Endpoints
    - must match all the selectors
- how service know which port to forward it to?
    - defined in `targetPort` attribute
- service endpoints
    - k8s creates endpoint object
    - same name as service
    - keeps track of, which pods are the members/ endpoints of the service
- A particular use case example for ClusterIP is when a workload requires a load balancer within the same cluster.

### NodePort
- A nodeport service provides a simple way for external software, such as a load balancer, to route traffic to the pods
- The software only needs to be aware of node IP addresses, and the service's port(s).
- A nodeport service exposes a fixed port on all nodes, which routes to applicable pods.
- The nodeport field can be left blank, in which case k8s automatically selects a unique port. (range 30000-32767)
- ExternalTrafficPolicy indicates how a service will route external traffic to either node-local or cluster-wide endpoints.

### Headless
- A headless service isn't a formal type of service (i.e., there is no `.spec.type:Headless`). A headless service is a service with `.spec.clusterIP: "NONE"`.
- When ClusterIP is set to None, the service does not support any load balancing functionality. Instead, it only provisions an Endpoints object and points the service DNS record at all pods that are selected and ready.
- Headless services allow developers to deploy multiple copies of a pod in a deployment. Instead of a single IP address returned, like with the ClusterIP service, all the IP addresses of the endpoint are returned in the query.
- Headless has a specific use case and is not typically used for deployments. As we mentioned in “StatefulSets”, if developers need to let the client decide which endpoint to use, headless is the appropriate type of service to deploy.

### ExternalName
- ExternalName is a special type of service that does not have selectors and uses DNS names instead.

### LoadBalancer
LoadBalancer service exposes services external to the cluster network. They combine the NodePort service behavior with an external integration, such as a cloud provider’s load balancer. Notably, LoadBalancer services handle L4 traffic (unlike ingress, which handles L7 traffic), so they will work for any TCP or UDP service, provided the load balancer selected supports L4 traffic.

## Ingress
- Ingress is a Kubernetes-specific L7 (HTTP) load balancer, which is accessible externally, contrasting with L4 ClusterIP service, which is internal to the cluster.
- We call ingress implementations ingress controllers. In Kubernetes, a controller is software that is responsible for managing a typical resource type and making reality match the desired state.
</br>There are two general kinds of controllers: external load balancer controllers and internal load balancer controllers. External load balancer controllers create a load balancer that exists “outside” the cluster, such as a cloud provider product. Internal load balancer controllers deploy a load balancer that runs within the cluster and do not directly solve the problem of routing consumers to the load balancer. 

## Service Mesh
 A service mesh is an API-driven infrastructure layer for handling service-to-service communication.