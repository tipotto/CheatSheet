# Hashcat

## Core Attack Modes
- 0 : 辞書式攻撃
- 1 : コンビネーター攻撃（複数の辞書ファイルから文字を連結して試行する）
- 3 : ブルートフォース攻撃 / マスク攻撃

## Mask
| オプション   | 意味             | 内容                              |
|:-----------|:-----------------|:---------------------------------|
| ?l         | アルファベット小文字 | abcdefghijklmnopqrstuvwxyz       |
| ?u         | アルファベット大文字 | ABCDEFGHIJKLMNOPQRSTUVWXYZ       |
| ?d         | 数字              | 0123456789                       |
| ?s         | 記号              | !”#$%&'()*+,-./:;<=>?@[\]^_`{|}~ |
| ?a         | ?l?u?d?s と同じ   |                                  |
| ?b         | 16進数            | 0x00 – 0xff                      |

## 攻撃方法
### 辞書式攻撃（Mode 0）
指定した辞書ファイルにある文字列を1つずつ試行する。
```
hashcat -a 0 -m [HASH TYPE] [HASHFILE / HASH] [WORDLIST PATH]
```

### ブルートフォース攻撃（Mode 3）
```
hashcat -a 3 -m [HASH TYPE] [HASHFILE / HASH]
```

### マスク攻撃（Mode 3）
マスクで指定された文字形式、文字数で総当たりして試行する。（'?d?d?d?d' であれば、4文字の数字に絞って総当たりする。）
```
hashcat -a 3 -m [HASH TYPE] [HASHFILE / HASH] [MASK]
```

### インクリメンタル攻撃（Mode 3）
increment オプションで指定されたマスク形式の文字数に到達するまで、1文字ずつ増やして試行する。（'?l?l?l?l' であれば、アルファベット小文字を1文字ずつ増やしつつ、最大4文字まで試行する）  
```
hashcat -a 3 -m [HASH TYPE] [HASHFILE / HASH] --increment [MASK]
```

## ポットファイルの確認
一度クラックされたパスワードはポットファイルに保存される。そのため、2回目に同じパスワードをクラックしようとすると実行されない。
```
nano ~/.hashcat/hashcat.potfile
```

## Related Tools
Hashid  
- Hash-identifier や他のハッシュ解析ツールでは特定できないハッシュタイプも特定可能。また、解析結果に Hashcat のモード番号を表示する。
- ハッシュファイルだけでなく、ハッシュを指定することも可能。
```
hashid -m '[HASH]'
```

## Memo
*1 ハッシュをそのまま指定する際は、ハッシュに $ や * などの特殊文字が含まれている場合、シェルに解釈されて意図しない挙動になってしまうことがあるため、シングルクウォーテーションで囲むようにする。  

## Reference
Hashcat / Example Hashes:  
https://hashcat.net/wiki/doku.php?id=example_hashes

Hashcat / Attack Modes:  
https://hashcat.net/wiki/

fr33f0r4ll / Hashcat 使い方:  
https://hiziriai.hatenablog.com/entry/2018/09/03/103116

家studyをつづって / hashcatの使い方:  
https://www.iestudy.work/entry/2020/11/20/215526
