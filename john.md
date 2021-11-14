# John the Ripper
## Usage
```
john --format=[FORMAT] --wordlist=[WORDLIST PATH] [HASH FILE PATH]
```

## Search Formats
```
john --list=formats | grep -iF [FORMAT]
```

## Related Tools
### ssh2john
python3で実行した場合、エラーが発生する。
```
python /usr/share/john/ssh2john.py [KEY FILE PATH] | tee passphrase.txt
```

### zip2john
パスが通っているため、コマンドとして実行できる。
```
zip2john [KEY FILE PATH] | tee password.txt
```

zip や 7z コマンドで作成した zip ファイルの場合、出力されるパスワードハッシュに余計な文字列が含まれる。
```
zip -e test.zip test.txt 
zip2john test.zip | tee password.txt
test.zip/test.txt:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx:test.txt:test.zip::test.zip
```

そのため、ファイル出力時にハッシュ部分だけを抽出する。（デリミタ : の2番目のフィールドの内容だけを抽出して出力）
```
zip2john test.zip | cut -d ":" -f 2 | tee password.txt
```

## Memo
*1 John the Ripper は明示的にフォーマットを指定しなくても、自動的に判断してくれる。  
*2 zip, 7z コマンドで作成した zip ファイルのパスワードハッシュは、$pkzip2$ で始まり、$/pkzip2$ で終わる。
