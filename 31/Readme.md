Q:

1.Create pod name nginx-kusc0041, mirror nginx
2.Schedule the pod to the node with disk=ssd

A:

## Imperative

Modify the sample pod template

```shell
$kubectl run --port=80 --image=nginx --expose --overrides='{ "apiVersion": "v1", "spec": { "nodeSelector": { "disk": "ssd" } } }' nginx-kusc0041
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-kusc0041
spec:
  containers:
    - name: nginx
      image: nginx
  nodeSelector:
    disk: ssd
```
