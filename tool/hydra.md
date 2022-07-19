# [Hydra](https://hydra.cc/docs/intro/)
## command
normal:
```
hydra -l [USER] -P [WORDLIST] [IP] [PROTOCOL] -t [NUM OF CONNS]
```
```
wordlist=xato-net-10-million-passwords-1000.txt; hydra -l [USER] -P /usr/share/seclists/Passwords/$wordlist $rhost http -t 100
```

basic authentication:
```
hydra -l [USER] -P [WORDLIST] [IP] http-get [REQUEST PATH]
```
```
wordlist=xato-net-10-million-passwords-1000.txt; hydra -l [USER] -P /usr/share/seclists/Passwords/$wordlist $rhost http-get "[REQUEST PATH]"
```

post form:
```
hydra -l [USER] -P [WORDLIST] [IP] http-post-form "[REQUEST PATH]:[REQUEST BODY]:[ERROR MESSAGE]"
```
```
wordlist=xato-net-10-million-passwords-1000.txt; hydra -l [USER] -P /usr/share/seclists/Passwords/$wordlist $rhost http-post-form "[REQUEST PATH]:[REQUEST BODY]:[ERROR MESSAGE]"
```

## options
- -l : Username
- -L : Userlist path
- -p : Password
- -P : Wordlist path
- -s : Port
- -f : Exit when a login/pass pair is found
- -t : Number of parallel connections per target (default: 16)
- -w : defines the max wait time in seconds for responses (default: 32)
- -vV : Sets verbose mode to very verbose, shows the login+pass combination for each attempt

## wordlist
```
/usr/share/seclists/Passwords/
```

## memo
*1 REQUEST PATH では、リクエストパスの先頭に/を付けないとエラーになる。  
*2 REQUEST BODY ではユーザー名、パスワードが入る箇所をそれぞれ、^USER^, ^PASS^ というマジックパラメータに変更する。  
*3 もし指定したユーザーが存在しない場合、すぐに実行終了する。（詳細は Verbose モードで確認）  
