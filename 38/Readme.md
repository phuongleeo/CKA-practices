Q:

Create a new deployment named red with the nginx image and 2 replicas, and ensure it gets placed on the master/controlplane node only.

A:

- Use the label - node-role.kubernetes.io/master - set on the master/controlplane node.

```shell
$kubectl --dry-run=client -o yaml create deploy red --image=nginx --replicas=2 > red.yaml
```

Add those lines underneat pod specs

```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
        - matchExpressions:
        - key: node-role.kubernetes.io/master
          operator: Exists
```
