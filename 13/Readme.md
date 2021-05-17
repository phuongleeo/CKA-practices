Q: Create a pod that uses secrets

1. Create a secret
2. Pull secrets from environment variables
3. Pull secrets from a volume
4. Dump the secrets out via kubectl to show it worked

A: Imperative mode

$kubectl --dry-run=client -o yaml create secret generic 13-secret --from-literal=PROVISIONER=minukube >> 12-busybox.yaml
$kubectl get secret 13-secret -o jsonpath={.data.PROVISIONER}|base64 -D
$kubectl apply -f 12-busybox.yaml
$kubectl exec -it pod/$(kubectl get pod -l app=busybox -o name) -- env
