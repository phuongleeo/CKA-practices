Q:

1. Create a pod named kucc1
2. Two examples of running nginx and redis in pod

A:

- Dry-run a pod, just add one more mirror image, it is a free sub-question

```shell
$kubectl --dry-run=client -o yaml run --image=nginx kucc1
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: kucc1
  name: kucc1
spec:
  containers:
    - image: nginx:alpine
      name: nginx
    - image: redis:alpine
      name: redis
      resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
