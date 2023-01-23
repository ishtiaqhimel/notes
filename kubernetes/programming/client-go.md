## Basic of client-go
The Kubernetes project provides a number of third-party consumable Git repositories under the kubernetes organization on GitHub. You’ll need to import all of these with the domain alias k8s.io/… (not github.com/kubernetes/…) into your project.

## The Repositories
### The Client Library
The Kubernetes programming interface in Go mainly consists of the **k8s.io/client-go** library. client-go is a typical web service client library that supports all API types that are officially part of Kubernetes.
### Kubernetes API Types
The Kubernetes API Go types
for objects like pods, services, and deployments are located in their own repository. It is accessed as **k8s.io/api** in Go code.
### API Machinery
Last but not least, there is a third repository called API Machinery, which is used as **k8s.io/apimachinery** in Go. It includes all the generic building blocks to implement a Kubernetes-like API.

## Creating a Client
### Out of Cluster
To access resources in a K8s cluster from local environment, we need to configure client with server name, credentials etc. The kubeconfig file contains these credentials. The default location for the kubeconfig file is in **~/.kube/config** directory. (This is also where kubectl gets the credentials for the Kubernetes clusters.)
</br>To configure the kubeconfig file, we import **clientcmd** from client-go in order to read and parse the
kubeconfig file. Then we import the client-go **kubernetes** package with the client sets for Kubernetes resources (It’s called a client set because it contains multiple clients for all native Kubernetes resources). Then get the "example" pod in "book" namespace. Here we import the **meta/v1** from apimachinery to access metav1.GetOptions.
```go
kubeconfig = flag.String("kubeconfig", "~/.kube/config", "kubeconfig file")
flag.Parse()
config, err := clientcmd.BuildConfigFromFlags("", *kubeconfig)
clientset, err := kubernetes.NewForConfig(config)
pod, err := clientset.CoreV1().Pods("book").Get("example", metav1.GetOptions{})
```
### In Cluster
Where we will access K8s cluster (or namespace in case) from a pod in the cluster. We will run a binary inside of a pod in the cluster, the *kubelet* will automatically mount a service account into the container at `/var/run/secret/kubernetes.io/serviceaccount`. It replaces the kubeconfig file just mentioned and can easily be turned into a rest.Config via the `rest.InClusterConfig()` method.
```go
config, err := rest.InClusterConfig()
clientSet, err := kubernetes.NewForConfig(config)
pod, err := clientset.CoreV1().Pods("book").Get("example", metav1.GetOptions{})
```
## K8s Objects in Go
Kubernetes resources or more precisely the objects that are instances of a kind and are served as a resource by the API server are represented as structs. In other words, a Kubernetes object in Go is a data structure that can:
- Return and set the GroupVersionKind (GVK)
- Be deep-copied

A **deep copy** is a clone of the data structure such that it does not share any memory with
the original object. It is used wherever code has to mutate an object without modifying
the original.

### TypeMeta
- kind
- APIVersion

kind is just the Go type name, which could be derived from an object via reflection (except custom resources).
</br>group is just the Go package name(types for the apps API group are declared in k8s.io/api/apps).

### ObjectMeta
In addition to TypeMeta, most top-level objects have a field of type `metav1.ObjectMeta`. In JSON or YAML these fields are under metadata. it contains all metalevel information like name, namespace, resource version (not to be confused with the API group version), several timestamps, and the well-known labels and annotations is part of ObjectMeta.

### spec and status
Finally, nearly every top-level object has a spec and a status section. This convention comes from the declarative nature of the Kubernetes API: spec is the user desire, and status is the outcome of that desire, usually filled by a controller in the system.

## Client Sets
A client set gives access to clients for multiple API groups and resources. In the case of `kubernetes.NewForConfig(config)` from `k8s.io/client-go/kubernetes`, we get access to all API groups and resources defined in `k8s.io/api`.

## Informers and Caching
Informers give a higher-level programming interface for the most common use case for watches: in-memory caching and fast, indexed lookup of objects by name or other properties in-memory.
### Work Queue
A work queue is a data structure. You can add elements and take elements out of the queue, in an order predefined by the queue. Formally, this kind of queue is called a priority queue. client-go provides a powerful implementation for the purpose of building controllers in `k8s.io/client-go/util/workqueue`.