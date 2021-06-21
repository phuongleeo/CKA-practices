Q:

Create a new stacked HA cluster

I used Nginx TCP load balancer instead of HA proxy

<img src="https://www.ibm.com/docs/en/SSMNED_v10/com.ibm.apic.install.doc/loadbal_k8s.jpeg">
A:

- Vagrant

```ruby
$vagrant up
```

- Loadbalancer Nginx

```shell
$vagrant ssh lb-master
$sudo apt install nginx
$cat /vagrant/nginx_tcp_proxy.conf | sudo tee -a /etc/nginx/nginx.conf
$sudo nginx -s reload
```

- On Master-1 node

  - Noted: As we're using Vagrant with VirtualBox to provision cluster, thus VMs contain of 2 VNIC ( nat + private network). Therefor prior to perform `kubeadm init` command, we must sepecify ip address of VM at `apiserver-advertise-address` parameter ( resolved link [here](https://github.com/kubernetes/kubeadm/issues/1359#issuecomment-619564221)), otherwise your additional master node could not joined cluster.

  On Additional master: must also sepecify `apiserver-advertise-address` parameter when joining cluster ( check example below)

```shell
$kubeadm init --apiserver-advertise-address="192.168.50.11" --apiserver-cert-extra-sans="192.168.50.11" --apiserver-cert-extra-sans="192.168.50.12" --apiserver-cert-extra-sans="192.168.50.13" --node-name master-1 --pod-network-cidr=192.168.0.0/16 --control-plane-endpoint="192.168.50.10:6443" --upload-certs

You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join 192.168.50.10:6443 --token 9regks.vmkh166rj3qf7xur \
    --discovery-token-ca-cert-hash sha256:f64f2a8fc453c2c5889a145a4cc0044a6fa4ec963d2c8fb2fc9131d68bd683e3 \
    --control-plane --certificate-key 6b80e2ccdd1317fc737a0a33857f4828bf49ee2e99c1821d4fa2af63fc4ecf64

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use
"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.50.10:6443 --token 9regks.vmkh166rj3qf7xur \
    --discovery-token-ca-cert-hash sha256:f64f2a8fc453c2c5889a145a4cc0044a6fa4ec963d2c8fb2fc9131d68bd683e3
```

- Generate a new certificate-key, in case of missing the previous one

```shell
$kubeadm init phase upload-certs --upload-certs
$kubeadm token create --print-join-command --certificate-key 4958cec9a2b920eb5b4a2f4632d8b1c81efa86dbd1a3b6acf6525d26ebc304d6
```

- Install calico network plugin

```shell
$kubectl create -f https://docs.projectcalico.org/manifests/tigera-operator.yaml
$kubectl create -f https://docs.projectcalico.org/manifests/custom-resources.yaml
```

- Print joint master command

```shell
$kubeadm token create --print-join-command --certificate-key 4958cec9a2b920eb5b4a2f4632d8b1c81efa86dbd1a3b6acf6525d26ebc304d6

kubeadm join 192.168.50.10:6443 --token 0k8ye6.nsvrinqsnug5n1t9     --discovery-token-ca-cert-hash sha256:a6ebc718f7f05d82188c17348a7960f4597213e118d933ed086ededb27fb4411     --control-plane --certificate-key 4958cec9a2b920eb5b4a2f4632d8b1c81efa86dbd1a3b6acf6525d26ebc304d6

#For the master nodes join cluster
kubeadm join 192.168.50.10:6443 --token 8z2xgl.gx88wyuqy0mm726e \
    --discovery-token-ca-cert-hash sha256:a6ebc718f7f05d82188c17348a7960f4597213e118d933ed086ededb27fb4411 \
    --control-plane --certificate-key 4958cec9a2b920eb5b4a2f4632d8b1c81efa86dbd1a3b6acf6525d26ebc304d6 \
    --apiserver-advertise-address={{master ip}}
```

- Output

```shell
root@master-1:~# kubectl get node
NAME       STATUS   ROLES                  AGE   VERSION
master-1   Ready    control-plane,master   31m   v1.20.4
master-2   Ready    control-plane,master   29m   v1.20.4
master-3   Ready    control-plane,master   5m    v1.20.4
```

    - Check etcd member

```shell
root@master-1:~# docker exec -it k8s_etcd_etcd-master-1_kube-system_89b59474702d42c4ba8f023f666e7f3c_0 sh -c "etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key member list -w table"
+------------------+---------+----------+----------------------------+----------------------------+------------+
|        ID        | STATUS  |   NAME   |         PEER ADDRS         |        CLIENT ADDRS        | IS LEARNER |
+------------------+---------+----------+----------------------------+----------------------------+------------+
| d2dc09dd794becad | started | master-3 | https://192.168.50.13:2380 | https://192.168.50.13:2379 |      false |
| d8aaf9913a9b0ac6 | started | master-2 | https://192.168.50.12:2380 | https://192.168.50.12:2379 |      false |
| e4e4533a349b670b | started | master-1 | https://192.168.50.11:2380 | https://192.168.50.11:2379 |      false |
+------------------+---------+----------+----------------------------+----------------------------+------------+
```
