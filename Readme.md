These tasks are required a minikube cluster and a real running cluster ( EKS for example)

# Minikube

## Sample Minikube config

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

## Minikube config file: `~/.minikube/config/config.json`

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
```
