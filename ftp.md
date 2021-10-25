# FTP
File Transfer Protocol (FTP) is a protocol used to allow remote transfer of files over a network.  

ftp [IP]

## Workflow
1. Port scan with nmap
2. Enumerate by logging in to an FTP server, or exploiting [this](https://www.exploit-db.com/exploits/20745) vulnerability.
3. Crack password (In case any useful information such as username is found in enumeration stage)
