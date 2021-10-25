# SMB
SMB - Server Message Block Protocol - is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network. Servers make file systems and other resources (printers, named pipes, APIs) available to clients on the network. Client computers may have their own hard disks, but they also want access to the shared file systems and printers on the servers.

## Enumeration
### Enum4Linux
Enum4linux is a tool used to enumerate SMB shares on both Windows and Linux systems. 

enum4linux [OPTIONS] [IP]

- -U : get userlist
- -M : get machine list
- -N : get namelist dump (different from -U and-M)
- -S : get sharelist
- -P : get password policy information
- -G : get group and member list
- -A : all of the above (full basic enumeration)

## Exploitation
### [SMBClient](http://www.samba.gr.jp/project/translation/3.6/htmldocs/manpages-3/smbclient.1.html)
smbclient //[IP]/[SHARE] -U [NAME] -p [PORT]  

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
