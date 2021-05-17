Q: List all PersistentVolumes sorted by their name
A:

```shell
$kubectl get pv --sort-by='{.metadata.name}' -A
```
