# [Gobuster](https://github.com/OJ/gobuster)
```
gobuster dir -u [URL] -w [WORDLIST PATH] -t [NUM OF THREADS] -x [EXTENSIONS] -o [OUTPUT FILE] -q
```

- -u : The target URL
- -w : Path to the wordlist
- -t : Number of concurrent threads (default 10)
- -x : File extension(s) to search for
- -o : File name to output to
- -q : Don't print the banner and other noise

# Reference
dir-mode options  
https://github.com/OJ/gobuster#dir-mode-options

dir-mode example  
https://github.com/OJ/gobuster#dir-mode

# Memo
*1 ディレクトリ内のサブディレクトリの検索はしてくれないので注意（dirbusterであれば可能）  
*2 スレッド数は100くらいが良さそう。150-200くらいだとブラウザ表示などでエラーが発生するため、他の操作が難しくなる。
