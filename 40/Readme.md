Q:

Backup/restore cluster etcd
Noted: Use Vagrantfile in question [39](../39/Vagrantfile)
A:

- Check cluster status

```shell
$etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key --endpoints=master-1:2379,master-2:2379,master-3:2379 endpoint status -
w table

+---------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
|   ENDPOINT    |        ID        | VERSION | DB SIZE | IS LEADER | IS LEARNER | RAFT TERM | RAFT INDEX | RAFT APPLIED INDEX | ERRORS |
+---------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
| master-1:2379 | 24c5c54781d6bfad |  3.4.13 |  5.8 MB |      true |      false |         2 |        364 |                364 |        |
| master-2:2379 | 29aa8bf96bcd49f4 |  3.4.13 |  5.8 MB |     false |      false |         2 |        364 |                364 |        |
| master-3:2379 | cd5a5ee8db7783f7 |  3.4.13 |  5.8 MB |     false |      false |         2 |        364 |                364 |        |
+---------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
```

- Stop API server: either move static pod api-server to another location or stop systemd service

- Save snapshot: perform on 1 master node

```shell
$etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key snapshot save snapshot.db
```

- Restore snapshot:

  - Perform on all nodes:

    - master-1:

    ```shell
    $ ETCDCTL_API=3 etcdctl snapshot restore snapshot.db \
      --data-dir=/var/lib/etcd-new \
      --name=master-1 \
      --initial-cluster=master-1=https://master-1:2380,master-2=https://master-2:2380,master-3=https://master-3:2380 \
      --initial-cluster-token=etcd-cluster-1 \
      --initial-advertise-peer-urls=https://master-1:2380
    ```

    - master-2:

    ```shell
    $ ETCDCTL_API=3 etcdctl snapshot restore snapshot.db \
      --data-dir=/var/lib/etcd-new \
      --name=master-2 \
      --initial-cluster=master-1=https://master-1:2380,master-2=https://master-2:2380,master-3=https://master-3:2380 \
      --initial-cluster-token=etcd-cluster-1 \
      --initial-advertise-peer-urls=https://master-2:2380
    ```

    - master-3

    ```shell
    $ ETCDCTL_API=3 etcdctl snapshot restore snapshot.db \
      --data-dir=/var/lib/etcd-new \
      --name=master-3 \
      --initial-cluster=master-1=https://master-1:2380,master-2=https://master-2:2380,master-3=https://master-3:2380 \
      --initial-cluster-token=etcd-cluster-1 \
      --initial-advertise-peer-urls=https://master-3:2380
    ```

  - Edit static pod's manifest

  ```yaml
  #...
  - --initial-cluster=master-1=https://master-1:2380,master-2=https://master-2:2380,master-3=https://master-3:2380
  - --initial-cluster-token=etcd-cluster-1
  - --initial-cluster-state=existing
  #...
  volumes:
    - hostPath:
        path: /var/lib/etcd-new
        type: DirectoryOrCreate
      name: etcd-data
  ```

## Force new cluster

- Master-1

  - Edit static pod etcd

  ```yaml
  #...
  - --initial-cluster=master-1=https://192.168.50.11:2380
  - --force-new-cluster=true
  - --initial-cluster-state=new
  #...
  ```

  - Waiting for etcd service up and running on master 1
  - Adding individual master node 2

  ```shell
  $etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key member add master-2 --peer-urls=https://192.168.50.12:2380
  Member e4f4512d5bb2e8e5 added to cluster 9b25ac60976b4671

  ETCD_NAME="master-2"
  ETCD_INITIAL_CLUSTER="master-1=https://master-1:2380,master-2=https://192.168.50.12:2380"
  ETCD_INITIAL_ADVERTISE_PEER_URLS="https://192.168.50.12:2380"
  ETCD_INITIAL_CLUSTER_STATE="existing"
  ```

  - Adding master node 3

  ```shell
  $etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.root@master-1:/etc/kubernetes# etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key member add master-3 --peer-urls=https://192.168.50.13:2380
  Member  c71992ce41e891f added to cluster 9b25ac60976b4671

  ETCD_NAME="master-3"
  ETCD_INITIAL_CLUSTER="master-3=https://192.168.50.13:2380,master-1=https://master-1:2380,master-2=https://192.168.50.12:2380"
  ETCD_INITIAL_ADVERTISE_PEER_URLS="https://192.168.50.13:2380"
  ETCD_INITIAL_CLUSTER_STATE="existing"
  ```

- Master 2

  - Edit etcd static pod

  ```yaml
  #...
  #Add exactly values which was provided as above, otherwise etcd will throw error: unmatched member
  - --initial-cluster=master-1=https://master-1:2380,master-2=https://192.168.50.12:2380
  - --initial-cluster-state=existing
  ```

- Verify status

```shell
root@master-1:/etc/kubernetes# etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key member list -w table
+------------------+---------+----------+----------------------------+----------------------------+------------+
|        ID        | STATUS  |   NAME   |         PEER ADDRS         |        CLIENT ADDRS        | IS LEARNER |
+------------------+---------+----------+----------------------------+----------------------------+------------+
|  c71992ce41e891f | started | master-3 | https://192.168.50.13:2380 | https://192.168.50.13:2379 |      false |
| 24c5c54781d6bfad | started | master-1 |      https://master-1:2380 | https://192.168.50.11:2379 |      false |
| e4f4512d5bb2e8e5 | started | master-2 | https://192.168.50.12:2380 | https://192.168.50.12:2379 |      false |
+------------------+---------+----------+----------------------------+----------------------------+------------+
```
