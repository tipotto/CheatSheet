# radare2

## Registers
| 64 bit | 32 bit | name   |
|:------:|:------:|:------:|
| %rax   | %eax   |        |
| %rbx   | %ebx   |        |
| %rcx   | %ecx   |        |
| %rdx   | %edx   |        |
| %rsi   | %esi   |        |
| %rdi   | %edi   |        |
| %rsp   | %esp   | stack pointer |
| %rbp   | %ebp   | frame pointer |
| %r8    | %r8d   |        |
| %r9    | %r9d   |        |
| %r10   | %r10d  |        |
| %r11   | %r11d  |        |
| %r12   | %r12d  |        |
| %r13   | %r13d  |        |
| %r14   | %r14d  |        |
| %r15   | %r15d  |        |

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
以下は AT & T の場合
```
e asm.syntax=att
```

### 関数のリストアップ
```
afl
```

### コードの検証
以下は Main 関数の検証
```
pdf @main
```

