Q:

1. Back up the etcd data on https://127.0.0.1:2379 to /var/lib/backup/etcd-snapshot.db
2. Use the previous file /data/backup/etcd-snapshot-previous.db to restore etcd
3. Use the specified ca.crt, etcd-client.crt, etcd-client.key

## Imperative:

```shell
#backup
$ETCDCTL_API=3 etcdctl --endpoints https://172.0.0.1:2379 --cacert=/opt/xxx/ca.crt --cert=/opt/xxx/etcd-client.crt --key=/opt/xxx/etcd-client.key snapshot save /var/lib/backup/etcd-snapshot.db
#restore
$ETCDCTL_API=3 etcdctl --endpoints https://172.0.0.1:2379 --cacert=/opt/xxx/ca.crt --cert=/opt/xxx/etcd-client.crt --key=/opt/xxx/etcd-client.key snapshot restore /var/lib/backup/etcd-snapshot-previous.db
```

After the restoration is successful, it is best to confirm that the cluster status is normal through get nodes
Note 2: there might a problem in restoring,

1. When restoring, a default.etcd folder will be generated in the execution directory, which contains etcd data files.
2. Then switch to root, systemctl cat etcd and look at the parameters at startup, where is the data directory of etcd.
3. Then stop etcd, back up the previous data and then mv default.etcd, to the directory specified by the configuration file, and both the subordinate group and the master are etcd. Then just start etcd

## Declarative:
