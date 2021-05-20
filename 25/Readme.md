Q:

1. Set ek8s-node-1 node as unavailable
2. Reschedule all pods on the node

<img src=https://kubernetes.io/images/docs/kubectl_drain.svg>

## Imperative:

```shell
$kubectl cordon ek8s-node-1
$kubectl drain node ek8s-node-1 --ignore-daemonsets=true --delete-emptydir-data=true
```

## Declarative:
