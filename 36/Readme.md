Q:

1. Add a sidecar container (using busybox image) to the existing pod 11-factor-app
2. Ensure that the sidecar container can output /var/log/11-factor-app.log information
3. Use volume to mount the /var/log directory to ensure that the sidecar can access the 11-factor-app.log file

A:

```shell
$kubectl apply -f sidecar.yaml
```
