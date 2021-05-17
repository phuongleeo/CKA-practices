Q: Create a busybox container without a manifest. Then edit the manifest.
A:

```shell
$kubectl create deploy busybox --image=busybox -- sleep 1h
$kubectl edit deploy busybox
```
