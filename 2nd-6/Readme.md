Q: Configure the cluster to use 8.8.8.8 and 8.8.4.4 as upstream DNS servers.
A:
[CoreDNS](https://coredns.io/manual/toc/#forwarding)

```shell
$kubectl edit cm/coredns -n kube-system
...
  Corefile: |
    .:53 {
        errors
        health
        kubernetes cluster.local in-addr.arpa ip6.arpa {
          pods insecure
          fallthrough in-addr.arpa ip6.arpa
        }
        prometheus :9153
        forward . 8.8.8.8 8.8.4.4
        forward . /etc/resolv.conf
        cache 30
        loop
        reload
        loadbalance
    }
...
```
