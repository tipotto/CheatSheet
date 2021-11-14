# Hashcat

## Usage
```
hashcat -m [HASH TYPE] -a [ATTACK MODE] [HASHFILE / HASH] [WORDLIST PATH]
```

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

## インクリメンタル攻撃
increment オプションで指定されたマスク形式の文字数に到達するまで、1文字ずつ増やして試行する。（'?l?l?l?l' であれば、アルファベット小文字を1文字ずつ増やしつつ、最大4文字まで試行する）  
```
hashcat -m [HASH TYPE] -a 3 [HASHFILE / HASH] --increment [MASK] 
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
Hashcat Example Hashes  
https://hashcat.net/wiki/doku.php?id=example_hashes

Hashcat Attack Modes  
https://hashcat.net/wiki/
