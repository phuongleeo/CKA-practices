Q:

1. The status of node wk8s-node-0 is NotReady, check the reason and restore its status to Ready
2. Ensure that the operation is durable

A:

- Check the abnormal node through get nodes, log in to the node to check the status of kubelet and other components and determine the cause
- Start kubelet and enable kubelet

```shell
$systemctl status kubelet
$systemctl enable kubelet
$systemctl restart kubelet
$systemctl status kubelet
```
