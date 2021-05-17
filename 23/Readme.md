Q: Write a service that exposes nginx on a nodeport

1. Change it to use a cluster port
2. Scale the service
3. Change it to use an external IP
4. Change it to use a load balancer

## Imperative:

```shell
$kubectl create deploy nginx --image=nginx
$kubectl create service nodeport nginx --tcp=80:80
$kubectl patch svc nginx -p '{"spec":{"type": "NodePort"}}'
$kubectl patch svc nginx -p '{"spec":{"externalIPs": ["x.x.x.x"]}}'
$kubectl patch svc nginx -p '{"spec":{"type": "LoadBalancer"}}'
```

## Declarative:

```shell
$kubectl apply -f 23-services.yaml
```
