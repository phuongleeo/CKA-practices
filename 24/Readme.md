Q:

1. Create a ClusterRole named deployment-clusterrole
2. This role has the authority to create Deployment, Statefulset, and Daemonset
3. Create a ServiceAccount named cicd-token in the namespace app-team1
4. Bind ClusterRole to ServiceAccount, and limit the namespace to app-team1

## Imperative:

```shell
$kubectl create ns app-team1
$kubectl create serviceaccount cicd-token -n app-team1
$kubectl create clusterrole deployment-clusterrole --verb=create --resource=deployment,statefulset,daemonset
$kubectl -n app-team1 create rolebinding cicd-clusterrole --clusterrole=deployment-clusterrole --serviceaccount=app-team1:cicd-token
```

## Declarative:
