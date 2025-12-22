# CH27 -- directory
## objectives
You will know how to

  + list all directories and folders of a directory
  + list specific directories and folders of a directory
  + delete a directory
  + copy a directory
  + move a directory
  + rename a directory

## CH27-1 -- list all directories and folders of a directory
### `find`
`find` built-in command can find directories and folder of a directory. 

### Examples
#### Example 1

`list-directory-example-1.bash`

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

## CH27-2 -- list all directories and folders of a directory
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
    export current_directory=""
    cd "$base_directory"
    current_directory="$PWD"

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
awk: cmd. line:9: warning: escape sequence `\*' treated as plain `*'
File: ./markdown/README.md
File: ./markdown/tutorial.md

Summary of current directory (/d/workspace/Bash/Bash tutorial/outputs/examples/list directories)
Filters:*.cs,*.md
Files: 2
Directories: 0
Total: 2

```
