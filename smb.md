# SMB / Port 139, 445
SMB - Server Message Block Protocol - is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network. Servers make file systems and other resources (printers, named pipes, APIs) available to clients on the network. Client computers may have their own hard disks, but they also want access to the shared file systems and printers on the servers.

## Workflow
1. Port scan with nmap
2. Enumerate with Enum4Linux
3. Access the vulnarable share with SMBClient  
4. Investigate files and directories to see if it is accessible to the host machine in some ways
5. SSH into the remote host (if private key is found)

## Advance preparation
```
export rhost=[RHOST IP]
export rport=[RHOST PORT]
```

## Port scan
Check if port 139 or 445 is open
```
sudo nmap -sS -sV -O $rhost
```

## Enumeration
### Enum4Linux (*1)
Enum4linux is a tool used to enumerate SMB shares on both Windows and Linux systems. 
```
enum4linux [OPTIONS] $rhost | tee [OUTPUT FILE]
```

- -U : get userlist
- -M : get machine list
- -N : get namelist dump (different from -U and-M)
- -S : get sharelist
- -P : get password policy information
- -G : get group and member list
- -A : all of the above (full basic enumeration)

## Exploitation
### SMBClient
```
smbclient //${rhost}/[SHARE] -U [NAME] -p $rport  
```

- -U [name] : to specify the user
- -p [port] : to specify the port

#### Basic commands
- ls : List files and directories
- cd : Change to a specified directory
- more : Examine the contents of a text file
- pwd : Show current directory
- get : Retrieve a file
- mget : Retrieve multiple files
- put : Upload a file
- mput : Upload multiple files
- exit : Close the session

All the commands are [here](http://www.samba.gr.jp/project/translation/3.6/htmldocs/manpages-3/smbclient.1.html).

### SSH into the remote host (*2)
```
chmod 600 [PRIVATE KEY]
ssh -i [PRIVATE KEY] [USERNAME]@${rhost}
```

## Memo
*1
- Session Checkセクションで<strong>Server xxx allows sessions using username '', password ''</strong>と表示されている場合は、匿名ログインが可能。
- Share Enumerationセクションで<strong>Mapping: OK, Listing: OK</strong>と表示されている場合、そのshareにSMBClientでアクセス可能。

*2 Private keyにOthersに対する権限が付与されている場合、SSHクライアントで拒否されるため、適切な権限を付与する必要がある。

## Reference

SMB Access from Linux Cheat Sheet  
https://www.willhackforsushi.com/sec504/SMB-Access-from-Linux.pdf
