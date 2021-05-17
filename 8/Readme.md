Q: Find which Pod is taking max CPU
A:

```shell
$kubectl top pod --sort-by=cpu -A
```
