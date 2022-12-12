## DaemonSet

**Set up**
- At first, create working nodes

```yaml
# three node (two workers) cluster config
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```
**Create Cluster**
- `$ kind create cluster --name k8s-playground --config kind-config.yaml`

**Delete Cluster**
- `$ kind delete cluster --name k8s-playground`

**Overview**
- ensures that all (or some) nodes run a copy of a pod
- As nodes are added to the cluster, pods are added 
- As nodes are remobved from the cluster, those pods are garbage collected
- Deleting a DaemonSet will clean up the pods it created
- Use Cases:
    - Node monitoring daemons. Ex- collectd
    - Log collection daemons. Ex- fluentd
    - storage daemons. Ex- ceph

**Manifest File**
```yaml
#flentds.yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
    name: fluentd-ds
spec:
    template:
        metadata:
            labels:
                name: fluentd
        spec:
            containers:
            - name: fluentd
              image: gcr.io/google-containers/fluentd-elasticsearch:1.20
    selector:
        matchLabels:
            name: fluentd
```

**Create and Display**
- `$ kubectl create -f fluentd-daemonset.yaml`
- `$ kubectl get pods -o wide`
- `$ kubectl get ds`

**Delete**
- `$ kubectl delete ds fluentd-ds`