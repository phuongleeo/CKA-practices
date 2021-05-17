Q:

1. Taint a node and run a Jenkins Pod on that specified node only.
2. Create a pod that has a liveness check
3. Use the utility nslookup to look up the DNS records of the service and pod.

A:

1. $kubectl taint node node-01 app=jenkins:NoSchedule
2. $kubectl apply -f jenkins.yaml
3. $kubectl run -it --rm --image=gcr.io/kubernetes-e2e-test-images/dnsutils:1.3  dnsutils -- nslookup jenkins.default.svc.cluster.local
$kubectl apply -f 7-dnsutils.yaml
