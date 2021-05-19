These tasks are required a minikube cluster and a real running cluster ( EKS for example)

# Minikube

- Sample Minikube config

```shell
❯ minikube config view
- kubernetes-version: v1.20.2
- memory: 4g
- network-plugin: cni
- nodes: 2
- cni: calico
- cpus: 4
- driver: hyperkit
- install-addons: [registry-creds ingress]
```

- Minikube config file: `~/.minikube/config/config.json`

```json
{
  "cni": "calico",
  "cpus": "4",
  "driver": "hyperkit",
  "install-addons": ["registry-creds", "ingress"],
  "kubernetes-version": "v1.20.2",
  "memory": "4g",
  "network-plugin": "cni",
  "nodes": "2"
}
```

# EKS

[Terraform](https://github.com/phuongleeo/k8s-todo/tree/master/tf-INF/development/eks)

# Vagrant

- Noted:
  - Edit the Vagrantfile in order to change the default kubernetest version ( default: `1.21.1` )
  - Once provisioner finished setup cluster, the kube config file will be copied to `kubernetes-setup/config` location

```shell
$cd Vagrant
$vagrant up
$ ## Accessing master
$ vagrant ssh k8s-master
vagrant@k8s-master:~$ kubectl get nodes
NAME         STATUS   ROLES    AGE     VERSION
k8s-master   Ready    master   18m     v1.21.1
node-1       Ready    <none>   12m     v1.21.1
node-2       Ready    <none>   6m22s   v1.21.1

$ ## Accessing nodes
$ vagrant ssh node-1
$ vagrant ssh node-2

$#From host
$export KUBECONFIG="./kubernetes-setup/config"
$kubectl get nodes -o wide
NAME         STATUS   ROLES                  AGE     VERSION   INTERNAL-IP     EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
k8s-master   Ready    control-plane,master   4h5m    v1.21.1   192.168.50.10   <none>        Ubuntu 16.04.7 LTS   4.4.0-210-generic   docker://20.10.6
node-1       Ready    <none>                 3h55m   v1.21.1   192.168.50.20   <none>        Ubuntu 16.04.7 LTS   4.4.0-210-generic   docker://20.10.6
node-2       Ready    <none>                 3h45m   v1.21.1   192.168.50.30   <none>        Ubuntu 16.04.7 LTS   4.4.0-210-generic   docker://20.10.6
```
