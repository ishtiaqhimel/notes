## Jobs

**Overview**
- 
- Jobs are 2 types
    - Run to completion (Jobs)
    - Scheduled (CronJob)

**Run to completion**
- Each job creates one or more pods
- Ensures they are successfully terminated
- Job controller restarts or rescheduled if a pod or node fails during execution
- can run multiple pods in parallel
- can scale up using kubectl scale command
- Use cases:
    - one time initialization of resources such as Database
    - Multiple workers to process messages in queue
    
    producer -----> work Queue -----> consumer

### Lets Start with Jobs
**Manifest File**
```yaml
# temp-job.yaml
apiVersion: batch/v1
kind: Job
metadata:
    name: helloworld
spec:
    template:
        spec:
            containers:
            - name: busybox
              image: busybox
              command: ["echo", "Hello Kubernetes!!!"]
            restartPolicy: Never
```

**Create and Display**
- `$ kubectl apply -f temp-job.yaml`

Now execute the below command-
- `$ kubectl logs <pod_name>`

We will see-
<br>`Hello Kubernetes!!!`

Now delete the job and change the yaml file like below-
```yaml
...
command: ["sleep", "30"]
...
```
Now create the job again and we will see the pod got completed after 30sec, run in another Terminal-
- `$ watch -n 2 kubectl get all -n default`


**Completions and Parallelism**
```yaml
...
spec:
    completions: 2
    parallelism: 1
    template:
...
```
create job with this yaml and it will run 2 jobs in serially. If we set `parallelism: 2`, then it will run parallelly.

**Backoff Limit**

If a pod fails, then new pods is created until it is created successfully. But it can take infinit times for some error. Lets change the yaml file like below-

```yaml
...
spec:
    backoffLimit: 3
    completions: 2
    parallelism: 1
    template:
    ...
            command: ["ls", "/himel"]
    ...
...
```
Now run the below command to see the events-
- `$ kubectl describe jobs | less`

**Active Deadline**
If we know that our job will not take more than a specific time, then we can set `activeDeadlineSeconds` to terminate our job after a specific time.

```yaml
...
spec:
    activeDeadlineSeconds: 10
    template:
    ...
            command: ["sleep", "30"]
    ...
...
```

Now run the below command to see the events-
- `$ kubectl describe jobs | less`

### Cronjob
- Sometimes we want to schedule a job to be run at a certain interval.
- To achieve this, we can declare a CronJob in Kubernetes, which is responsible for creating a new Job object at a particular interval
```yaml
# cron-job.yaml
apiVersion: batch/v1
kind: CronJob
metadata:
    name: helloworld
spec:
    schedule: "* * * * *"
    jobTemplate:
        spec:
            template:
                spec:
                    containers:
                    - name: busybox
                      image: busybox
                      command: ["echo", "Hello Kubernetes!!!"]
                    restartPolicy: Never
```
To delete-
- `$ kubectl delete cronjob  helloworld`