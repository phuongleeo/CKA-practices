Q: Join a new node to cluster
A:

- Master node

```shell
$kubeadm token create --print-join-command > node-join.sh
```

- Worker node: Require `kubelet` and `kubeadm` installed

```shell
$bash node-join.sh
```
