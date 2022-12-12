## Deployments

**Intro**

- Just as we learned that ReplicaSets manage Pods, Deployments manage ReplicaSets
- As with all relationships in Kubernetes, this relationship is defined by labels and a label selector

![alt text](https://github.com/ishtiaqhimel/notes/blob/master/images/deployment.png?raw=true "Deployment")

Imagine, we are upgrading application from v1 to v2\
Can we -
- Upgrade with zero downtime?
- Upgrade sequentially, one after the other?
- Pause and resume upgrade process?
- Rollback upgrade to previous stable release?


**Features**
- Multiple Replicas
- Upgrade
- Rollback
- Scale up or down
- Pause and Resume

**Strategy Types**
- Recreate
- RollingUpdate (Ramped or Incremental)
- Canary
- Blue / Green


**Manifest file**
```yaml
# nginx-deploy.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: nginx-deploy
    labels:
        app: nginx-app
spec:
    replicas: 3
    selector: 
        matchLabels: 
            app: nginx-app
    template:
        metadata:
            name: nginx-pod
            labels:
                app: nginx-app
        spec:
            containers:
            - name: nginx-container
              image: nginx:1.7.9
              ports: 
                - containerPort: 80
```

**Create and Display**
- `$ kubectl apply -f nginx-deploy.yaml`
- `$ kubectl get deployments -l app=nginx-app`
- `$ kubectl get rs -l app=nginx-app`
- `$ kubectl get pods -l app=nginx-app`

**Update Deployment**
> nginx:1.7.9 -> nginx:1.9.1
- `$ kubectl set image deploy nginx-deploy nginx-container=nginx:1.9.1`

or,

- `$ kubectl edit deploy nginx-deploy`
- `$ kubectl rollout status deployment/nginx-deploy`

or,
- Just update the yaml file and write apply command

**Rollback Deployment**
> nginx:1.7.9 -> nginx:1.91
- `$ kubectl set image deploy nginx-deply nginx-container=nginx:1.9.1 --record`
- `$ kubectl rollout status deployment/nginx-deploy`
- `$ kubectl rollout history deployment/nginx-deploy`
- `$ kubectl rollout undo deployment/nginx-deploy`
- `$ kubectl rollout status deployment/nginx-deploy`


**Scale up and down**
- `kubectl scale deployments nginx-deploy --replicas=5`
- `kubectl get deployments`

**Delete**
- `kubectl delete -f nginx-deploy.yaml`
- `kubectl get pod -l app=nginx-app`