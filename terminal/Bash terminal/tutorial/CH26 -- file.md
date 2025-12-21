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

## CH26-2 -- create a file
### `touch`
`touch` built-in command in Linux will create an empty file with given file name.

## CH26-3 -- delete a file
### `rm`
`rm` built-in command in Linux will remove a file with given specific file name.

Additionally, `rm` built-in command with options can be used to remove a directory.



