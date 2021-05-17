Q: Create a deployment running nginx version 1.12.2 that will run in 2 pods

1. Scale this to 4 pods.
2. Scale it back to 2 pods.
3. Upgrade this to 1.13.8
4. Check the status of the upgrade
5. How do you do this in a way that you can see the history of what happened?
6. Undo the upgrade
7. Expose the service on port 80

A:

```shell
$kubectl create deploy nginx --image=nginx:1.12.2 --replicas=2
$kubectl scale deploy nginx --replicas=4
$kubectl set image deployment/nginx nginx=1.13.8 (--record=true)
$kubectl rollout status deploy/nginx
$kubectl rollout history deploy/nginx (--revision=xxx)
$kubectl rollout undo deploy/nginx (--to-revision=xxx)
$kubectl expose deploy/nginx --port=80 --target-port=80 --name=nginx (--type=ClusterIP)
```
