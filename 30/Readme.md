Q:

1.Create a new Ingress resource, name ping, namespace ing-internal 2. Use the /hello path to expose port 5678 of the service hello

A:

## Imperative

```shell
$kubectl create ingress ping \
  --rule="/hello=hello:5678" \
  --annotation nginx.ingress.kubernetes.io/rewrite-target=/
```

## Declarative

```shell
$kubectl apply -f ingress.yaml
```
