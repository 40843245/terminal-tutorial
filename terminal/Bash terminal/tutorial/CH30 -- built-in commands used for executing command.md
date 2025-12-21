# CH30 -- built-in commands used for executing command
## objectives
You will know 

  + built-in commands used for executing command

## CH30-1 -- built-in commands used for executing command
### `:`
`:`: just expanding arguments (see CH11) and then performing redirection (see CH24)

syntax:

```
: {arguments}
```

### `.`

syntax:

```
. (-p {path})? {file-name} {arguments}?
```

`.` command reads and then execute {file-name} with `{arguments}` in the current shell context.

> [!NOTE]
> If `-p` is supplied, `.` treats `{path}` as a colon-separated list of directories in which to find {file-name}
>
> Otherwise, `.` uses the directories in `$PATH` to find {file-name}.

> [!NOTE]
>  If the `sourcepath` option is turned off, `.` does not search `$PATH`.

> [!NOTE]
> `{file-name}` does not need to be executable.

> [!NOTE]
> If any arguments are supplied, they become the positional parameters iff `{file-name}` is executed. Otherwise the positional parameters are unchanged.

### `bash`
You can execute an inline subshell script using `bash -c`.

> [!NOTE]
> `bash -c` opens a subshell, and consider the arguments as commands then executes commands
>
> it is executed at sandbox,
> 
> thus it may NOT pollute user-defined variables that exported on shell

`bash -i` built-in command changes it in an interactive shell

`bash --posix` will execute a shell using POSIX standard.

#### Examples
##### Example 1

`bash-example-4.bash`

```
main(){
    echo "executing \`bash --posix\` will make Bash engine applies the POSIX standard. However, it may disable some extended functionalities that based on Bourne"
    bash --posix
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\subshell\bash\bash-example-4.bash"
executing `bash --posix` will make Bash engine applies the POSIX standard. However, it may disable some extended functionalities that based on Bourne
bash-5.2$

```
