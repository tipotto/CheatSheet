# [Rustscan](https://rustscan.github.io/RustScan/)

## scan type
### full-port scan
```
rustscan -a [IP] -b [BATCH] -r 0-65535 -t [TIMEOUT] -- -A -oN [OUTPUT FILE]
```
```
ports=$(rustscan -a $rhost -b 1000 -r 0-65535 -t 5000 -- --min-rate 10000 -oN rustscan/all.out | grep ^[0-9] | cu
t -d '/' -f1 | tr '\n' ',' | sed s/,$//)
```

### scan specific ports executing NSE scripts
```
rustscan -a [IP] -b [BATCH] -p [PORT] -t [TIMEOUT] -- -A -oN [OUTPUT FILE] --script vuln
```
```
rustscan -a $rhost -b 1000 -p $ports -t 5000 -- --min-rate 10000 -sV --script=vuln -oN rustscan/ports.out
```

### options
- -a : IP address
- -b : Batch size. Depends on the open file limit of your OS.  If you do 65535 it will do every port at the same time. 
- -r : A range of ports
- -p : Single port or multiple ports (Comma seperated)
- -t : The timeout in milliseconds before a port is assumed to be closed (default: 1500)

## memo
- -- の後にNmapのオプションを指定することで、Nmapの機能を実行することができる。
- oオプションを付与してOSスキャンを実行する場合は、root権限が必要。

