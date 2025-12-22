# CH27 -- directory
## objectives
You will know how to

  + list all directories and folders of a directory
  + list specific directories and folders of a directory
  + create an empty directory
  + delete a directory
  + copy a directory
  + move a directory
  + rename a directory

## CH27-1 -- list all directories and folders of a directory
### `ls`
`ls` built-in command will list directories and folders of a directory.

> [!IMPORTANT]
> It will list its directly children rather than its descendants.
>
> To list its descendants, use `-R` short option.

> [!IMPORTANT]
> It will list its directly children for itself and its any entry in one line.
>
> To list one entry in one line, use `-1` short-option.

> [!IMPORTANT]
> It is allowed to combine many short-options into one short-option 
>
> For example,
>
> `ls -ltr` *l*ists by *t*ime from earliest to most recent

> [!NOTE]
> Classify symbol is used for classifying the entry
>
> When the entries is directory, it is `/`
>
> When the entries is executable, it is `*`

| short-option | description | acceptable arguments |
| :-- | :-- | :-- |
| `-l` | *l*ong format, list entries in long format (including permission, owner, group, file size, modified time, file name) | none |
| `-h` | *n*uman-readable format, the file size is represented with unit (such as `1KB`, `23GB`) | none |
| `-a` | *a*ll, list all entries including hidden entries (i.e. entries that start with `.`)   | none |
| `-A` | *a*lmost-all, list all entries except that current directory (i.e. `.`) and parent directory (i.e. `..`)  | none |
| `-r` | *r*eversely | none |
| `-t` | *t*ime, list by time descendantly (i.e. from most recent to earliest) | none |
| `-S` | file size, list by file size descendantly (i.e. from largest to smallest) | none |
| `-R` | *r*ecursive, it searches file recursively | none |
| `-d` | *d*irectory, it will ONLY search directories | `none |
| `-X` | e*x*tension, list by file extension descendantly in alphabetic order | `none |
| `-u` | access time, list by access time ascendantly (i.e. from earliest to most recent) | `none |
| `-1` | per *one* line, list one entry in one line | `none |
| `-i` | *i*node (index node), list index node for all entries | `none |
| `-F` | classify, append classify symbol at the end of entry name | `none |

| short-option | description | acceptable arguments |
| :-- | :-- | :-- |
| `-u -t` | lastest access time, list by access time descendantly (i.e. from most recent to earliest) | `none |

### `find`
`find` built-in command can find directories and folders of a directory with more filters compared with `ls`. 

> [!IMPORTANT]
> It will list itself and its descendants rather than its directly children.
>
> To list its directly children (exclusive itself), set `-mindepth` as `1`
>
> To list its descendants (exclusive itself), set `-mindepth` as `1` and `-maxdepth` as `1`

> [!TIP]
> `+`: more than
>
> `-`: less than

short-option about filters

| short-option | description | acceptable arguments |
| :-- | :-- | :-- |
| `-regex` | *regex*, the matching pattern with regex | any pattern |
| `-regextype` | *regex type*, which standard type of regex will be used | such as `posix-extended` |
| `-mindepth` | *min depth*, it will ONLY search for at least `n` level | `n` is a positive integer |
| `-maxdepth` | *max depth*, it will ONLY search for at most `n` level | `n` is a positive integer |
| `-name` | *name*, it will search by name in case-sensitive | file name |
| `-iname` | case-*i*nsensitive *name*, it will search by name in case-insensitive | file name |
| `-type` | *type*, it will search by type | `f` for files , `d` for directories, `l` for linkes |
| `-size` | *size*, it will search by size | ``+` or `-` followed by a string presenting file size |
| `-user` | *user*, it will search by owner | owner name |
| `-perm` | *perm*ission, it will search by permission | a three-digit octal number representing permission |
| `-mtime` | *m*odified *time*, it will search by modified time | `+` or `-` followed by a positive integer |

short-option about actions for all searched entries after searching

| short-option | description | acceptable value |  notes |
| :-- | :-- | :-- | :-- |
| `-print` | printf all searched entries path on screen one-by-one | arguments of `printf` | |
| `-delete` | delete all searched entries path one-by-one | none | |
| `-exec` | execute the commands for all searched entries path one-by-one | a string represents command for execution | |
| `-ls` | printf the detailed info in format as `ls -dils` | none | |

short-option about logical operation

| short-option | description | acceptable value | notes |
| :-- | :-- | :-- | :-- |
| `-and` (`-a`) | *and*, all conditions are satisfied | two conditions | its the default short-option |
| `-or` (`-o`) | *and*, one of conditions is satisfied | two conditions | |
| `-not` (`!`) | *not*, negate the condition | one condition | |

### `tree`
`tree` command can graph all directories and folders of current working directory

> [!IMPORTANT]
> It is not a built-in command,
>
> you have to install it, then use it

> [!IMPORTANT]
> If it is installed in Windows OS environment (with `winget` etc)
>
> then you have to execute `.exe` or `.bat` on your device (in Windows OS) using `cmd //c` (such as entering `cmd //c "tree"`)
>
> If it is installed in Linux environment,
>
> then you can simply execute the command (such as entering `tree)`.
>
> If it is installed in Windows OS environment but with .NET CLI
>
> then you can simply execute the command because the installed executable file or batch file is added in system environment variable `$PATH`

### Examples
#### Example 1
```list-directory-example-1.bash

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

function initialize(){
    echo "directory of current script:\`$SCRIPT_DIR\`"
    
    local base_directory="$SCRIPT_DIR/../../outputs/examples/list directories"
    export current_directory=""
    cd "$base_directory"
    current_directory="$PWD"

    # 使用 -v 將 Shell 的 current_directory 傳給 awk 的內部變數 dir_name
    find . -mindepth 1 -printf "%y %p\n" | awk -v dir_name="$current_directory" '
    {
        type = $1;
        $1 = ""; 
        if (type == "f") { files++; print "File:" $0 }
        else if (type == "d") { dirs++; print "Dir: " $0 }
    } 
    END { 
        printf "\nSummary of current directory ('%s'):\nFiles: %d\nDirectories: %d\nTotal: %d\n",
        dir_name, files, dirs, files+dirs 
    }'
}

main(){
    initialize
}

main
```

executing this script will echo all files, all directories, number of files, number of directories, total count of children under a directory. Format as follows

```
$ "D:\workspace\Bash\Bash tutorial\examples\directory\list-directory-example-1.bash"
directory of current script:`/d/workspace/Bash/Bash tutorial/examples/directory`
Dir:  ./markdown
File: ./markdown/README.md
File: ./markdown/tutorial.md
Dir:  ./shell script
File: ./shell script/version-example-2.bash
File: ./shell script/version-example-2.sh

Summary of current directory (/d/workspace/Bash/Bash tutorial/outputs/examples/list directories):
Files: 4
Directories: 2
Total: 6
```

#### Example 2
`tree-directory-example-1.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

function initialize(){
    echo "directory of current script:\`$SCRIPT_DIR\`"
    
    local base_directory="$SCRIPT_DIR/../../outputs/examples/list directories"
    export current_directory=""
    cd "$base_directory"
    current_directory="$PWD"

    # 直接用安裝好的tree指令
    ## tree 
    # 調用在Windows OS環境下所安裝的tree指令
    cmd //c "tree"
}

main(){
    initialize
}

main
```

executing this script will graph all directories and folders of current working directory

```
$ "D:\workspace\Bash\Bash tutorial\examples\directory\tree-directory-example-1.bash"
directory of current script:`/d/workspace/Bash/Bash tutorial/examples/directory`
Folder PATH listing for volume 新增磁碟區
Volume serial number is <volume-serial-number>
D:.
├─markdown
└─shell script

```

#### Example 3
`list-directory-example-3.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

function initialize(){
    echo "directory of current script:\`$SCRIPT_DIR\`"
    
    local base_directory="$SCRIPT_DIR/../../outputs/examples/list directories"
    export current_directory=""
    cd "$base_directory"
    current_directory="$PWD"

    ls -R | wc -l

    ls -R
}

main(){
    initialize
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\directory\list-directory-example-3.bash"
directory of current script:`/d/workspace/Bash/Bash tutorial/examples/directory`
11
.:
 markdown  'shell script'

./markdown:
README.md  tutorial.md

'./shell script':
version-example-2.bash  version-example-2.sh

```

## CH27-2 -- list specific directories and folders of a directory
### `ls`
`ls` built-in command will list directories and folders of a directory.

### `find`
`find` built-in command can find directories and folder of a directory.


### Examples
#### Example 1

`list-directory-example-2.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

function initialize(){
    echo "directory of current script:\`$SCRIPT_DIR\`"
    
 local base_directory="$SCRIPT_DIR/../../outputs/examples/list directories"
s"
 export current_directory=""
""
 cd "$base_directory"
y"
 current_directory="$PWD"

"

 # 使用 -v 將 Shell 的 current_directory 傳給 awk 的內部變數 dir_namectory="$PWD"

    # 使用 -v 將 Shell 的 current_directory 傳給 awk 的內部變數 dir_name
    find  . -mindepth 1 -regextype posix-extended -regex ".*\.(cs|md)" -printf "%y %p\n" | awk -v dir_name="$current_directory" '
    {
        type = $1;
        $1 = ""; 
        if (type == "f") { files++; print "File:" $0 }
        else if (type == "d") { dirs++; print "Dir: " $0 }
    } 
    END { 
        printf "\nSummary of current directory ('%s')\nFilters:\*.cs,\*.md\nFiles: %d\nDirectories: %d\nTotal: %d\n",
        dir_name, files, dirs, files+dirs 
    }'
}

main(){
    initialize
}

main
```

executing this script will echo all files and all directories that ends with `.cs`, `.md`, and number of files that ends with `.cs`, `.md`, number of directories that ends with `.cs`, `.md`, total count of children that ends with `.cs`, `.md` under a directory. Format as follows

```
$ "D:\workspace\Bash\Bash tutorial\examples\directory\list-directory-example-2.bash"
directory of current script:`/d/workspace/Bash/Bash tutorial/examples/directory`
awk: cmd. line:9: warning: escape sequence `\*' treSummary of current directory (/d/workspace/Bash/Bash tutorial/outputs/examples/list directories)
Filters:*.cs,*.md
Files: 2
Directories: 0
Total: 2

```

## CH27-3 -- create an empty directory
### `mkdir`
`mkdir` built-in command will create one or more empty directories

### Examples
#### Example 1
`create-directory-example-1.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

function initialize(){
    echo "directory of current script:\`$SCRIPT_DIR\`"
    
    local base_directory="$SCRIPT_DIR/../../outputs/examples/create directories"
    local current_directory=""
    local directories_name=$(echo directory{1..5})
    cd "$base_directory"
    current_directory="$PWD"

    mkdir $directories_name
}

main(){
    initialize
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\directory\create-directory-example-1.bash"
directory of current script:`/d/workspace/Bash/Bash tutorial/examples/directory`

```

create empty directories `directory1`, `directory2`,`directory3`,`directory4`,`directory5` under `/d/workspace/Bash/Bash tutorial/outputs/examples/directory/create directory`

## CH27-4 -- delete a directory
### `rmdir`
`rmdir` built-in command will delete one or more empty directories

> [!IMPORTANT]
> `rmdir` can ONLY delete empty directories.
>
> Trying to delete non-empty directories using `rmdir` will cause an error.

### Examples
#### Example 1

utility module

`print-directories-info-module.bash`

```
function print_directories_info(){

    local current_directory="$1"
    
    # 使用 -v 將 Shell 的 current_directory 傳給 awk 的內部變數 dir_name
    find . -mindepth 1 -printf "%y %p\n" | awk -v dir_name="$current_directory" '
    {
        type = $1;
        $1 = ""; 
        if (type == "f") { files++; print "File:" $0 }
        else if (type == "d") { dirs++; print "Dir: " $0 }
    } 
    END { 
        printf "\nSummary of current directory ('%s'):\nFiles: %d\nDirectories: %d\nTotal: %d\n",
        dir_name, files, dirs, files+dirs 
    }'
}

```

main script:

`delete-directory-example-1.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

source "$SCRIPT_DIR/../../utility modules/directory/print-directories-info-module.bash"

function initialize(){
    echo "directory of current script:\`$SCRIPT_DIR\`"
    
    local base_directory="$SCRIPT_DIR/../../outputs/examples/delete directories"
    local current_directory=""
    local directories_name=$(echo directory{1..5})
    cd "$base_directory"
    current_directory="$PWD"

    print_directories_info "$current_directory"

    echo "These directories \`$directories_name\` will be created"
    mkdir $directories_name
    echo "After creating empty directories \`$directories_name\`"
    print_directories_info "$current_directory"

    echo "These directories \`$directories_name\` will be deleted"
    rmdir $directories_name
    echo "After deleting empty directories \`$directories_name\`"
    print_directories_info "$current_directory"
}

main(){
    initialize
}

main
```

executing this main script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\directory\delete-directory-example-1.bash"
directory of current script:`/d/workspace/Bash/Bash tutorial/examples/directory`

Summary of current directory (/d/workspace/Bash/Bash tutorial/outputs/examples/delete directories):
Files: 0
Directories: 0
Total: 0
These directories `directory1 directory2 directory3 directory4 directory5` will be created
After creating empty directories `directory1 directory2 directory3 directory4 directory5`
Dir:  ./directory1
Dir:  ./directory2
Dir:  ./directory3
Dir:  ./directory4
Dir:  ./directory5

Summary of current directory (/d/workspace/Bash/Bash tutorial/outputs/examples/delete directories):
Files: 0
Directories: 5
Total: 5
These directories `directory1 directory2 directory3 directory4 directory5` will be deleted
After deleting empty directories `directory1 directory2 directory3 directory4 directory5`

Summary of current directory (/d/workspace/Bash/Bash tutorial/outputs/examples/delete directories):
Files: 0
Directories: 0
Total: 0

```

and creates empty directories `directory1` `directory2` `directory3` `directory4` `directory5` under `/d/workspace/Bash/Bash tutorial/outputs/examples/delete directories`

then deletes empty directories `directory1` `directory2` `directory3` `directory4` `directory5` under `/d/workspace/Bash/Bash tutorial/outputs/examples/delete directories`
```
```
