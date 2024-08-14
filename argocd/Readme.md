```sh

helm repo add argo https://argoproj.github.io/argo-helm
# install Argocd 2.10.9 https://artifacthub.io/packages/helm/argo/argo-cd/6.7.17
helm install argocd argo/argo-cd --version 6.7.17 -f values.yaml -n argocd
kubectl -n argocd patch -f patch.yaml
# istio ingress
openssl genrsa -out argocd-server.key 2048
openssl req -new -key argocd-server.key -out argocd-server.csr
openssl x509 -req -days 365 -in argocd-server.csr -signkey argocd-server.key -out argocd-server.crt
kubectl -n argocd create secret tls argocd-server-tls --cert=argocd-server.crt --key=argocd-server.key
kubectl -n argocd apply -f istio-gateway.yaml
```