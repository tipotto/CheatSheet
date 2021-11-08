# John the Ripper
```
john --format=[FORMAT] --wordlist=[WORDLIST PATH] [HASH FILE PATH]
```

## Search Formats
```
john --list=formats | grep -iF [FORMAT]
```

## Related Tools
ssh2john
```
python /usr/share/john/ssh2john.py [KEY FILE PATH] > passphrase.txt
```

zip2john
```
zip2john [KEY FILE PATH] > password.txt
```

## Memo
*1 ssh2johnをpython3で実行した場合、エラーが発生する。  
*2 zip2johnにはパスが通っているため、コマンドとして実行できる。
