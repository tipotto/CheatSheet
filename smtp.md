# SMTP / Port 25
SMTP stands for "Simple Mail Transfer Protocol". It is utilised to handle the sending of emails. In order to support email services, a protocol pair is required, comprising of SMTP and POP/IMAP. Together they allow the user to send outgoing mail and retrieve incoming mail, respectively.

## Workflow
1. Port scan with nmap
2. Enumerate useful information
3. Crack password with hydra

## Advance preparation
```
export rhost=[RHOST IP]
```

## Port scan
Check if port 25 is open
```
sudo nmap -sS -sV -O $rhost
```

## Enumeration
### Metasploit
Determine the version of mail servers
```
use auxiliary/scanner/smtp/smtp_version
```

Enumerate the users of mail servers. (*1)(*2) 
```
use auxiliary/scanner/smtp/smtp_enum
```

## Exploitation
### Hydra
Crack the password of the user found using Metasploit module
```
hydra -t 16 -l [USERNAME] -P /usr/share/wordlists/rockyou.txt -vV $rhost ssh
```

## Memo
*1 デフォルトのユーザーファイルではなく、top-usernames-shortlist.txt(/usr/share/wordlists/secLists/Usernames/top-usernames-shortlist.txt)を使っても良い。  

*2 auxiliary/scanner/smtp/smtp_versionの後に続けて実行すると、前のモジュールのものと同じ実行結果が表示されることがある。  
その場合、Metasploitを再起動すると正常に実行される。
