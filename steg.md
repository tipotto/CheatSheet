# steganography

## Tools

### Steghide
png ファイルには対応していないため注意。  

#### ステゴファイルの作成
```
steghide embed -cf [FILE] -ef [FILE] -sf [FILE]
```

#### データの抽出
```
steghide extract -sf [FILE]
```

- -cf : ファイルを埋め込むカバーファイル
- -ef : 埋め込みたいデータファイル
- -sf : 作成されたステゴファイル

### binwalk
ファイル内に埋め込まれている別のファイルを抽出する
```
binwalk -e [FILE]
```

- -e : extract

### foremost
ファイルのヘッダやフッタ、内部データ構造に基づいてファイルを復元する。png にも対応。

```
foremost [FILE]
```

### strings
バイナリファイルから、表示可能な文字列を抽出して表示する
```
strings [FILE]
```

### exiftool
画像のメタデータを表示する
```
exiftool [FILE]
```

表示されるデータ
```
ExifTool Version Number         : 12.32
File Name                       : test.jpg
Directory                       : .
File Size                       : 187 KiB
File Modification Date/Time     : 2021:11:12 16:50:25+09:00
File Access Date/Time           : 2021:11:12 16:53:35+09:00
File Inode Change Date/Time     : 2021:11:12 16:51:14+09:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Image Width                     : 1200
Image Height                    : 1600
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 1200x1600
Megapixels                      : 1.9
```
