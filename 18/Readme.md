Q: Create a networking policy such that only pods with the label access=granted can talk to it.

1. Create a nginx pod and attach this policy to it.
2. Create a busybox pod and attempt to talk to nginx - should be blocked
3. Attach the label to busybox and try again - should be allowed

A:

## Imperative:

## Declarative:

Advance lab: busybox pod was allowed to talk to podinfo and echo-server

```shell
$kubectl apply -f 18-policy.yaml
```
