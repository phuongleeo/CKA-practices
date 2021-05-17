Q: Create a busybox container without a manifest. Then edit the manifest.
A:

```shell
$kubectl --dry-run=client -o yaml create deploy busybox --image=busybox -- sleep 1h > busybox-deploy.yaml
```
