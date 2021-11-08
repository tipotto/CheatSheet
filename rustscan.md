# Rustscan

```
rustscan -a $rhost -b 1000 -r 0-65535 -t 5000 -- -A -oN [OUTPUT FILE]
```

- -a : IP address
- -b : Batch size. Depends on the open file limit of your OS.  If you do 65535 it will do every port at the same time. 
- -r : A range of ports
- -t : The timeout in milliseconds before a port is assumed to be closed (default: 1500)

## Memo
-- の後にNmapのオプションを指定することで、Nmapの機能を実行することができる。
