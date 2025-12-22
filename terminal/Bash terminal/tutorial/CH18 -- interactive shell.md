# CH18 -- interactive shell
## objectives
You will know how to

  + check it is executed in an interative shell

## CH18-1 -- check it is executed in an interative shell
### `$-`
The special variable `$-` contains `i` iff it is executed in an interactive shell

### `$PS1`
The special variable `$PS1` is set on startup script file iff it is executed in an interactive shell.

Otherwise, it is unset.

### Examples
#### Example 1
`interactive-shell-example-1.bash`

```
function print_shell_info(){
    case "$-" in
    *i*)	echo This shell is interactive ;;
    *)	echo This shell is not interactive ;;
    esac
}

main(){
    print_shell_info
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\interactive shell\interactive-shell-example-1.bash"
This shell is not interactive

```

#### Example 2
`interactive-shell-example-2.bash`

```
function print_shell_info(){
    if [ -z "$PS1" ]; then
            echo This shell is not interactive
    else
            echo This shell is interactive
    fi
}

main(){
    print_shell_info
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\interactive shell\interactive-shell-example-2.bash"
This shell is not interactive

```
