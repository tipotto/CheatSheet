# [Gobuster](https://github.com/OJ/gobuster)

## dir
### command
```
gobuster dir -u [URL] -w [WORDLIST PATH] -t [NUM OF THREADS] -x [EXTENSIONS] -o [OUTPUT FILE] -k -r -q
```
```
wordlist=raft-small-words.txt; gobuster dir -u http://$rhost -w /usr/share/seclists/Discovery/Web-Content/$wordlist -t 100 -x php,txt -o gobuster/$wordlist.out -k -r -q
```

### wordlist
```
/usr/share/seclists/Discovery/Web-Content
```

### Reference
options:  
https://github.com/OJ/gobuster#dir-mode-options

example:  
https://github.com/OJ/gobuster#dir-mode


## vhost
### command
```
gobuster vhost -u [URL] -w [WORDLIST PATH] -t [NUM OF THREADS] -o [OUTPUT FILE] -k -r -q
```
```
wordlist=subdomains-top1million-5000.txt; gobuster vhost -u http://$rhost -w /usr/share/seclists/Discovery/DNS/$wordlist -t 100 -o gobuster/$wordlist.out -k -r -q
```

### wordlist
```
/usr/share/seclists/Discovery/DNS/
```

### Reference
options:  
https://github.com/OJ/gobuster#vhost-mode-options

example:  
https://github.com/OJ/gobuster#vhost-mode

## options
- -u : The target URL
- -w : Path to the wordlist
- -t : Number of concurrent threads (default 10)
- -x : File extension(s) to search for
- -o : File name to output to
- -q : Don't print the banner and other noise
- -k : Skip TLS certificate verification
- -r : Follow redirects 

## Memo
*1 ディレクトリ内のサブディレクトリの検索はしてくれないので注意（dirbusterであれば可能）  
*2 スレッド数は100くらいが良さそう。150-200くらいだとブラウザ表示などでエラーが発生するため、他の操作が難しくなる。
