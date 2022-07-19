# John the Ripper

## Usage
明示的にフォーマットを指定しなくても、自動的に判断してくれる。  
```
sudo john --format=[FORMAT] --wordlist=[WORDLIST PATH] [HASH FILE PATH]
```
```
wordlist=xato-net-10-million-passwords-1000.txt; sudo john --format=[FORMAT] --wordlist=/usr/share/seclists/Passwords/$wordlist hash.txt
```

## Search Formats
```
sudo john --list=formats | grep -iF [FORMAT]
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
test.zip/test.txt:$pkzip2$xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx$/pkzip2$:test.txt:test.zip::test.zip
```

そのため、ファイル出力時にハッシュ部分だけを抽出する。（デリミタ : の2番目のフィールドの内容だけを抽出して出力）
```
zip2john test.zip | cut -d ":" -f 2 | tee password.txt
```

zip, 7z コマンドで作成した zip ファイルのパスワードハッシュは、$pkzip2$ で始まり、$/pkzip2$ で終わる。
```
cat password.txt
$pkzip2$xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx$/pkzip2$
```

## Potfile
一度クラックされたパスワードはポットファイルに保存される。そのため、2回目に同じパスワードをクラックしようとしてもスキップされる。

```
nano ~/.john/john.pot
```
