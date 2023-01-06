## StatefulSets
StatefulSet for stateful applications.
</br>Stateful applications-
- databases
- applications that stores data

Stateless applications-
- don't keep record of state
- each request is completely new

### Deployment VS StatefulSet
| Deployment | StatefulSet |
|:-----|:----|
| identical and interchangable | not identical and not interchangable (though created from same specification) |
| created/deleted in random order with random hashes | can't be created/deleted at same time and can't be randomly addressed |
| has random hashers, like: my-app-asnhcfu7, my-app-yshbch34 | sticky identity for each pod, like: my-app-0, my-app-1, my-app-2 and persistent identifier across any re-scheduling |
| one Service that load balances to any Pod | StatefulSets currently require a Headless Service to be responsible for the network identity of the Pods. You are responsible for creating this Service. |

StatefulSets are valuable for applications that require one or more of the following-
- Stable, unique network identifiers.
- Stable, persistent storage.
- Ordered, graceful deployment and scaling.
- Ordered, automated rolling updates.

In the above, stable is synonymous with persistence across Pod (re)scheduling. If an application doesn't require any stable identifiers or ordered deployment, deletion, or scaling, you should deploy your application using a workload object that provides a set of stateless replicas. Deployment or ReplicaSet may be better suited to your stateless needs.

### Why statefulset?
Let say we created 3 replicas of database pods. We can't write in the 3 pods simultaneously. It will create data inconsistency. So we need to follow master-slave mechanism (only master has write access and slaves must now about each change to be up-to-date). So, not all pods are identical and they do not use the same physical storage and they are continuosly syncronizing of the data. Now if we scale the application, the new slave pod always clone (sync) from the previous pod. Here we must use Persistent volume to store data as "Persistent Volume lifecycle isn't tied to other component's lifecycle". So, if all pods get wiped out the data will remain safe.

For two characteristics 1) predictable pod name and 2) fixed individual DNS name , when Pod restarts ip address changes but name and endpoint stays same. (Thats why sticky identity!!!)

**Stateful applications not perfect for containerized environments**

### How to create?
- `kubectl apply -f demo-statefulset.yaml`
