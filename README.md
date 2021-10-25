# CheatCheet

## Ports
20, 21 / FTP  
22 / SSH  
23 / Telnet  
25 / SMTP  
53 / DNS  
80 / HTTP  
110 / POP3  
443 / HTTPS  
139, 445 / SMB  

## Tools

### Payload
#### MSFVenom
msfvenom -p [PAYLOAD] lhost=[LOCAL IP] lport=[LOCAL PORT] R  

-p : payload  
lhost : our local host IP address (this is your machine's IP address)  
lport : the port to listen on (this is the port on your machine)  
R : export the payload in raw format  

### Online Password Cracking
#### Hydra
Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more.   

hydra -t [NUM OF CONNS] -l [USER] -P [WORDLIST] -vV [IP] [PROTOCOL]  

-t : Number of parallel connections per target  
-l : Username  
-P : Wordlist path  
-vV : Sets verbose mode to very verbose, shows the login+pass combination for each attempt  

### SSH
ssh -i [KEY FILE] [USERNAME]@[IP]  

### SMB
SMB - Server Message Block Protocol - is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network. Servers make file systems and other resources (printers, named pipes, APIs) available to clients on the network. Client computers may have their own hard disks, but they also want access to the shared file systems and printers on the servers.

#### Enumeration
##### Enum4Linux  
Enum4linux is a tool used to enumerate SMB shares on both Windows and Linux systems. 

enum4linux [OPTIONS] [IP]

-U : get userlist  
-M : get machine list  
-N : get namelist dump (different from -U and-M)  
-S : get sharelist  
-P : get password policy information  
-G : get group and member list  
-A : all of the above (full basic enumeration)  

#### Exploitation
##### SMBClient  
smbclient //[IP]/[SHARE] -U [NAME] -p [PORT]  

-U [name] : to specify the user  
-p [port] : to specify the port  


### Telnet
Telnet is an application protocol which allows you, with the use of a telnet client, to connect to and execute commands on a remote machine that's hosting a telnet server. Telnet sends all messages in clear text and has no specific security mechanisms. Thus, in many applications and services, Telnet has been replaced by SSH in most implementations.

telnet [IP] [PORT]

### NFS
NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.

#### Listing NFS Shares
/usr/sbin/showmount -e [IP]  

#### Mounting NFS shares
sudo mount -t nfs [IP]:[SHARE] [MOUNT DIR] -nolock

-t : Type of device to mount  
IP : The IP Address of the NFS server  
SHARE	: The name of the share in the remote host  
MOUNT DIR: The local directory to mount  
-nolock	: Specifies not to use NLM locking  

### FTP
File Transfer Protocol (FTP) is a protocol used to allow remote transfer of files over a network.  

### SMTP
SMTP stands for "Simple Mail Transfer Protocol". It is utilised to handle the sending of emails. In order to support email services, a protocol pair is required, comprising of SMTP and POP/IMAP. Together they allow the user to send outgoing mail and retrieve incoming mail, respectively.

Metasploit Module
Scan a range of IP addresses and determine the version of mail servers.
auxiliary/scanner/smtp/smtp_version

Enumerate the users of mail servers.
auxiliary/scanner/smtp/smtp_enum

### MySQL  
mysql -h [IP] -u [USERNAME] -p

To install MySQL client:  
sudo apt install default-mysql-client  

## Reference

Exploit DB  
https://www.exploit-db.com/

MSF Console Commands  
https://www.offensive-security.com/metasploit-unleashed/msfconsole-commands/

Meterpreter Commands  
https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/

Reverse Shell Cheat Sheet
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md

GTFOBins  
https://gtfobins.github.io/

Linux - Privilege Escalation
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md#find-suid-binaries
