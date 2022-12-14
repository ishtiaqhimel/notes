## Pods


**What is Pod?**
- Smallest unit of K8s for scheduling
- Abstraction over container, pod can group together container images into a single deployment unit, Usually 1 application per Pod unless there is dependency
- Pod is not a process but environment for running containers
- Each Pod gets its own IP address, new IP address on re-creation
- Usually we don't need to create Pod manually. Deployment, StatefulSet and DaemonSet manages pods. PodTemplates are specification for creating Pods and are included in workload resources such as Deployment, StatefulSet, DaemonSet.
- When the pod template is updated, the controller creates new pod based on updated template and replace the existing one.

**Pod Deployment**

![alt text](https://github.com/ishtiaqhimel/notes/blob/master/images/Pod-Deployment.png?raw=true "Pod Deployment")


**Pod Networking**
- Each pod gets its own IP address and all containers in pods run in different ports.
- The shared context of a Pod is a set of Linux namespaces, cgroups, and potentially other facets of isolation - the same things that isolate a container
- A Pod is similar to a set of containers with shared namespaces and shared filesystem volumes.

**Inter-Pod Communication VS Intra-Pod Communication**
- Containers inside a Pod can communicate between each other using localhost. Containers are not in same Pod can communicate using Pods IP addresses.
- To access a container we need to expose port of the container

**Pod Lifecycle**
- Each pod has five phases-
    - *Pending*: The Pod has been accepted by the Kubernetes cluster, but one or more of the containers has not been set up and made ready to run. This includes time a Pod spends waiting to be scheduled as well as the time spent downloading container images over the network.
    - *Running*: The Pod has been bound to a node, and all of the containers have been created. At least one container is still running, or is in the process of starting or restarting.
    - *Succeeded*: All containers in the Pod have terminated in success and will not be restarted.
    - *Failed*: All containers in the Pod have terminated, and at least one container has terminated in failure. That is, the container either exited with non-zero status or was terminated by the system.
    - *Unknown*: For some reason the state of the Pod could not be obtained. This phase typically occurs due to an error in communicating with the node where the Pod should be running.

**Manifest File**
```yaml
#demo-pod.yaml
apiVersion: v1
kind: Pod
metadata:
    name: nginx-pod
    labels:
        app: nginx
        tier: dev
spec:
    containers:
    - name: nginx-container
      image: nginx
```

**Create, Display and Describe**
- `$ kubectl create -f demo-pod.yaml`
- `$ kubectl get pod`
- `$ kubectl get pod -o wide`
- `$ kubectl get pod nginx-pod -o yaml`
- `$ kubectl describe pod nginx-pod`

**Basic Testing**
- `$ ping <pod_IP>`
- `$ kubectl exec -it nginx-pod -- /bin/sh`
    - `# hostname`
    - `# exit`

**Delete**
- `$ kubectl delete pod nginx-pod`

**Some Info**
- Some Pods have init containers as well as app containers
- Init containers run and complete before the app containers are started
- The kubelet refuses to run a Pod where you have specified a Pod OS, if this isn't the same as the operating system for the node where that kubelet is running
- Pod Topology Spread Constraint : Using topologyKey we can put pods on different zones and nodes.
- Pod Termination : API Server updates the pod status and the pod in the api server is considered dead beyond grace period(30s). Kubelet notices the pod updates and start graceful shutdown. Control Plane removes the shutting-down Pod from Endpoints. The pod can no longer provide service. Other objects no longer consider the pod valid. When the graceful shutdown period is over, kubelet triggers forcible shutdown.
- Pod Disruption Budget : Pod disruption budget tries to ensure a minimum number of pod running always. It prevents voluntary disruption such a directly deleting a pos or updating a deployment pod template causing restart. However, it can not prevent involuntary disruption such as hardware failure or kernel panic. Though it counts both disruption.
- “Will these containers work correctly if they land on different machines?” If the answer is no, a Pod is the correct grouping for the containers. If the answer is yes, using multiple Pods is probably the correct solution