## RBAC
*RBAC (Role-Based Access Control)* is a method of regulating access to resources in a Kubernetes cluster. It allows administrators to control which users and processes have access to specific resources and actions within the cluster.

In RBAC, users and processes are granted permissions through the assignment of roles. Roles can be created and customized to suit the specific needs of an organization, and they can be bound to users or processes (called "subjects") through role bindings.

RBAC can be used to control access to:

- *Namespaces*: A namespace is a virtual environment within a cluster where resources such as pods and services can be created and managed. RBAC can be used to control which users have access to specific namespaces.
- *Resources*: RBAC can be used to control which users have the ability to create, view, update, or delete specific resources within a namespace.

- *Cluster-level resources*: RBAC can also be used to control access to cluster-level resources, such as nodes and persistent volumes.

RBAC is an important security feature in Kubernetes, as it helps to ensure that only authorized users and processes have access to the resources they need. It is recommended to use RBAC in conjunction with other security measures, such as network policies, to ensure the security of a Kubernetes cluster.

### Using built-in roles

Designing your own roles can be complicated and time-consuming. Kubernetes has a large number of built-in cluster roles for well-known system identities (e.g., a scheduler) that require a known set of capabilities. You can view these by running:

- `$ kubectl get clusterroles`

While most of these built-in roles are for system utilities, four are designed for generic end users:

- The **cluster-admin** role provides complete access to the entire cluster.
- The **admin** role provides complete access to a complete namespace.
- The **edit** role allows an end user to modify resources in a namespace.
- The **view** role allows for read-only access to a namespace.

Most clusters already have numerous ClusterRole bindings set up, and you can view these bindings with 
- `$ kubectl get clusterrolebindings`

### Managing RBAC
**Testing Authorization with can-i**
- `$ kubectl auth can-i create pods`
- `$ kubectl auth can-i get pods --subresource=logs`

**Managing RBAC in Source Control**
- `$ kubectl auth reconcile -f <some-rbac-config.yaml>`