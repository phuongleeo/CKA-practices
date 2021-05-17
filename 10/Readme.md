Q: Create a daemon set
Change the update strategy to do a rolling update but delaying 30 seconds between pod updates
A:

## Declarative:

```shell
$kubectl apply -f 10-ds.yaml
```
