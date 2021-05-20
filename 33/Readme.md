Q:

1. Create a PV named app-config, the capacity of the PV is 2Gi, the access mode is ReadWriteMany, and the type of volume is hostPath
2. The hostPath mapped by pv is the /srv/app-config directory

A:

```shell
$kubectl apply -f pv.yaml
```
