## ConfigMaps and Secrets

**Configuring Containerized Application**
- Container images are build to be "portable"
- Containers expect configuration from
    - Configuration files
    - Command line arguments
    - Environment variables
- INI - XML - JSON - Custom Format

**ConfigMap**
- Decouples configuration from pods and components
- Stores configuration data as key-value pairs
    - Configuration files 
    - Command line arguments
    - Environment variables
- Similar to Secrets but don't contain sensitive information
- Must create a ConfigMap before referencing it in a Pod spec

**Create**
- `$ kubectl create configmap <map-name> <data-source>`
- Ex: `$ kubectl create configmap my-config --from-file=sample-yaml/demo-configmap.yaml`

or,
- `$ kubectl create configmap demo-config --from-literal=color.name=red --from-literal=color.code="danger ahead!"`

For `<data-source>`:
- Path to dir/file: `--from-file`
- Key-Value Pair: `--from-literal`

**Display**
- `$ kubectl get cm`
- `$ kubectl get configmaps my-config -o yaml`
- `$ kubectl describe cm my-config`

**ConfigMaps and Pods**

There are four different ways that you can use a ConfigMap to configure a container inside a Pod:

1. Inside a container command and args
2. Environment variables for a container
3. Add a file in read-only volume, for the application to read
4. Write code to run inside the Pod that uses the Kubernetes API to read a ConfigMap

```yaml
# pod-configmap-env.yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  containers:
  - image: busybox
    name: busybox
    command: ["/bin/sh"]
    args: ["-c", "sleep 600"]
    env:
    # Define the environment variable
    - name: COLORNAME # Notice that the case is different here from the key name in the configMap.
      valueFrom:
        configMapKeyRef:
          name: demo-config # The configMap this value comes from
          key: color.name # The key to fetch
    - name: COLORCODE
      valueFrom:
        configMapKeyRef:
          name: demo-config
          key: color.code
```

- `$ kubectl apply -f pod-configmap-env.yaml`
- `$ kubectl get pods`
- `$ kubectl exec -it busybox -- sh`
    - `# echo $COLORNAME`
    - `# echo $COLORCODE`
    - `# env | grep -i color`

```yaml
# pod-configmap-volume.yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  volumes:
  # set volumes at the Pod level, then mount them into containers inside that Pod
  - name: demo
    configMap:
      # Provide the name of the ConfigMap to mount.
      name: demo-config
  containers:
  - image: busybox
    name: busybox
    command: ["/bin/sh"]
    args: ["-c", "sleep 600"]
    volumeMounts:
    - name: demo # should be same as the name of volume
      mountPath: /mydata
```

- `$ kubectl apply -f sample-yaml/pod-configmap-volume.yaml`
- `$ kubectl exec -it busybox -- sh`
    - `# ls /mydata`
    - `# cat /mydata/color.name; echo`
    - `# cat /mydata/color.code; echo`

