# CH28 -- existence of a file or directory
## objectives
You will learn how to

   * [check a file exists](#ch28-5-check-a-file-exists)
   * [check a directory exists](#ch28-6-check-a-directory-exists)
   * [check a symbolic link exists](#ch28-7-check-a-symbolic-link-exists)
   * [check a file is a block special file](#ch28-8-check-a-file-is-a-block-special-file)
   * [check a file is a character special file](#ch28-9-check-a-file-is-a-character-special-file)
   * [check a file is a regular file](#ch28-10-check-a-file-is-a-regular-file)
   * [check a file is a named pipe](#ch28-11-check-a-file-is-a-named-pipe)
   * [check a file is a socket](#ch28-12-check-a-file-is-a-socket)
   * [check a file is readable](#ch28-13-check-a-file-is-readable)
   * [check a file is writable](#ch28-14-check-a-file-is-writable)
   * [check a file is executable](#ch28-15-check-a-file-is-executable)
   * [check sticky bit of a file is set](#ch28-16-check-sticky-bit-of-a-file-is-set)
   * [check SUID (set-user-id) bit of a file is set](#ch28-17-check-suid-set-user-id-bit-of-a-file-is-set)
   * [check GUID (set-group-id) bit of a file is set](#ch28-18-check-guid-set-group-id-bit-of-a-file-is-set)
   * [check a file is owned by its EUID (effective user id)](#ch28-19-check-a-file-is-owned-by-its-euid-effective-user-id)
   * [check a file is owned by its EGID (effective group id)](#ch28-20-check-a-file-is-owned-by-its-egid-effective-group-id)
   * [check a file has been modified since it was last accessed.](#ch28-21-check-a-file-has-been-modified-since-it-was-last-accessed)
   * [check a file is newer than an another file.](#ch28-22-check-a-file-is-newer-than-an-another-file)
   * [check a file is older than an another file.](#ch28-23-check-a-file-is-older-than-an-another-file)
   * [check a file and an another file refer same device and same inode number](#ch28-24-check-a-file-and-an-another-file-refer-same-device-and-same-inode-number)
   * [check a file has size greater than zero](#ch28-25-check-a-file-has-size-greater-than-zero)
   * [check the file descriptor of a file is open and refers to terminal](#ch28-26-check-the-file-descriptor-of-a-file-is-open-and-refers-to-terminal)

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

<!-- TOC --><a name="ch28-5-check-a-file-exists"></a>
## CH28-5 -- check a file exists
### `-a`
`-a {file1_name}` returns true iff the file named `{file1_name}` exists.

### `-e`
same as `-a`

<!-- TOC --><a name="ch28-6-check-a-directory-exists"></a>
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

## CH28-10 -- check a file is a regular file
### `-f`
`-f {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a regular file.

## CH28-11 -- check a file is a named pipe
### `-p`
`-p {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a named pipe.

## CH28-12 -- check a file is a socket
### `-S`
`-S {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a socket.

## CH28-13 -- check a file is readable
### `-r`
`-r {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is readable.

## CH28-14 -- check a file is writable
### `-w`
`-w {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is writable.

## CH28-15 -- check a file is executable
### `-e`
`-e {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is executable.

## CH28-16 -- check sticky bit of a file is set
### `-k`
`-k {file1_name}` returns true iff the file named `{file1_name}` exists and the sticky bit of `{file1_name}` is set.

## CH28-17 -- check SUID (set-user-id) bit of a file is set
### `-u`
`-u {file1_name}` returns true iff the file named `{file1_name}` exists and the SUID (set-user-id) bit of `{file1_name}` is set.

## CH28-18 -- check GUID (set-group-id) bit of a file is set
### `-g`
`-g {file1_name}` returns true iff the file named `{file1_name}` exists and the GUID (set-group-id) bit of `{file1_name}` is set.

## CH28-19 -- check a file is owned by its EUID (effective user id)
### `-O`
`-O {file1_name}` returns true iff the file named `{file1_name}` exists and the file named `{file1_name}` is owned by its EUID (effective user id).

## CH28-20 -- check a file is owned by its EGID (effective group id)
### `-G`
`-G {file1_name}` returns true iff the file named `{file1_name}` exists and the file named `{file1_name}` is owned by its EGID (effective group id).

## CH28-21 -- check a file has been modified since it was last accessed.
### `-N`
`-N {file1_name}` returns true iff the file named `{file1_name}` exists and it has been modified since it was last accessed.

## CH28-22 -- check a file is newer than an another file.
### `-nt`
`{file1_name} -nt {file2_name}` returns true iff one of conditions are met

+ the file named `{file1_name}` exists and the file named `{file2_name}` does NOT exist.
+ the file named `{file1_name}` is newer (according to modification date) than the file named `{file2_name}`.

## CH28-23 -- check a file is older than an another file.
### `-ot`
`{file1_name} -ot {file2_name}` returns true iff one of conditions are met

+ the file named `{file2_name}` exists and the file named `{file1_name}` does NOT exist.
+ the file named `{file1_name}` is older (according to modification date) than the file named `{file2_name}`.

## CH28-24 -- check a file and an another file refer same device and same inode number
### `-ef`
`{file1_name} -ef {file2_name}` returns true iff all conditions are met

+ the file named `{file1_name}` and the file named `{file2_name}` refer same device.
+ the file named `{file1_name}` and the file named `{file2_name}` refer inode number.

## CH28-25 -- check a file has size greater than zero
### `-s`
`-s {file1_name}` returns true iff the file named `{file1_name}` exists and its file size greater than zero (which indicates that its data is NOT empty).

> [!NOTE]
> In Linux, its data is NOT neccessary equivalent to its content.
>
> Thus, I use the word `its data` rather than `its content` in context of
>
> ```
> which indicates that its data is NOT empty
> ```

## CH28-26 -- check the file descriptor of a file is open and refers to terminal
### `-t`
`-t {file1_name}` returns true iff the file named `{file1_name}` exists and file descriptor of the file named `{file1_name}` is open and refers to terminal.
