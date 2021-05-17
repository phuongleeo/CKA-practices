Q: Create 2 pod definitions:

1. the second pod should be scheduled to run anywhere the first pod is running - 2nd pod runs alongside the first pod.

A:

```shell
$kubectl apply -f 2-pod-affinity.yaml
```
