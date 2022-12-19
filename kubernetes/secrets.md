## Secrets
A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Because Secrets can be created independently of the Pods that use them, there is less risk of the Secret (and its data) being exposed during the workflow of creating, viewing, and editing Pods. Secret can be mounted as data volumes or exposed as environment variables to be used by a container in a Pod.

Secrets are similar to ConfigMaps but are intended to hold confidential data

By default, Kubernetes Secrets are stored in plain text in the etcd storage for the cluster. In particular, anyone who has cluster administration rights in your cluster will be able to read all of the Secrets in the cluster. In recent versions of Kubernetes, support has been added for encrypting the Secrets with a user-supplied key, generally integrated into a cloud key store.

There are three main ways for a Pod to use a Secret:

1. As files in a volume mounted on one or more of its containers.
2. As container environment variable.
3. By the kubelet when pulling images for the Pod.

**Create and Display**
```yaml
# demo-secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret-demo
type: Opaque
data:
  # base64 encoded
  username: a3ViZWFkbWlu
  password: cGFzc3dvcmQ=
```

- `$ kubectl create -f demo-secret.yaml`
- `$ kubectl get secret secret-demo -o yaml`
- `$ kubectl describe secret secret-demo`

**Types of Secret**
- **Opaque secrets** : Default Secret type to store any key-value pair.
- **Service Account Token Secrets** : It is used to store a token that identifies a service account.
- **Docker Config Secrets** : Stores credentials for accessing Docker registry for images. `~/.coker/config.json` files is provided as base64 encoded string in the secret.
- **Basic authentication Secret** : Contains `username` and `password` for basic authentication
- **SSH authentication secrets** : Used for passing `ssh-privateke` for SSH authentication.
- **TLS secrets** : It can be used for storing a certificate(`tls.crt`) and its associated key(`tls.key`) that are typically used for TLS. Can be created form command line using `tls` subcommand.
- **Bootstrap token Secrets** : It is designed for tokens used during the node bootstrap process.

**Secrets and Pods**
```yaml
# pod-secret-env.yaml
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
    - name: myusername
      valueFrom:
        secretKeyRef:
          name: secret-demo
          key: username

```
```yaml
# pod-secret-volume.yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  volumes:
  - name: secret-volume
    secret:
      secretName: secret-demo
  containers:
  - image: busybox
    name: busybox
    command: ["/bin/sh"]
    args: ["-c", "sleep 600"]
    volumeMounts:
    - name: secret-volume
      mountPath: /mydata
```