Q: Create a horizontal autoscaling group that starts with 2 pods and scales when CPU usage is over 50%.
A:

## Imperative:

```shell
$kubectl create deploy podinfo --image=stefanprodan/podinfo
$kubectl autoscale deploy podinfo --min=2 --max=3 --cpu-percent=50
$kubectl get hpa
```

## Declarative:

```shell
$kubectl apply -f 16-hpa.yaml
```
