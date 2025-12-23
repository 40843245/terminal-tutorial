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

## read permission
IfF one has read permission, one can read data of file.

### write permission
IfF one has write permission, one can modify data of file.

### execute permission
IfF one has execute permission, one can execute the file.

## CH28-3 -- terms about role and role id
### SUID (set-user-id)
Iff set-user-id bit of the executable is set, one will execute the executable using Owner role of the executable (even if the one is NOT the owner of the executable)

### SGID (set-group-id)
For a file (but NOT belongs to a directory), iff set-user-id bit of the file is set, one will execute the executable using the group of the file (even if the one is NOT one of members in the group of the file)

For a directory, iff set-user-id bit of the directory is set, then the newly created file will inherit attributes from the group of the directory rather than from the group of creator.

### EUID (effective-user-id)
Stores the user id who can access the entry. When one user tries to access the entry, the system will check the user's id is same as EUID.

> [!NOTE]
> Typically, EUID is same as the login user for executables.
>
> EUID is the UID (user-id) of owner for executable that has SUID bit.

### EGID (effective-group-id)
Stores the group id who can access the entry. When one user tries to access the entry, the system will check the user's id is same as one of membership of the group whose group id is EGID.

### sticky bit
Iff the stick bit of the file or directory is set, then ONLY one with root role who is owner of creator can access the file, or entries of the directory (including itself).  

## CH28-4 -- file management
### inode number, index node number
The inode number of a file stores some metadata, including

  + file size
  + permission
  + modified time
  + location of file (the location where stores the data)
    
The indoe number of a file behaves like a ID (identity) card number of a person.

### file descriptor
The file descriptor of a file describes the type file stream

In Linux, when one executables a file,

if the file descriptor returned in the program is `0`, then the file is streamed in from `stdin` standard input stream.

if the file descriptor returned in the program is `1`, then the file is streamed out from `stdout` standard output stream.

if the file descriptor returned in the program is `0`, then the file is streamed out from `stderr` standard error stream.

## CH28-5 -- check a file exists
### `-a`
`-a {file1_name}` returns true iff the file named `{file1_name}` exists.

### `-e`
same as `-a`

## CH28-6 -- check a file exists
### `-d`
`-d {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a directory

## CH28-7 -- check a symbolic link exists
### `-h`
`-h {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a symbolic link

### `-L`
same as `-h`

## CH28-8 -- check a file is a block special file
### `-b`
`-b {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a block special file.

## CH28-9 -- check a file is a character special file
### `-c`
`-c {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a character special file.

## CH28-10-- check a file is a regular file
### `-f`
`-f {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a regular file.

## CH28-11-- check a file is a named pipe
### `-p`
`-p {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a named pipe.

## CH28-12-- check a file is readable
### `-r`
`-r {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a readable.

## CH28-15 -- check sticky bit of a file is set
### `-k`
`-k {file1_name}` returns true iff the file named `{file1_name}` exists and the sticky bit of `{file1_name}` is set.

