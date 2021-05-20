Q:

1. Use the specified storageclass csi-hostpath-sc to create a PVC named pv-volume with a capacity of 10Mi
2. Create a pod named web-server, and mount the /usr/share/nginx/html directory of the nginx container with this pvc
3. Update the size of the above pvc from 10Mi to 70Mi, and record this change

A:

- Copy a PVC according to the official document, modify the parameters, and use the explain of kubectl if you are unsure
- Generate a nginx pod through dry-run + -o yaml, and then add volumeMounts and volume (if you are not familiar with it, just copy it from the daemonset case and modify it)
- Modify pvc through edit, don't forget the --record parameter

```shell
$kubectl apply -f pv.yaml
#modify pv-volume, adjust size of volume
$kubectl edit pv/pv-volume --record
#or
$kubectl patch pv/pv-volume -p '{"spec":{"resources": { "requests": {"storage": "70Mi" } } } }'
```
