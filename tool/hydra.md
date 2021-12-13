# Hydra
Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more.   

## Normal
```
hydra -l [USER] -P [WORDLIST] [IP] [PROTOCOL] -t [NUM OF CONNS] -vV
```

- -l : Username
- -L : Userlist path
- -p : Password
- -P : Wordlist path
- -s : Port
- -f : Exit when a login/pass pair is found
- -t : Number of parallel connections per target (default: 16)
- -w : defines the max wait time in seconds for responses (default: 32)
- -vV : Sets verbose mode to very verbose, shows the login+pass combination for each attempt

## Basic Authentication
```
hydra -l [USER] -P [WORDLIST] [IP] http-get [REQUEST PATH]
```

## Post Form
```
hydra -l [USER] -P [WORDLIST] [IP] http-post-form "[REQUEST PATH]:[REQUEST BODY]:[ERROR MESSAGE]"
```

## Memo
*1 REQUEST PATH では、リクエストパスの先頭に/を付けないとエラーになる。  
*2 REQUEST BODY ではユーザー名、パスワードが入る箇所をそれぞれ、^USER^, ^PASS^ というマジックパラメータに変更する。  
*3 もし指定したユーザーが存在しない場合、すぐに実行終了する。（詳細は Verbose モードで確認）  
