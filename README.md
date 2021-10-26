# CheatSheet

## Ports
- 20, 21 / FTP
- 22 / SSH
- 23 / Telnet
- 25 / SMTP
- 53 / DNS
- 80 / HTTP
- 110 / POP3
- 443 / HTTPS
- 139, 445 / SMB

## Tools
### Nmap
```
nmap [OPTIONS] [IP]
```

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

### John the Ripper
```
john --format=[FORMAT] --wordlist=[WORDLIST PATH] [HASH FILE PATH]
```

#### Search format
```
john --list=formats | grep -iF [FORMAT]
```

### MSFVenom
```
msfvenom -p [PAYLOAD] lhost=[LOCAL IP] lport=[LOCAL PORT] R
```

- -p : payload
- lhost : our local host IP address (this is your machine's IP address)
- lport : the port to listen on (this is the port on your machine)
- R : export the payload in raw format

### Hydra
Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more.   
```
hydra -t [NUM OF CONNS] -l [USER] -P [WORDLIST] -vV [IP] [PROTOCOL]
```

- -t : Number of parallel connections per target
- -l : Username
- -P : Wordlist path
- -vV : Sets verbose mode to very verbose, shows the login+pass combination for each attempt

## Exploit by protocol
- [SSH](ssh.md)
- [SMB](smb.md)
- [Telnet](telnet.md)
- [NFS](nfs.md)
- [FTP](ftp.md)
- [SMTP](smtp.md)
- [MySQL](mysql.md)

## Reference

Exploit DB  
https://www.exploit-db.com/

MSF Console Commands  
https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/

Meterpreter Commands  
https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/

Reverse Shell Cheat Sheet
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md

GTFOBins  
https://gtfobins.github.io/

Linux - Privilege Escalation
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md#find-suid-binaries
