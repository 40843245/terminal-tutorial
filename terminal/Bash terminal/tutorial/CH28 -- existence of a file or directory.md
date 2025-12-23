# CH28 -- existence of a file or directory
## objectives
You will learn how to

  + check a file exists
  + check a directory exists
  + check a symbolic link exists
  + check it is a block special file
  + check it is a character special file
  + check it is a regular file

Also you will know these terms.

## CH28-1 -- terms about file
In Linux, all things is considered as a file.

### regular files
Its content is its data

For example, 

+ text file => `.txt`
+ `C#` script => `.cs`
+ image file => `.jpg`,`.png` etc

### block special file
Its a hardware-device that can randomly access data and its data is transferred block-by-block.

For example,

+ SDA (a kind of hardware-device)
+ Flash memory

### character special file
Its data is transferred byte-by-byte.

For example,

+ Mouse (滑鼠)
+ Keyborad
+ Terminal

### named pipe
Named pipes are used in IPC (Interaction Process Communication).

One program writes data in a named pipe,

while another program reads data from the named pipe.

It is FIFO (First-In-First-Out)

It behaves like `NamedPipeServerStream`.

## CH28-2 -- terms about permission
In Linux, there are these permissions

  + read permission
  + write permission
  + execute permission
  + SUID (set-user-id)
  + SGID (set-group-id)

## read permission
IfF one has read permission, one can read data of file.

### write permission
IfF one has write permission, one can modify data of file.

### execute permission
IfF one has execute permission, one can execute the file.

### SUID (set-user-id)
Iff set-user-id bit of the executable is set, one will execute the executable using Owner role of the executable (even if the one is NOT the owner of the executable)

### SGID (set-group-id)
For a file (but NOT belongs to a directory), iff set-user-id bit of the file is set, one will execute the executable using the group of the file (even if the one is NOT one of members in the group of the file)

For a directory, iff set-user-id bit of the directory is set, then the newly created file will inherit attributes from the group of the directory rather than from the group of creator.

### sticky bit
Iff the stick bit of the file or directory is set, then ONLY one with root role who is owner of creator can access the file, or entries of the directory (including itself).  
