## Code Generation
Here, all means to call all four standard code generators for CRs:
</br>*deepcopy-gen*
</br>Generates func (t *T) DeepCopy() *T and func (t *T) DeepCopyInto(*T) methods.
</br>*client-gen*
</br>Creates typed client sets.
</br>*informer-gen*
</br>Creates informers for CRs that offer an event-based interface to react to changes of CRs on the server.
</br>*lister-gen*
</br>Creates listers for CRs that offer a read-only caching layer for GET and LIST requests.

## Controlling the Generators with Tags
A tag is a specially formatted Go comment in the following form:
```go
// +some-tag
// +some-other-tag=value
```
There are two kind of tags:
- Global tags above the package line in a file called doc.go
- Local tags above a type declaration (e.g., above a struct definition)

### Global Tags
Global tags are written into a package’s doc.go. A typical `pkg/apis/group/version/doc.go` file looks like this:
```go
// +k8s:deepcopy-gen=package
// Package v1 is the v1alpha1 version of the API.
// +groupName=cnat.programming-kubernetes.info
package v1alpha1
```
- The first line of this file tells deepcopy-gen to create deep-copy methods by default for every type in that package.
- The second tag, `// +groupName=example.com`, defines the fully qualified API group name.

### Local Tags
Local tags are written either directly above an API type or in the second comment block above it.