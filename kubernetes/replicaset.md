## ReplicaSet

**Replication Controller**
- Ensures that a specified numberof pods are running at any time.
- Replication controllers and Pods are associated with 'Labels'.
- Advantages: High availability, load balancing
- Supports equality-based selectors

**ReplacaSet**
- Ensures that a specified numberof pods are running at any time.
- ReplicaSets and Pods are associated with 'Labels'.
- Replication controller is replaced by replicaset (next-generation replication controller)
- Supports set-based selectors

**Equality-based VS Set-Based**

|  | Equality-based        | Set-Based     |
| :---- |:-------------|:---------|
| Operators  |  = == !=      | in notin exists |
| Example      | environment=production <br/> tier!=frontend      | environment in (production,qa) <br/> tier notin (frontend, backend) |
| Command line | `$ kubectl get pods -l environment=production` | `$kubectl get pods -l environment in (production)` |
| In manifest | <pre>`selector:`<br>  `environment: production`<br>  `tier: frontend`</pre> |<pre>`selector:`<br> `matchExpressions:`<br>  `- {key: environment, operator: In, values:[prod, qa]}`<br>  `- {key: tier, operator: NotIn, values: [frontend, backend]}`</pre> |
|Supports | Services, Replication Controller | Job, Deployment, ReplicaSet, and DaemonSet|

**Manifest file**
```yaml
# nginx-rs.yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
    name: nginx-rs
spec:
    replicas: 3
    selectors: 
        matchLabels: 
            app: nginx-app
        matchExpressions:
            - {key: tier, operator: In, values: [frontend]}
    template:
        metadata:
            name: nginx-pod
            labels:
                app: nginx-app
                tier: frontend
        spec:
            containers:
            - name: nginx-container
              image: nginx
              ports: 
            - containerPort: 80
```

**Create, Display and Inspect ReplicaSet**
- `$ kubectl apply -f nginx-rs.yaml`
- `$ kubectl get pods`
- `$ kubectl describe rs nginx-rs`

**Scaling Up and Down**

Imperative Scaling:
- `$ kubectl scale replicasets nginx-rs --replicas=5`

Declarative Scaling:

To scale the nginx-rs ReplicaSet, edit the nginx-rs.yaml configuration file and set the replicas count to 5.

*Indeed, if the need is not acute, we generally recommend only making declarative changes*

**Delete**
- `$ kubectl delete rs nginx-rs`