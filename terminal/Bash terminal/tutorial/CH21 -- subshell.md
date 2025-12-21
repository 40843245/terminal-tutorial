# CH21 -- subshell
## objectives
You will know

  + subshell
  + how to execute a subshell script
  + command grouping, restricting the scope of subshell script

## CH21 -- introduction to subshell
> [!TIP]
> The prefix `-sub` means under of something

A subshell script is a script inside a shell script.

## CH21-2 -- execute a subshell script
### inline subshell script
#### `bash`
You can execute an inline subshell script using `bash` built-in command with `-c` short option.

> [!NOTE]
> `bash -c` opens a subshell, and consider the arguments as commands then executes commands
>
> it is executed at sandbox,
> 
> thus it may NOT pollute user-defined variables that exported on shell

### Examples
#### Example 1
In interactive shell,

```
<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/outputs/permissions
$ bash -c "export GLOBAL_VAR=3"

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/outputs/permissions
$ bash -c "echo 'Hello World'; GLOBAL_VAR=4"
Hello World

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/outputs/permissions
$ bash -c "echo '$GLOBAL_VAR:\`$GLOBAL_VAR\`'"
:``

```

#### Example 2

`bash-example-1.bash`

```
main(){
    bash -c "export GLOBAL_VAR=3"
    bash -c "echo 'Hello World'; GLOBAL_VAR=4"
    bash -c "echo '$GLOBAL_VAR:\`$GLOBAL_VAR\`'"
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\subshell\bash-example-1.bash"
Hello World
:``
```

## CH21-3 -- command grouping
commands inside `()`, which uses command grouping technique, will form a subshell script and is executed in a clean environment, 

and thus, it can't access environment variables that exported on shell script.

And it will NOT pollute environment variables that exported on shell script

For example, 

One defines a global variable (called `varA`) inside `()` (which is considered as a subshell script).

All variables defined inside `()` (including `varA`) will **be unset after executing commands of `()`**

Thus, after executing commands in `()`, when accessing variable `varA`, the Bash will NOT find `varA` that is defined inside `()`.

On the other hand,

One defines a global variable (called `varA`) inside a subshell script but NOT wrapped with `()`.

All variables defined inside a subshell (including `varA`) will **NOT be unset after executing commands in the subshell script**.

Thus, after executing commands in the subshell script, when accessing variable `varA`, the Bash will try to find `varA` that is defined inside the subshell script.

Compare following examples for fully understanding.

## CH21-4 -- special variable about shell

`$$`: Expands to the process ID of the shell. 

      In a subshell, it expands to the process ID of the invoking shell, not the subshell.

`$!`: Expands to the process ID of the job most recently placed into the background, 

      whether executed as an asynchronous command or using the bg builtin.
      
## Examples
### Example 1
This example illustrates how a variable that is exported is polluted 

`command-grouping-example-1.bash`

```
# 父 Shell
echo "Parent Shell: Before: \`$TEMP_VAR\`" 

## 沒有使用command grouping技術--`()`來包裝 commands
## 所以，這個用export保留詞的變數`TEMP_VAR`被汙染了。
export TEMP_VAR="Temporary Value" ; echo "Subshell: Inside: \`$TEMP_VAR\`"

# 回到父 Shell
echo "Parent Shell: After: \`$TEMP_VAR\`"
```

executing this script will echo

```
Parent Shell: Before: ``
Subshell: Inside: `Temporary Value`
Parent Shell: After: `Temporary Value`
```

### Example 2
This example illustrates how prevent variable that is exported to be polluted using command grouping

`command-grouping-example-2.bash`

```
# 父 Shell
echo "Parent Shell: Before: \`$TEMP_VAR\`" 

## 有使用command grouping技術--`()`來包裝 commands
## 所以，這個用export保留詞的變數`TEMP_VAR`沒有被汙染了。
( export TEMP_VAR="Temporary Value" ; echo "Subshell: Inside: \`$TEMP_VAR\`" )

# 回到父 Shell
echo "Parent Shell: After: \`$TEMP_VAR\`"
```
executing this script will echo

```
Parent Shell: Before: ``
Subshell: Inside: `Temporary Value`
Parent Shell: After: ``

```


