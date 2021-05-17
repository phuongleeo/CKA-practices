Q: Create a job that runs every 3 minutes and prints out the current time.
A:

## Imperative:

```shell
$kubectl create cronjob my-job --image=busybox --schedule="*/3 * * * *" -- date
```

## Declarative:

```shell
$kubectl apply -f 14-cronjob.yaml
```
