# SMTP
SMTP stands for "Simple Mail Transfer Protocol". It is utilised to handle the sending of emails. In order to support email services, a protocol pair is required, comprising of SMTP and POP/IMAP. Together they allow the user to send outgoing mail and retrieve incoming mail, respectively.

## Workflow
1. Port scan with nmap
2. Enumerate useful information

## Advance preparation
```
export rhost=[RHOST IP]
```

## Enumeration
### Metasploit
Determine the version of mail servers
```
use auxiliary/scanner/smtp/smtp_version
```

Enumerate the users of mail servers. (*1 *2) 
```
use auxiliary/scanner/smtp/smtp_enum
```
*1 デフォルトのユーザーファイルではなく、top-usernames-shortlist.txt(/usr/share/wordlists/secLists/Usernames/top-usernames-shortlist.txt)を使っても良い。  

*2 auxiliary/scanner/smtp/smtp_versionの後に続けて実行すると、前のモジュールのものと同じ実行結果が表示される。  
その場合、Metasploitを再起動すると正常に実行される。
