Q: Rotate certificates
A:

- by default, certificates will be auto rotate whenever cert is being expired low to 30% to 10% threshold

```shell
$vagrant ssh k8s-master
$sudo kubeadm certs check-expiration
CERTIFICATE                EXPIRES                  RESIDUAL TIME   CERTIFICATE AUTHORITY   EXTERNALLY MANAGED
admin.conf                 May 19, 2022 04:18 UTC   362d                                    no
apiserver                  May 19, 2022 04:18 UTC   362d            ca                      no
apiserver-etcd-client      May 19, 2022 04:18 UTC   362d            etcd-ca                 no
apiserver-kubelet-client   May 19, 2022 04:18 UTC   362d            ca                      no
controller-manager.conf    May 19, 2022 04:18 UTC   362d                                    no
etcd-healthcheck-client    May 19, 2022 04:18 UTC   362d            etcd-ca                 no
etcd-peer                  May 19, 2022 04:18 UTC   362d            etcd-ca                 no
etcd-server                May 19, 2022 04:18 UTC   362d            etcd-ca                 no
front-proxy-client         May 19, 2022 04:18 UTC   362d            front-proxy-ca          no
scheduler.conf             May 19, 2022 04:18 UTC   362d                                    no

CERTIFICATE AUTHORITY   EXPIRES                  RESIDUAL TIME   EXTERNALLY MANAGED
ca                      May 17, 2031 04:18 UTC   9y              no
etcd-ca                 May 17, 2031 04:18 UTC   9y              no
front-proxy-ca          May 17, 2031 04:18 UTC   9y              no

$sudo kubeadm certs renew all
W0521 10:55:27.161541   15154 kubelet.go:215] detected "systemd" as the Docker cgroup driver, the provided value "cgroupfs" in "KubeletConfiguration" will be overrided

certificate embedded in the kubeconfig file for the admin to use and for kubeadm itself renewed
certificate for serving the Kubernetes API renewed
certificate the apiserver uses to access etcd renewed
certificate for the API server to connect to kubelet renewed
certificate embedded in the kubeconfig file for the controller manager to use renewed
certificate for liveness probes to healthcheck etcd renewed
certificate for etcd nodes to communicate with each other renewed
certificate for serving etcd renewed
certificate for the front proxy client renewed
certificate embedded in the kubeconfig file for the scheduler manager to use renewed
```

- The hard way:
  - Manual
    [mmumshad](https://github.com/mmumshad/kubernetes-the-hard-way/blob/master/docs/04-certificate-authority.md)

```shell

```
