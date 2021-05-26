Q:

1. Backup an etcd cluster
2. List the members of an etcd cluster
3. Find the health of etcd

A:

- Test etcdclt

```shell
$sudo etcdctl --cert=/var/lib/minikube/certs/etcd/server.crt --key=/var/lib/minikube/certs/etcd/server.key --cacert=/var/lib/minikube/certs/etcd/ca.crt --endpoints=https://localhost:2379 get --from-key ''
```

1.

```shell
$ sudo etcdctl --cert=/var/lib/minikube/certs/etcd/server.crt --key=/var/lib/minikube/certs/etcd/server.key --cacert=/var/lib/minikube/certs/etcd/ca.crt snapshot save etcd-backup
{"level":"info","ts":1621927790.2596488,"caller":"snapshot/v3_snapshot.go:119","msg":"created temporary db file","path":"etcd-backup.part"}
{"level":"info","ts":"2021-05-25T07:29:50.267Z","caller":"clientv3/maintenance.go:200","msg":"opened snapshot stream; downloading"}
{"level":"info","ts":1621927790.2675745,"caller":"snapshot/v3_snapshot.go:127","msg":"fetching snapshot","endpoint":"127.0.0.1:2379"}
{"level":"info","ts":"2021-05-25T07:29:50.301Z","caller":"clientv3/maintenance.go:208","msg":"completed snapshot read; closing"}
{"level":"info","ts":1621927790.3013473,"caller":"snapshot/v3_snapshot.go:142","msg":"fetched snapshot","endpoint":"127.0.0.1:2379","size":"2.9 MB","took":0.041333745}
{"level":"info","ts":1621927790.3014183,"caller":"snapshot/v3_snapshot.go:152","msg":"saved","path":"etcd-backup"}
Snapshot saved at etcd-backup
```

```shell
$ sudo etcdctl --cert=/var/lib/minikube/certs/etcd/server.crt --key=/var/lib/minikube/certs/etcd/server.key --cacert=/var/lib/minikube/certs/etcd/ca.crt snapshot status etcd-backup -w table
+----------+----------+------------+------------+
|   HASH   | REVISION | TOTAL KEYS | TOTAL SIZE |
+----------+----------+------------+------------+
| 5bf91e9a |    13536 |        521 |     2.9 MB |
+----------+----------+------------+------------+
```

2.

```shell
$ sudo etcdctl --cert=/var/lib/minikube/certs/etcd/server.crt --key=/var/lib/minikube/certs/etcd/server.key --cacert=/var/lib/minikube/certs/etcd/ca.crt member list -w table
+------------------+---------+----------+----------------------------+----------------------------+------------+
|        ID        | STATUS  |   NAME   |         PEER ADDRS         |        CLIENT ADDRS        | IS LEARNER |
+------------------+---------+----------+----------------------------+----------------------------+------------+
| 238f4c8a4261811a | started | minikube | https://192.168.64.10:2380 | https://192.168.64.10:2379 |      false |
+------------------+---------+----------+----------------------------+----------------------------+------------+
```

3.

```shell
$ sudo etcdctl --cert=/var/lib/minikube/certs/etcd/server.crt --key=/var/lib/minikube/certs/etcd/server.key --cacert=/var/lib/minikube/certs/etcd/ca.crt endpoint --cluster health -w table
+----------------------------+--------+-----------+-------+
|          ENDPOINT          | HEALTH |   TOOK    | ERROR |
+----------------------------+--------+-----------+-------+
| https://192.168.64.10:2379 |   true | 7.04349ms |       |
+----------------------------+--------+-----------+-------+
```
