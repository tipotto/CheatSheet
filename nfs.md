# NFS
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
