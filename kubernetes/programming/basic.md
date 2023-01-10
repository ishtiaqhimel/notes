## K8s API Basic
The Kubernetes API is a resource-based (RESTful) programmatic interface provided via HTTP. It supports retrieving, creating, updating, and deleting primary resources via the standard HTTP verbs (POST, PUT, PATCH, DELETE, GET).

**API Groups:** API groups make it easier to extend the Kubernetes API. The API group is specified in a REST path and in the `apiVersion` field of a serialized object.
</br>There are several API groups in k8s-
- The core (also called legacy) group is found at REST path `/api/v1`. The core group is not specified as part of the `apiVersion` field, for example, `apiVersion: v1`.
- The named groups are at REST path `/apis/$GROUP_NAME/$VERSION` and use `apiVersion: $GROUP_NAME/$VERSION` (for example, `apiVersion: batch/v1`).

**K8s API terminology:** Kubernetes generally leverages common RESTful terminology to describe the API concepts:
- A resource type is the name used in the URL (pods, namespaces, services)
- All resource types have a concrete representation (their object schema) which is called a kind
- A list of instances of a resource is known as a collection
- A single instance of a resource type is called a resource, and also usually represents an object
- For some resource types, the API includes one or more sub-resources, which are represented as URI paths below the resource

**How the API Server Processes Requests?**
</br>The Kubernetes API server is the central point of communication for all Kubernetes clusters. It exposes a RESTful API that allows clients, such as kubectl, to interact with the cluster's resources.

When a client sends a request to the API server, the request is first handled by the API server's reverse proxy, which is typically implemented using a web server like NGINX or Apache. The reverse proxy is responsible for routing the request to the appropriate handler based on the URL path and HTTP method.

Next, the request is passed to the Kubernetes API server's authentication and authorization module, which is responsible for determining if the client is authorized to access the requested resource. This module can be configured to use various authentication mechanisms, such as client certificates, tokens, or OAuth2.

If the client is authenticated and authorized, the request is passed to the API server's request handler. The request handler is responsible for parsing the request, validating the client's input, and forwarding the request to the appropriate controller or storage backend.

The controller is a component that watches for changes to resources and makes updates to the desired state, where as storage backend maintains the actual state of cluster's resources. API server uses etcd as a storage backend.

The API server returns a response to the client, indicating whether the request was successful or not. If the request resulted in a change to a resource, the API server will also return the updated state of the resource.

It's a complex process and also may involve several other components to handle more specific operations like kubelet, pod and scheduler. In general Kubernetes API server acts as a bridge between the client and the cluster components and makes sure the interactions are secure and authorized.

[Note: The content type for all of these requests is usually text-based JSON (application/json) but recent releases of Kubernetes also support Protocol Buffers (Protobuf) binary encoding. Generally speaking, JSON is better for human-readable and debuggable traffic on the network between client and server, but it is significantly more verbose and expensive to parse. Protocol Buffers are harder to introspect using common tools, like curl, but enable greater performance and throughput of API requests.]

**Resource Deletion:** When you delete a resource this takes place in two phases.
- finalization
- removal
```
{
  "kind": "ConfigMap",
  "apiVersion": "v1",
  "metadata": {
    "finalizers": {"url.io/neat-finalization", "other-url.io/my-finalizer"},
    "deletionTimestamp": nil,
  }
}
```
When a client first sends a delete to request removal of a resource, the `.metadata.deletionTimestamp` is set to the current time. Once the `.metadata.deletionTimestamp` is set, external controllers that act on finalizers may start performing their cleanup work at any time, in any order.

Order is not enforced between finalizers because it would introduce significant risk of stuck `.metadata.finalizers`.

The `.metadata.finalizers` field is shared: any actor with permission can reorder it. If the finalizer list were processed in order, then this might lead to a situation in which the component responsible for the first finalizer in the list is waiting for some signal (field value, external system, or other) produced by a component responsible for a finalizer later in the list, resulting in a deadlock.

Without enforced ordering, finalizers are free to order amongst themselves and are not vulnerable to ordering changes in the list.

Once the last finalizer is removed, the resource is actually removed from etcd.

**Libraries Used for K8s API objects using Go**
- Client-go
- api
- apimachinery