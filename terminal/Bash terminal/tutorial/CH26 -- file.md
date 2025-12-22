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

## CH26-3 -- copy a file
### `cp`
`cp` built-in command in Linux will copy a file from `old-directory` to `new-directory`.

syntax:

```
cd {file-name} {new-directory}
```

will copy the file named `{file-name}` from directory of `{file-name}` to new directory `{new-directory}`

or

```
cd {file-name} {new-file-name}
```

will copy the file named `{file-name}` from directory of `{file-name}` to same directory but with name `{new-file-name}`


or

```
cd {file-name} "{new-directory}/{new-file-name}"
```

will copy the file named `{file-name}` from directory of `{file-name}` to new directory `{new-directory}` and with name `{new-file-name}`

### Examples
#### Example 1
`file-example-3.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

function initialize(){
    echo "directory of current script:\`$SCRIPT_DIR\`"
    
    local old_directory="$SCRIPT_DIR/../../outputs/examples/copy files/old directory"
    local new_directory="$SCRIPT_DIR/../../outputs/examples/copy files/new directory"
    local file1_name="file1.txt"
    cd "$old_directory"
    touch $file1_name
    cp $file1_name "$new_directory"
}

main(){
    initialize
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\file\file-example-3.bash"
directory of current script:`/d/workspace/Bash/Bash tutorial/examples/file`

```

and create empty files `file1.txt`

under directory `/d/workspace/Bash/Bash tutorial/outputs/examples/create files`

then copy `file1.txt` from old directory `/d/workspace/Bash/Bash tutorial/outputs/examples/create files/old directory` to new directory `/d/workspace/Bash/Bash tutorial/outputs/examples/create files/new directory`

#### Example 2
`file-example-4.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

function initialize(){
    echo "directory of current script:\`$SCRIPT_DIR\`"
    
    local old_directory="$SCRIPT_DIR/../../outputs/examples/copy files/old directory"
    local new_directory="$SCRIPT_DIR/../../outputs/examples/copy files/new directory"
    local file1_name="file1.txt"
    local file2_name="file2.txt"
    cd "$old_directory"
    touch $file1_name
    cp $file1_name $file2_name
}

main(){
    initialize
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\file\file-example-4.bash"
directory of current script:`/d/workspace/Bash/Bash tutorial/examples/file`

```

and create empty files `file1.txt`

under directory `/d/workspace/Bash/Bash tutorial/outputs/examples/create files`

then copy `file1.txt` from old directory `/d/workspace/Bash/Bash tutorial/outputs/examples/create files/old directory` to same new directory `/d/workspace/Bash/Bash tutorial/outputs/examples/create files/old directory` but named `file2.txt`

#### Example 3
`file-example-5.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

function initialize(){
    echo "directory of current script:\`$SCRIPT_DIR\`"
    
    local old_directory="$SCRIPT_DIR/../../outputs/examples/copy files/old directory"
    local new_directory="$SCRIPT_DIR/../../outputs/examples/copy files/new directory"
    local file1_name="file1.txt"
    local file2_name="file2.txt"
    cd "$old_directory"
    touch $file1_name
    cp $file1_name "$new_directory/$file2_name"
}

main(){
    initialize
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\file\file-example-5.bash"
directory of current script:`/d/workspace/Bash/Bash tutorial/examples/file`

```

and create empty files `file1.txt`

under directory `/d/workspace/Bash/Bash tutorial/outputs/examples/create files`

then copy `file1.txt` from old directory `/d/workspace/Bash/Bash tutorial/outputs/examples/create files/old directory` to new directory `/d/workspace/Bash/Bash tutorial/outputs/examples/create files/new directory` but named `file2.txt`

## CH26-4 -- move a file
### `mv`
`mv` built-in command in Linux will move a file from `old-directory` to `new-directory` and it can rename a file.

> [!TIP]
> Thinking that
>
> renaming a file is considered as moving a file under same directory but with different name.

syntax:

```
mv {file-name} {new-directory}
```

will move the file named `{file-name}` from directory of `{file-name}` to new directory `{new-directory}` and named `{file-name}`

syntax:

```
mv {file-name} "{new-directory}/{new-file-name}"
```

will move the file named `{file-name}` from directory of `{file-name}` to new directory `{new-directory}` and named `{new-file-name}`

### Examples
#### Example 1
`file-example-6.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

function initialize(){
    echo "directory of current script:\`$SCRIPT_DIR\`"
    
    local old_directory="$SCRIPT_DIR/../../outputs/examples/move files/old directory"
    local new_directory="$SCRIPT_DIR/../../outputs/examples/move files/new directory"
    local file1_name="file1.txt"
    local file2_name="file2.txt"
    cd "$old_directory"
    touch $file1_name
    mv $file1_name "$new_directory"
}

main(){
    initialize
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\file\file-example-6.bash"
directory of current script:`/d/workspace/Bash/Bash tutorial/examples/file`

```

and create empty files `file1.txt`

under directory `/d/workspace/Bash/Bash tutorial/outputs/examples/move files`

then copy `file1.txt` from old directory `/d/workspace/Bash/Bash tutorial/outputs/examples/move files/old directory` to new directory `/d/workspace/Bash/Bash tutorial/outputs/examples/move files/new directory`

## CH26-5 -- rename a file
### `mv`
`mv` built-in command in Linux will move a file from `old-directory` to `new-directory` and it can rename a file.

syntax:

```
mv {file-name} {new-file-name}
```

will move the file named `{file-name}` from directory of `{file-name}` to same directory but named `{new-file-name}`.
