## Using Custom Resources
Custom Resources (CR) is one of the central extension mechanisms used throughout the Kubernetes ecosystem. Custom resources are used for small, in-house configuration objects without any corresponding controller logic—purely declaratively defined. CRs are stored in the same etcd instance as the main Kubernetes API resources and served by the same Kubernetes API server.
</br>A CustomResourceDefinition (CRD) is a Kubernetes resource itself. It describes the available CRs in the cluster.

### Advanced Features of CRS
- Validating CRS
- Short Names and Categories
- Printer Columns
- Subresources

### A Developer’s View on CRs
- client-go dynamic client
- typed client

### Dynamic Client
- totally agnostic to know GVKs
- does not even use any Go types other than `map[string]interface{}`, which wraps just json.Unmarshal and its output
- dynamic client makes use of neither a scheme nor a RESTMapper
- developer has to provide all the knowledge about types manually by providing a resource in the form of a GVR

### Typed Clients
- Typed clients do not use `map[string]interface{}`-like generic data structures but instead use real Golang types, which are different and specific for each GVK.
- more easy to use, have considerably increased type safety, and make code much more concise and readable
- they are less flexible because the processed types have to be known at compile time, and those clients are generated, and this adds complexity

### controller-runtime Client of Operator SDK and Kubebuilder
- controller-runtime project provides the basis for the operator solutions Operator SDK and Kubebuilder