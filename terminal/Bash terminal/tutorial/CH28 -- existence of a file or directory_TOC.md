<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [CH28 -- existence of a file or directory_TOC](#ch28-existence-of-a-file-or-directory_TOC)
   * [objectives](#objectives)
   * [CH28-1 -- terms about file](#ch28-1-terms-about-file)
      + [regular files](#regular-files)
      + [block special file](#block-special-file)
      + [character special file](#character-special-file)
      + [named pipe](#named-pipe)
   * [CH28-2 -- terms about permission](#ch28-2-terms-about-permission)
   * [read permission](#read-permission)
      + [write permission](#write-permission)
      + [execute permission](#execute-permission)
   * [CH28-3 -- terms about role and role id](#ch28-3-terms-about-role-and-role-id)
      + [SUID (set-user-id)](#suid-set-user-id)
      + [SGID (set-group-id)](#sgid-set-group-id)
      + [EUID (effective-user-id)](#euid-effective-user-id)
      + [EGID (effective-group-id)](#egid-effective-group-id)
      + [sticky bit](#sticky-bit)
   * [CH28-4 -- file management](#ch28-4-file-management)
      + [inode number, index node number](#inode-number-index-node-number)
      + [file descriptor](#file-descriptor)
   * [CH28-5 -- check a file exists](#ch28-5-check-a-file-exists)
      + [`-a`](#-a)
      + [`-e`](#-e)
   * [CH28-6 -- check a directory exists](#ch28-6-check-a-directory-exists)
      + [`-d`](#-d)
   * [CH28-7 -- check a symbolic link exists](#ch28-7-check-a-symbolic-link-exists)
      + [`-h`](#-h)
      + [`-L`](#-l)
   * [CH28-8 -- check a file is a block special file](#ch28-8-check-a-file-is-a-block-special-file)
      + [`-b`](#-b)
   * [CH28-9 -- check a file is a character special file](#ch28-9-check-a-file-is-a-character-special-file)
      + [`-c`](#-c)
   * [CH28-10 -- check a file is a regular file](#ch28-10-check-a-file-is-a-regular-file)
      + [`-f`](#-f)
   * [CH28-11 -- check a file is a named pipe](#ch28-11-check-a-file-is-a-named-pipe)
      + [`-p`](#-p)
   * [CH28-12 -- check a file is a socket](#ch28-12-check-a-file-is-a-socket)
      + [`-S`](#-s)
   * [CH28-13 -- check a file is readable](#ch28-13-check-a-file-is-readable)
      + [`-r`](#-r)
   * [CH28-14 -- check a file is writable](#ch28-14-check-a-file-is-writable)
      + [`-w`](#-w)
   * [CH28-15 -- check a file is executable](#ch28-15-check-a-file-is-executable)
      + [`-e`](#-e-1)
   * [CH28-16 -- check sticky bit of a file is set](#ch28-16-check-sticky-bit-of-a-file-is-set)
      + [`-k`](#-k)
   * [CH28-17 -- check SUID (set-user-id) bit of a file is set](#ch28-17-check-suid-set-user-id-bit-of-a-file-is-set)
      + [`-u`](#-u)
   * [CH28-18 -- check GUID (set-group-id) bit of a file is set](#ch28-18-check-guid-set-group-id-bit-of-a-file-is-set)
      + [`-g`](#-g)
   * [CH28-19 -- check a file is owned by its EUID (effective user id)](#ch28-19-check-a-file-is-owned-by-its-euid-effective-user-id)
      + [`-O`](#-o)
   * [CH28-20 -- check a file is owned by its EGID (effective group id)](#ch28-20-check-a-file-is-owned-by-its-egid-effective-group-id)
      + [`-G`](#-g-1)
   * [CH28-21 -- check a file has been modified since it was last accessed.](#ch28-21-check-a-file-has-been-modified-since-it-was-last-accessed)
      + [`-N`](#-n)
   * [CH28-22 -- check a file is newer than an another file.](#ch28-22-check-a-file-is-newer-than-an-another-file)
      + [`-nt`](#-nt)
   * [CH28-23 -- check a file is older than an another file.](#ch28-23-check-a-file-is-older-than-an-another-file)
      + [`-ot`](#-ot)
   * [CH28-24 -- check a file and an another file refer same device and same inode number](#ch28-24-check-a-file-and-an-another-file-refer-same-device-and-same-inode-number)
      + [`-ef`](#-ef)
   * [CH28-25 -- check a file has size greater than zero](#ch28-25-check-a-file-has-size-greater-than-zero)
      + [`-s`](#-s-1)
   * [CH28-26 -- check the file descriptor of a file is open and refers to terminal](#ch28-26-check-the-file-descriptor-of-a-file-is-open-and-refers-to-terminal)
      + [`-t`](#-t)

<!-- TOC end -->

<!-- TOC --><a name="ch28-existence-of-a-file-or-directory"></a>
# CH28 -- existence of a file or directory
<!-- TOC --><a name="objectives"></a>
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

<!-- TOC --><a name="ch28-1-terms-about-file"></a>
## CH28-1 -- terms about file
In Linux, all things is considered as a file.

<!-- TOC --><a name="regular-files"></a>
### regular files
Its content is its data

For example, 

+ text file => `.txt`
+ `C#` script => `.cs`
+ image file => `.jpg`,`.png` etc

<!-- TOC --><a name="block-special-file"></a>
### block special file
Its a hardware-device that can randomly access data and its data is transferred block-by-block.

For example,

+ SDA (a kind of hardware-device)
+ Flash memory

<!-- TOC --><a name="character-special-file"></a>
### character special file
Its data is transferred byte-by-byte.

For example,

+ Mouse (滑鼠)
+ Keyborad
+ Terminal

<!-- TOC --><a name="named-pipe"></a>
### named pipe
Named pipes are used in IPC (Interaction Process Communication).

One program writes data in a named pipe,

while another program reads data from the named pipe.

It is FIFO (First-In-First-Out)

It behaves like `NamedPipeServerStream`.

<!-- TOC --><a name="ch28-2-terms-about-permission"></a>
## CH28-2 -- terms about permission
In Linux, there are these permissions

  + read permission
  + write permission
  + execute permission

<!-- TOC --><a name="read-permission"></a>
## read permission
IfF one has read permission, one can read data of file.

<!-- TOC --><a name="write-permission"></a>
### write permission
IfF one has write permission, one can modify data of file.

<!-- TOC --><a name="execute-permission"></a>
### execute permission
IfF one has execute permission, one can execute the file.

<!-- TOC --><a name="ch28-3-terms-about-role-and-role-id"></a>
## CH28-3 -- terms about role and role id
<!-- TOC --><a name="suid-set-user-id"></a>
### SUID (set-user-id)
Iff set-user-id bit of the executable is set, one will execute the executable using Owner role of the executable (even if the one is NOT the owner of the executable)

<!-- TOC --><a name="sgid-set-group-id"></a>
### SGID (set-group-id)
For a file (but NOT belongs to a directory), iff set-user-id bit of the file is set, one will execute the executable using the group of the file (even if the one is NOT one of members in the group of the file)

For a directory, iff set-user-id bit of the directory is set, then the newly created file will inherit attributes from the group of the directory rather than from the group of creator.

<!-- TOC --><a name="euid-effective-user-id"></a>
### EUID (effective-user-id)
Stores the user id who can access the entry. When one user tries to access the entry, the system will check the user's id is same as EUID.

> [!NOTE]
> Typically, EUID is same as the login user for executables.
>
> EUID is the UID (user-id) of owner for executable that has SUID bit.

<!-- TOC --><a name="egid-effective-group-id"></a>
### EGID (effective-group-id)
Stores the group id who can access the entry. When one user tries to access the entry, the system will check the user's id is same as one of membership of the group whose group id is EGID.

<!-- TOC --><a name="sticky-bit"></a>
### sticky bit
Iff the stick bit of the file or directory is set, then ONLY one with root role who is owner of creator can access the file, or entries of the directory (including itself).  

<!-- TOC --><a name="ch28-4-file-management"></a>
## CH28-4 -- file management
<!-- TOC --><a name="inode-number-index-node-number"></a>
### inode number, index node number
The inode number of a file stores some metadata, including

  + file size
  + permission
  + modified time
  + location of file (the location where stores the data)
    
The indoe number of a file behaves like a ID (identity) card number of a person.

<!-- TOC --><a name="file-descriptor"></a>
### file descriptor
The file descriptor of a file describes the type file stream

In Linux, when one executables a file,

if the file descriptor returned in the program is `0`, then the file is streamed in from `stdin` standard input stream.

if the file descriptor returned in the program is `1`, then the file is streamed out from `stdout` standard output stream.

if the file descriptor returned in the program is `0`, then the file is streamed out from `stderr` standard error stream.

<!-- TOC --><a name="ch28-5-check-a-file-exists"></a>
## CH28-5 -- check a file exists
<!-- TOC --><a name="-a"></a>
### `-a`
`-a {file1_name}` returns true iff the file named `{file1_name}` exists.

<!-- TOC --><a name="-e"></a>
### `-e`
same as `-a`

<!-- TOC --><a name="ch28-6-check-a-directory-exists"></a>
## CH28-6 -- check a directory exists
<!-- TOC --><a name="-d"></a>
### `-d`
`-d {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a directory

<!-- TOC --><a name="ch28-7-check-a-symbolic-link-exists"></a>
## CH28-7 -- check a symbolic link exists
<!-- TOC --><a name="-h"></a>
### `-h`
`-h {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a symbolic link

<!-- TOC --><a name="-l"></a>
### `-L`
same as `-h`

<!-- TOC --><a name="ch28-8-check-a-file-is-a-block-special-file"></a>
## CH28-8 -- check a file is a block special file
<!-- TOC --><a name="-b"></a>
### `-b`
`-b {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a block special file.

<!-- TOC --><a name="ch28-9-check-a-file-is-a-character-special-file"></a>
## CH28-9 -- check a file is a character special file
<!-- TOC --><a name="-c"></a>
### `-c`
`-c {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a character special file.

<!-- TOC --><a name="ch28-10-check-a-file-is-a-regular-file"></a>
## CH28-10 -- check a file is a regular file
<!-- TOC --><a name="-f"></a>
### `-f`
`-f {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a regular file.

<!-- TOC --><a name="ch28-11-check-a-file-is-a-named-pipe"></a>
## CH28-11 -- check a file is a named pipe
<!-- TOC --><a name="-p"></a>
### `-p`
`-p {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a named pipe.

<!-- TOC --><a name="ch28-12-check-a-file-is-a-socket"></a>
## CH28-12 -- check a file is a socket
<!-- TOC --><a name="-s"></a>
### `-S`
`-S {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is a socket.

<!-- TOC --><a name="ch28-13-check-a-file-is-readable"></a>
## CH28-13 -- check a file is readable
<!-- TOC --><a name="-r"></a>
### `-r`
`-r {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is readable.

<!-- TOC --><a name="ch28-14-check-a-file-is-writable"></a>
## CH28-14 -- check a file is writable
<!-- TOC --><a name="-w"></a>
### `-w`
`-w {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is writable.

<!-- TOC --><a name="ch28-15-check-a-file-is-executable"></a>
## CH28-15 -- check a file is executable
<!-- TOC --><a name="-e-1"></a>
### `-e`
`-e {file1_name}` returns true iff the file named `{file1_name}` exists and `{file1_name}` is executable.

<!-- TOC --><a name="ch28-16-check-sticky-bit-of-a-file-is-set"></a>
## CH28-16 -- check sticky bit of a file is set
<!-- TOC --><a name="-k"></a>
### `-k`
`-k {file1_name}` returns true iff the file named `{file1_name}` exists and the sticky bit of `{file1_name}` is set.

<!-- TOC --><a name="ch28-17-check-suid-set-user-id-bit-of-a-file-is-set"></a>
## CH28-17 -- check SUID (set-user-id) bit of a file is set
<!-- TOC --><a name="-u"></a>
### `-u`
`-u {file1_name}` returns true iff the file named `{file1_name}` exists and the SUID (set-user-id) bit of `{file1_name}` is set.

<!-- TOC --><a name="ch28-18-check-guid-set-group-id-bit-of-a-file-is-set"></a>
## CH28-18 -- check GUID (set-group-id) bit of a file is set
<!-- TOC --><a name="-g"></a>
### `-g`
`-g {file1_name}` returns true iff the file named `{file1_name}` exists and the GUID (set-group-id) bit of `{file1_name}` is set.

<!-- TOC --><a name="ch28-19-check-a-file-is-owned-by-its-euid-effective-user-id"></a>
## CH28-19 -- check a file is owned by its EUID (effective user id)
<!-- TOC --><a name="-o"></a>
### `-O`
`-O {file1_name}` returns true iff the file named `{file1_name}` exists and the file named `{file1_name}` is owned by its EUID (effective user id).

<!-- TOC --><a name="ch28-20-check-a-file-is-owned-by-its-egid-effective-group-id"></a>
## CH28-20 -- check a file is owned by its EGID (effective group id)
<!-- TOC --><a name="-g-1"></a>
### `-G`
`-G {file1_name}` returns true iff the file named `{file1_name}` exists and the file named `{file1_name}` is owned by its EGID (effective group id).

<!-- TOC --><a name="ch28-21-check-a-file-has-been-modified-since-it-was-last-accessed"></a>
## CH28-21 -- check a file has been modified since it was last accessed.
<!-- TOC --><a name="-n"></a>
### `-N`
`-N {file1_name}` returns true iff the file named `{file1_name}` exists and it has been modified since it was last accessed.

<!-- TOC --><a name="ch28-22-check-a-file-is-newer-than-an-another-file"></a>
## CH28-22 -- check a file is newer than an another file.
<!-- TOC --><a name="-nt"></a>
### `-nt`
`{file1_name} -nt {file2_name}` returns true iff one of conditions are met

+ the file named `{file1_name}` exists and the file named `{file2_name}` does NOT exist.
+ the file named `{file1_name}` is newer (according to modification date) than the file named `{file2_name}`.

<!-- TOC --><a name="ch28-23-check-a-file-is-older-than-an-another-file"></a>
## CH28-23 -- check a file is older than an another file.
<!-- TOC --><a name="-ot"></a>
### `-ot`
`{file1_name} -ot {file2_name}` returns true iff one of conditions are met

+ the file named `{file2_name}` exists and the file named `{file1_name}` does NOT exist.
+ the file named `{file1_name}` is older (according to modification date) than the file named `{file2_name}`.

<!-- TOC --><a name="ch28-24-check-a-file-and-an-another-file-refer-same-device-and-same-inode-number"></a>
## CH28-24 -- check a file and an another file refer same device and same inode number
<!-- TOC --><a name="-ef"></a>
### `-ef`
`{file1_name} -ef {file2_name}` returns true iff all conditions are met

+ the file named `{file1_name}` and the file named `{file2_name}` refer same device.
+ the file named `{file1_name}` and the file named `{file2_name}` refer inode number.

<!-- TOC --><a name="ch28-25-check-a-file-has-size-greater-than-zero"></a>
## CH28-25 -- check a file has size greater than zero
<!-- TOC --><a name="-s-1"></a>
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

<!-- TOC --><a name="ch28-26-check-the-file-descriptor-of-a-file-is-open-and-refers-to-terminal"></a>
## CH28-26 -- check the file descriptor of a file is open and refers to terminal
<!-- TOC --><a name="-t"></a>
### `-t`
`-t {file1_name}` returns true iff the file named `{file1_name}` exists and file descriptor of the file named `{file1_name}` is open and refers to terminal.
