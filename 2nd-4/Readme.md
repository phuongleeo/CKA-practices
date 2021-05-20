Q: Taint a node and un-taint it
A:

### taint

```shell
$kubectl taint node node-1 app=jenkins:NoSchedule
```

### untaint

```shell
$kubectl taint node node-1 app=jenkins:NoSchedule-
```
