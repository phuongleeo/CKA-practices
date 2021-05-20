Q:

1. Upgrade the master node to 1.20.1
2. Ensure the drain master node before upgrading
3. Do not upgrade worker node, container manager, etcd, CNI plug-in, DNS, etc.

## Imperative:

```shell
$kubectl get nodes
$ssh mk8s-master-0
$kubectl cordon mk8s-master-0
$kubectl drain mk8s-master-0 --ignore-daemonsets
$apt-mark unhold kubeadm kubectl kubelet
$apt-get update && apt-get install -y kubeadm=1.20.1-00 kubelet=1.20.1-00 kubectl=1.20.1-00
$apt-mark hold kubeadm kubectl kubelet
$kubeadm upgrade plan
$kubeadm upgrade apply v1.20.1 --etcd-upgrade=false
#kubectl rollout undo deployment coredns -n kube-system
$kubectl uncordon mk8s-master-0
```

## Declarative:
