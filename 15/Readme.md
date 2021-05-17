Q: Create a job that runs 20 times, 5 containers at a time, and prints “Hello parallel world”
A:

## Imperative:

```shell
$kubectl create cronjob my-job --image=busybox -- echo "Hello parallel world"
```

## Declarative:

```shell
$kubectl apply -f 15-job.yaml
```
