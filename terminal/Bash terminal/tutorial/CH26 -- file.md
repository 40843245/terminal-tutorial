# CH26 -- file
## objectives
You will know how to

  + acecss content of a file
  + create a file
  + delete a file
  + copy a file
  + move a file

## CH26-1 -- acecss content of a file
### `cat`
`cat` built-in command in Linux will echo content of a file.

### Examples
#### Example 1
`bash-example-5.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

main(){
    local example_script_path="$SCRIPT_DIR/multi-language-example-1.bash"
    
    echo "The example \`$example_script_path\` content:"
    cat "$example_script_path"
    echo " "
    bash -D "$example_script_path"
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\subshell\bash\bash-example-5.bash"
The example `/d/workspace/Bash/Bash tutorial/examples/subshell/bash/multi-language-example-1.bash` content:
main(){
    echo $"Hello, World!"
    echo $"Good morning, user."
    echo "This will not be extracted." # 一般雙引號不會被提取
}

main
"Hello, World!"
"Good morning, user."

```

## CH26-2 -- create a file
### `touch`
`touch` built-in command in Linux will create an empty file with given file name.

### Examples
#### Example 1
`file-example-1.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

function initialize(){
    echo "directory of current script:\`$SCRIPT_DIR\`"
    
    local base_directory="$SCRIPT_DIR/../../outputs/examples/create files"
    local files1_name=$(echo file{1..5}.txt)
    cd "$base_directory"
    touch $files1_name
}

main(){
    initialize
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\file\file-example-1.bash"
directory of current script:`/d/workspace/Bash/Bash tutorial/examples/file`

```

and create empty files `file1.txt` `file2.txt` `file3.txt` `file4.txt` `file5.txt`

under directory `/d/workspace/Bash/Bash tutorial/outputs/examples/create files`

## CH26-3 -- delete a file
### `rm`
`rm` built-in command in Linux will remove a file with given specific file name.

Additionally, `rm` built-in command with options can be used to remove a directory.

### Examples
#### Example 1
`file-example-2.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

function initialize(){
    echo "directory of current script:\`$SCRIPT_DIR\`"
    
    local base_directory="$SCRIPT_DIR/../../outputs/examples/create files"
    local files1_name=$(echo file{1..5}.txt)
    cd "$base_directory"
    touch $files1_name
    rm $files1_name
}

main(){
    initialize
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\file\file-example-1.bash"
directory of current script:`/d/workspace/Bash/Bash tutorial/examples/file`

```

and create empty files `file1.txt` `file2.txt` `file3.txt` `file4.txt` `file5.txt`

under directory `/d/workspace/Bash/Bash tutorial/outputs/examples/create files`

then delete files `file1.txt` `file2.txt` `file3.txt` `file4.txt` `file5.txt`

