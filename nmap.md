# Nmap

```
nmap [OPTIONS] [IP]
```

## Options
- -sT : TCP Connect Scan
- -sS : SYN Scan
- -sU : UDP Scan
- -sN : NULL Scan
- -sF : FIN Scan
- -sX : Xmas Scan
- -sn : Ping Sweep
- -sV : Service / Version Detection
- -sC : Common script scanning
- -O : OS Detection
- -vv : Verbose mode level two
- -A : Aggressive mode (Service / Version detection, OS detection, a traceroute and common script scanning)
- -oA : Save results in three major formats
- -oN : Save results in a normal format
- -oG : Save results in a grepable format
- -T : The timing / speed your scan is run at (e.g. -T5 / Nmap offers five levels. Higher speeds are noisier, and can incur errors)
- -p : Scans a specific port or range of ports (e.g. -p 80 / -p 1000-1500)
- -p- : Scans all ports
- --script : Activate a script from the nmap scripting library (e.g. --script=vuln)

## NSE (The Nmap Scripting Engine)
