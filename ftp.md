# FTP
File Transfer Protocol (FTP) is a protocol used to allow remote transfer of files over a network.  
```
ftp [IP]
```

## Workflow
1. Port scan with nmap
2. Enumerate by logging in to an FTP server, or exploiting [this](https://www.exploit-db.com/exploits/20745) vulnerability.
3. Crack password (In case any useful information such as username is found in enumeration stage)

## Useful Commands
### ls
ls -la で隠しディレクトリ/ファイルを表示することもできる。

### lcd
ローカル（Kali）のカレントディレクトリをFTPから変更できる。

### !dir
ローカル（Kali）のカレントディレクトリの内容を表示する。

### get
第1引数に取得ファイル名、第2引数に保存先ファイル名を指定する。第2引数を省略すると、ローカルのカレントディレクトリ（lcd コマンドで変更可能）に、取得ファイルと同じファイル名で保存される。
```
get [FTP FILE] [LOCAL LOCATION]
```

### mget
複数ファイルを一括ダウンロードする。i オプションをつけない場合、各ファイルごとに Yes / No を求められる。

### pwd
### cd
### put
### mput

## Memo
*1 匿名ログインが可能な場合、ユーザー名に anonymous と入力しないと認証に失敗する（パスワードは必要なし）

## Reference
ftpコマンドについて詳しくまとめました 【Linuxコマンド集】  
https://eng-entrance.com/linux-command-ftp

TCP/IP - FTP Command  
https://www.infraexpert.com/study/tcpip22.5.html
