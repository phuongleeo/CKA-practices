Q: Write an ingress rule that redirects calls to /foo to one service and to /bar to another
A:

## Imperative:

```shell
$kubectl create deploy podinfo --image=stefanprodan/podinfo
$kubectl create service clusterip podinfo --tcp=9898
$kubectl create deploy echo-server --image=jmalloc/echo-server
$kubectl create service clusterip echo-server --tcp=8080
$kubectl create ingress cka --class=nginx \
  --rule="foo.com/foo=podinfo:9898" \
  --rule="bar.com/bar=echo-server:8080" \
  --annotation kubernetes.io/ingress.class=nginx
```

## Declarative:

```shell
$kubectl apply -f 22-ingress.yaml
```
