Q:

1. Create a network policy allow-port-from-namespace in the fubar namespace
2. Only allow pod in ns my-app to connect to port 80 of pod in fubar

- Note: There are 2 ns , one is fubar (the ns of the target pod), and the other is my-app (the ns of the source pod)
  - The real question before February, the network strategy and the source pod are the same ns, this time the adjustment source is my-app, not all ns pods can access port 80 of the pod under fubar;
    Therefore, you need to check the labels of my-app, and then add the corresponding labels in namespaceSelector; many friends who have taken the test report that there is no label during the actual test, so it is recommended to add a label during the actual test;
    Since the previous questions were all the same ns, the author also ignored the 2 ns when taking the exam. This should be a pit that the author stepped on.

## Imperative:

```shell
kubectl create ns ns-target
kubectl create ns ns-app1
kubectl create ns ns-app2

kubectl -n ns-target create deployment nginx-target --image=nginx:1.19.4
kubectl -n ns-app1 create deployment nginx-app1 --image=nginx:1.19.4
kubectl -n ns-app2 create deployment nginx-app2 --image=nginx:1.19.4
target: echo "hi, nginx-target!" > /usr/share/nginx/html/index.html
app1: echo "hi, nginx-app1!" > /usr/share/nginx/html/index.html
app2: echo "hi, nginx-app2!" > /usr/share/nginx/html/index.html

kubectl -n ns-target create deployment ubuntu-target --image=ubuntu:20.04 -- sleep infinity
kubectl -n ns-app1 create deployment ubuntu-app1 --image=ubuntu:20.04 -- sleep infinity
kubectl -n ns-app2 create deployment ubuntu-app2 --image=ubuntu:20.04 -- sleep infinity
#Start port 81 in the 3 ubuntu respectively, python3 -m http.server 81 &

kubectl -n ns-target expose deployment nginx-target --name=svc-nginx-target --port=80
kubectl -n ns-app1 expose deployment nginx-app1 --name=svc-nginx-app1 --port=80
kubectl -n ns-app2 expose deployment nginx-app2 --name=svc-nginx-app2 --port=80
```

1. Test 1, limit port 80, ingress from does not impose any restrictions:

```shell
$kubectl apply -f test-1.yaml
```

2. Test 2, limit 80 port, ingress from limit namespaceSelector:

```shell
$kubectl label ns ns-app1 "ns=ns-app1"
$kubectl apply -f test-2.yaml
```

- combining the above tests can find that if you need to limit one If ns A accesses another ns B, it is better to set the label of ns A, and then restrict it through namespaceSelector.

3. Test 3, limit port 80, the range of namespaceSelector and podSelector in from is different

- this is considered `OR` operator: pod which come from namespace which have label `ns=ns-app` or all pods regardless label are allowed to connect port `80`

```yaml
- from:
    - namespaceSelector:
        matchLabels:
          ns: ns-app1
    - podSelector:
        matchLabels: {}
```

- This is considered `AND` operator: pod which only come from namespace which have label `ns=ns-app` is allowed

```yaml
- from:
    - namespaceSelector:
        matchLabels:
          ns: ns-app1
      podSelector:
        matchLabels: {}
```
