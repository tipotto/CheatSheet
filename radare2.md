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
