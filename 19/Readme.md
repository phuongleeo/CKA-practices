Q: Create a service that references an externalname - https://api.github.com/users/phuongleeo
Test that this works from another pod

## Imperative:

```shell
$kubectl create service externalname api --external-name api.github.com
$kubectl run -it --rm --image=busybox busybox -- sh
#If you don't see a command prompt, try pressing enter.
$wget -S --header 'Host: api.github.com' https://api.default.svc.cluster.local/users/phuongleeo
```

## Declarative:

```shell
$kubectl apply -f 19-svc-externalName.yaml
```
