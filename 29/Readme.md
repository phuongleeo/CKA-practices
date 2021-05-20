Q:

1. Reconfigure the existing deployment front-end, add a port named http, and expose 80/TCP
2. Create a service named front-end-svc to expose the http port of the container
3. Configure the service category as NodePort

A:

- Edit deploy as needed, add port information

```shell
$kubectl edit deploy/front-end
```

append these lines at the container spec

```yaml
ports:
  - name: http
    protocol: TCP
    containerPort: 80
```

- Expose the port by using NodePort through the expose command

```shell
$kubectl expose deployment front-end --type=NodePort --port=80 --target-port=80 --name=front-end-svc
```
