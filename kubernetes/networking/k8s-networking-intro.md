## K8s Networking Intro
- K8s networking model natively supports multihost cluster networking
- Pods can communicate with each other by default, regardless of which host they are deployed on
- Kubernetes users typically do not create pods directly. Instead, users create a high-level workload, such as a deployment, which manages pods according to some intended spec
- There are several other ways to manage workloads such as ReplicaSets and StatefulSets
- There are also third-party workload types, in the form of custom resource definitions (CRDs)
- The CNI plugin is what manages pod IP addresses and individual container network provisioning.
- The CNI plugin is responsible for assigning pod IP addresses and maintaining a route between all (applicable) pods.

### Node and Pod Network Layout
