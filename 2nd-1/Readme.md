Q: Make an API call using CURL and proper certs
A:
[Cluster administrator](https://kubernetes.io/docs/tasks/administer-cluster/access-cluster-api/)

```shell
$kubectl config view -o jsonpath='{"Cluster name\tServer\n"}{range .clusters[*]}{.name}{"\t"}{.cluster.server}{"\n"}{end}'
$kubectl proxy
$curl http://localhost:8001/api/v1/namespaces/
```
