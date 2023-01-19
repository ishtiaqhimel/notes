### Controller
In Kubernetes, controllers are control loops that watch the state of your cluster, then make or request changes where needed. Each controller tries to move the current cluster state closer to the desired state.

Controllers watch the shared state of your cluster through the apiserver (part of the Control Plane).

Some controllers also run inside the control plane, providing control loops that are core to Kubernetes' operations. For example: the deployment controller, the daemonset controller, the namespace controller, and the persistent volume controller (and others) all run within the kube-controller-manager.

- Controllers can act on core resources such as deployments or services, which are typically part of the Kubernetes controller manager in the control plane, or can watch and manipulate user-defined custom resources.
- Operators are controllers that encode some operational knowledge, such as application lifecycle management, along with the customresources.

### Control loop
In general, the control loop looks as follows:
1. Read the state of resources, preferably event-driven (using watches)
2. Change the state of objects in the cluster or the cluster-external world. For example, launch a pod, create a network endpoint, or query a cloud API.
3. Update status of the resource in step 1 via the API server in etcd.
4. Repeat cycle; return to step 1.

No matter how complex or simple your controller is, these three steps—**read resource state ˃ change the world ˃ update resource status**—remain the same.

![alt text](https://github.com/ishtiaqhimel/notes/blob/master/images/control-loop.png?raw=true "Controller")

**Informers**: Informers watch the desired state of resources in a scalable and sustainable fashion. They also implement a resync mechanism that enforces periodic reconciliation.
</br>**Work queues**: a component that can be used by the event handler to handle queuing of state changes and help to implement retries.
</br>**Events**: Kubernetes controllers watch changes to Kubernetes objects in the API server: adds, updates, and removes. When such an event happens, the controller executes its business logic.
</br>**Control Loops**: In the event of an unintended state change leading to a state drift, the system may autonomously determine and apply the set of mitigating actions leading to a state match. This process is called a control loop, a popular choice for the implementation of controllers.
</br>**k8s Mechanism**:
![alt text](https://github.com/ishtiaqhimel/notes/blob/master/images/deployment-control-loops.png?raw=true "Deployment Control Loops")
</br>**Operator**:An Operator is an application-specific controller that extends the Kubernetes API to create, configure, and manage instances of complex stateful applications on behalf of a Kubernetes user. It builds upon the basic Kubernetes resource and controller concepts but includes domain or application-specific knowledge to automate common tasks.

