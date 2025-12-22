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

executing this script will echo all files, all directories, number of files, number of directories, total count of children under a directory 
