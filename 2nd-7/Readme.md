Q: Create a pod with nginx and place a file using an init container that creates a simple index.html file with content - “created by init container”
A:

```shell
$kubectl apply -f init-container.yaml
$kubectl expose pod init-demo  --port 80
$kubectl port-forward svc/init-demo 8000:80
$curl localhost:8000
created by init container
```
