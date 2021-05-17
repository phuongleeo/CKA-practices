Q: Create a static pod
A: The full link to document [here] (https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/)

use minikube to complete this task
$minikube ssh -n minikube-m02 --native-ssh=true

`Run this command on the node where kubelet is running`

```shell
mkdir /etc/kubelet.d/
cat <<EOF >/etc/kubelet.d/static-web.yaml
apiVersion: v1
kind: Pod
metadata:
  name: static-web
  labels:
    role: myrole
spec:
  containers:
    - name: web
      image: nginx
      ports:
        - name: web
          containerPort: 80
          protocol: TCP
EOF
```

Check whether it worked or not:

```shell
$kubectl get pods
```
