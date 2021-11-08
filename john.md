# John the Ripper
```
john --format=[FORMAT] --wordlist=[WORDLIST PATH] [HASH FILE PATH]
```

#### Search format
```
john --list=formats | grep -iF [FORMAT]
```

#### ssh2john (*1)
```
python /usr/share/john/ssh2john.py [KEY FILE PATH] > passphrase.txt
```

#### zip2john
```
zip2john [KEY FILE PATH] > password.txt
```
