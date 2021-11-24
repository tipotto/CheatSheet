# radare2

## Registers
| 64 bit | 32 bit | meaning |
|:------:|:------:|:-------:|
| %rax   | %eax   |         |
| %rbx   | %ebx   |         |
| %rcx   | %ecx   |         |
| %rdx   | %edx   |         |
| %rsi   | %esi   |         |
| %rdi   | %edi   |         |
| %rsp   | %esp   | stack pointer |
| %rbp   | %ebp   | frame pointer |
| %r8    | %r8d   |         |
| %r9    | %r9d   |         |
| %r10   | %r10d  |         |
| %r11   | %r11d  |         |
| %r12   | %r12d  |         |
| %r13   | %r13d  |         |
| %r14   | %r14d  |         |
| %r15   | %r15d  |         |


## Instructions
| mnemonic | instruction | meaning |
|:------:|:------:|:------:|
| lea | ```leaq src, dst``` | Sets dst to the address denoted by the expression in src |
| add | ```addq src, dst``` | dst = dst + src |
| sub | ```subq src, dst``` | dst = dst - src |
| imul | ```imulq src, dst``` | dst = dst * src |
| sal | ```salq src, dst``` | dst = dst << src ( << is the left bit shifting operator) |
| sar | ```sarq src, dst``` | dst = dst >> src ( >> is the right bit shifting operator) |
| xor | ```xorq src, dst``` | dst = dst XOR src |
| and | ```andq src, dst``` | dst = dst & src |
| or | ```orq src, dst``` | dst = dst | src |


## Last letter of instruction
| type             | letter | size (bytes) |
|:----------------:|:------:|:------------:|
| Byte             | b      | 1            |
| Word             | w      | 2            |
| Double Word      | l      | 4            |
| Quad Word        | q      | 8            |
| Single Precision | s      | 4            |
| Double Precision | l      | 8            |


## Commands
### バイナリファイルをデバッグモードで開く
```
r2 -d [FILE]
```

### プログラムの分析
```
aa
aaa
```

### ディスアセンブリ シンタックスの設定
AT&T の場合
```
e asm.syntax=att
```

Intel の場合
```
e asm.syntax=intel
```

### 関数のリストアップ
```
afl
```

### コードの検証
Main 関数(エントリーポイント)の検証
```
pdf @main
```

### ブレイクポイントの設定
```
db [MEMORY ADDR]
```

### 次のブレイクポイントまで実行
```
dc
```

### 次の命令を実行
```
ds
```

### 各レジスタの値を参照
```
dr
```

### ローカル変数の値を参照
引数には、各ローカル変数に対応する @ から始まる値を渡す。その値は ```pdf @main``` を実行すると、出力結果の上部に表示される。
```
px [POSITION OF LOCAL VAL]
```

一番左上の「6c」がローカル変数に格納されている値。値は16進数のため、6c = 108 となる。

![スクリーンショット 2021-11-24 17 29 57](https://user-images.githubusercontent.com/39334151/143202326-0b989e4e-4cfc-445b-b457-23bb9ba5c8f1.png)

### 定数

定数は $ から始まり、16進数で表される。ここでは 0x14 = 20, 0x16 = 22 をそれぞれローカル変数 var_ch, car_8h に代入している。

![スクリーンショット 2021-11-24 17 35 21](https://user-images.githubusercontent.com/39334151/143203106-0f571861-e05b-48ee-916e-69bc96c3475a.png)

## References

10進数・16進数変換ツール  
http://tool.muzin.org/16/
