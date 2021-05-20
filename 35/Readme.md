Q:

1. Monitor logs in foobar pod
2. Obtain the log containing the disabled-to-access-website and write the log to /opt/KUTR00101/foobar

A:

```shell
$kubectl logs pod/foobar |grep -i disabled-to-access-website > /opt/KUTR00101/foobar
```
