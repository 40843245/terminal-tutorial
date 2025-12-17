# CH16 -- directory and directory stack
## objectives
You will know 

  + what is directory
  + how to look at current working directory
  + how to look at previous working directory
  + how to change current working directory
  + what is directory stack
  + how to look at directory stack
  + how to push directory stack
  + how to pop up directory stack

## CH16-1 -- introduction of directory
A directory is a parent of a folder or a file.

If a directory has one child or more children, then it is NOT an empty directory.

Otherwise, it is an empty directory.

### Examples
#### Example 1

Given the following structure, 

```
D:\workspace\Bash\Bash tutorial>tree
Folder PATH listing for volume 新增磁碟區
Volume serial number is <volume-serial-number>
D:.
├─examples
│  ├─command grouping
│  ├─declare
│  ├─getopts
│  ├─IFS
│  ├─joining
│  ├─parsing long options
│  ├─Regex
│  ├─shell expansions
│  │  └─brace expansion
│  ├─status of functionalities
│  ├─unset
│  └─variables
└─utility modules
    ├─functionality enabled
    ├─joining
    ├─Regex
    └─unset
```

The directory `D:\workspace\Bash\Bash tutorial` has two folders named `examples` and `utility modules`.

## CH16-2 -- look at current working directory
### `$PWD`
`$PWD`: a shorthand of *p*resent *w*orking *d*irectory. It will be expanded into current working directory.

## CH16-3 -- look at previous working directory
### `$PWD`
`$OLDPWD`: a shorthand of *old* *p*resent *w*orking *d*irectory. It will be expanded into previous working directory.

## CH16-3 -- change current working directory
### `cd`
The built-in command `cd` can change current directory to specific directory

> [!IMP
## CH16-4 -- introduction of directory stack
A directory stack is a stack that stores the order of directory path.

It use FILO (First-In-Last-Out)

An example illustrates the directory stack after pushing and popping up.

![directory stack-example2](https://github.com/user-attachments/assets/a2e4bc9b-7599-4a3c-aedc-62659bc7d5ca)

## CH
