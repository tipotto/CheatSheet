# Rustscan

## スキャン方法
### フルポートスキャン
```
rustscan -a $rhost -b 1000 -r 0-65535 -t 5000 -- -A -oN [OUTPUT FILE]
```
```
ports=$(rustscan -a $rhost -b 1000 -r 0-65535 -t 5000 -- --min-rate 10000 -oN rustscan/all.out | grep ^[0-9] | cu
t -d '/' -f1 | tr '\n' ',' | sed s/,$//)
```

### 特定ポートのスキャン（NSE スクリプトの実行）
```
rustscan -a $rhost -b 1000 -p [PORT] -t 5000 -- -A -oN [OUTPUT FILE] --script vuln
```
```
rustscan -a $rhost -b 1000 -p $ports -t 5000 -- --min-rate 10000 -sV --script=vuln -oN rustscan/ports.out
```

### オプション
- -a : IP address
- -b : Batch size. Depends on the open file limit of your OS.  If you do 65535 it will do every port at the same time. 
- -r : A range of ports
- -p : Single port or multiple ports (Comma seperated)
- -t : The timeout in milliseconds before a port is assumed to be closed (default: 1500)

### 

## Memo
-- の後にNmapのオプションを指定することで、Nmapの機能を実行することができる。
