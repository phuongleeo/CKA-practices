Q: Create a custom resource definition - CRD
Display it in the API with curl
A:

## Declarative:

```shell
$kubectl apply -f 17-crd.yaml
$kubectl get crontab
$kubectl proxy
$curl -s "http://localhost:8001/apis/stable.cka/v1/namespaces/\*/crontabs/"|jq
```
