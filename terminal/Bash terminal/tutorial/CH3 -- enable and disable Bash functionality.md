# CH3 -- enable and disable Bash functionality
## objectives
You will know how to

  + enable and disable functionality
  + look at all functionalites are turned on or off
  + look at one functionality is turned on or off

## CH3-1 -- enable and disable Bash functionality
### set
The built-in command `set` can enable or disable Bash functionalities.

syntax:

```
set {one-symbol-for-enabling-or-disabling}{shorthand-of-functionality} {other-argumments}*
```

where

`{one-symbol-for-enabling-or-disabling}` can be one of `-` or `+`

| `{one-symbol-for-enabling-or-disabling}` | enable or disable |
| :-- | :-- |
| `-` | enable |
| `+` | disable |

and

`{shorthand-of-functionality}` is a letter (usually is abbreviated from a word) that determines which functionality will be enabled or disabled

| `{shorthand-of-functionality}` | functionality |
| :-- | :-- |
| `B` | Bash commands (which is extended based on shell) |
| `e` | termination on error |
| `u` | report an error when accessing a variable in unset state |
| `x` | echo expanded string before executing script |
| `C` | report an error when overwriting an existing file |

> [!IMPORTANT]
> Directly assignment into a variable will not perform brace expansion
>
> for example,
>
> ```
> local local_expanded_string={1..5}
> ```
>
> will NOT perform brace expansion on `{1..5}`.
>
> It will consider `{1..5}` as a string then store it in `local_expanded_string` variable.
>
> To correctly assign a value after brace expansion into a variable,
>
> you will need to `echo` the expression to perform brace expansion then
>
> assign the result (after brace expansion) into a variable using `$()` (command substitution technique, see CH11 for more details).
>
> For example,
>
> changing above example into
>
> ```
> local local_expanded_string=$(echo {1..5})
> ```

> [!IMPORTANT]
> Note that the precedence of expansion (see CH11 for more details).
>
> Since brace expansion takes precedence of variable expansion,
>
> in an expression, when accessing a variable, it will expand the variable to its value and
>
> after that, it will not expand brace (if it contains)
>
> for example,
>
> ```
> local local_expanded_string={1..5}
> echo "\`{1..5}\` will be \`$local_expanded_string\`"
> ```
>
> executing this script will always echo
>
> ```
> `{1..5}` will be `{1..5}`
> ```

### Examples
#### Example 1
This example illustrates the behavior when a functionality is enabled and it is disabled respectively

`enable-or-disable-functionality-example-1.bash`

```
main(){
    local local_expanded_string
    echo "--------- When Bash commands are enabled (by default) ---------"
    local_expanded_string=$(echo {1..5})
    echo "\`{1..5}\` will be \`$local_expanded_string\`"
    
    # Disable Bash commands (which there doesn't exist on shell (using `sh` command)
    set +B

    echo "--------- After Bash commands are disabled (by executing \`set +B\`) ---------"
    local_expanded_string=$(echo {1..5})
    echo "\`{1..5}\` will be \`$local_expanded_string\`"

    # Enable Bash commands (which there doesn't exist on shell (using `sh` command)
    set -B

    echo "--------- When Bash commands are enabled (by executing \`set -B\`) ---------"
    local_expanded_string=$(echo {1..5})
    echo "\`{1..5}\` will be \`$local_expanded_string\`"
}

main
```

executing this script will echo

```
--------- When Bash commands are enabled (by default) ---------
`{1..5}` will be `1 2 3 4 5`
--------- After Bash commands are disabled (by executing `set +B`) ---------
`{1..5}` will be `{1..5}`
--------- When Bash commands are enabled (by executing `set -B`) ---------
`{1..5}` will be `1 2 3 4 5`

```

## CH3-2 -- look at all functionalites are turned on or off
### `set -o`
The built-in command `set` with short option `-o` (`set -o`)

will list all functionalies active status (whether it is enabled or not)

### `echo $-`
`$-` is a special variable that stores all enabled functionalities (represented as a string, each character is a shorthand of functionality) (for example, see `{shorthand-of-functionality}` table)

### Examples
#### Example 1
This examples list all functionalities active status.

`list-all-functionalities-active-status-example-1.bash`

```
main(){
    set -o
}

main
```

executing this script will echo

```
allexport       off
braceexpand     on
emacs           off
errexit         off
errtrace        off
functrace       off
hashall         on
histexpand      off
history         off
igncr           off
ignoreeof       off
interactive-comments    on
keyword         off
monitor         off
noclobber       off
noexec          off
noglob          off
nolog           off
notify          off
nounset         off
onecmd          off
physical        off
pipefail        off
posix           off
privileged      off
verbose         off
vi              off
xtrace          off

```

#### Example 2
This example list all enabled functionalities (represented as a string where each character indicates shorthand of functionality) 

`list-all-aliased-enabled-functionalities-example-1.bash`

```
main(){
    echo $-
}

main
```

executing this script will echo

```
hB

```

indicating that 

there is currently two functionalities are enabled in this shell 

+ `h` (shorthand of `hashall`)
+ `B` (shorthand of `braceexpand`)

## CH3-3 -- look at one functionality is turned on or off
### `shopt -p -o`
Though in POSIX, `set -o` can list active status of all functionalities (and you can filter it)

there is a more convenience command to do that

```
shopt -p -o {functionality-name}
```

where

`{functionality-name}` is the functionality name (for example `braceexpand`)

#### Examples
##### Example 1
> [!NOTE]
> Although it is not cleaner than using other approaches (`shopt -p -o {functionality-name}` and `set -o | grep {functionality-name}`)
>
> it is easier and more flexible (as the condition consists of wildcards) to check the active status of one functionality (rather than simply looking at it),
>
> especially when it is needed to check the active status of lots functionalities.
>
> It is useful for executing different commands according to the active status of one or more functionalities.  

`list-one-functionality-active-status-example-1.bash`

```
main(){
    shopt -p -o braceexpand
}

main
```

executing this script will echo, by default,

```
set -o braceexpand
```

there is a minus `-` before `o`, 

thus it indicates `braceexpand` functionality is currently enabled in this shell.

> [!TIP]
> minus `-`: enabled
>
> plus `+`: disabled
>
> see the above `{one-symbol-for-enabling-or-disabling}` table for more details.

### grep
You can also use `grep` to filter it by functionality name

> [!NOTE]
> `grep` is a built-in command in POSIX used for filtering by conditions

#### Examples
##### Example 1
`list-one-functionality-active-status-example-2.bash`

```
main(){
    set -o | grep braceexpand
}

main
```

executing this script will echo, by default,

```
braceexpand     on
```

which indicates the `braceexpand` functionality is currently enabled in this shell.

### check with `$-`
Thanks to special variable `$-` (see CH3-2 for more details),

We can check the value of `$-` contains a shorthand of functionality name using wildcard `*`.

see following example 1 for utility function and its usage.

#### Examples
##### Example 1

`list-one-functionality-active-status-example-3.bash`

```
function is_functionality_enabled(){
    local shorthand_of_functionality_name="$1"
    local matching_pattern="*${shorthand_of_functionality_name}*"
    if [[ $- == $matching_pattern ]]; then 
        return 0
    else
        return 1
    fi
}

function print_active_status(){
    local functionality_name="$1"
    declare -i is_active=$2

    if [[ $is_active == 0 ]]; then
        echo "\`$functionality_name\` is currently enabled in this shell"
    else
        echo "\`$functionality_name\` is currently disabled in this shell"
    fi
}

main(){
    declare -i is_active=0

    set -o | grep braceexpand

    is_functionality_enabled B
    is_active=$?
    print_active_status "braceexpand" $is_active

    set -o | grep hashall

    is_functionality_enabled h
    is_active=$?
    print_active_status "hashall" $is_active
}

main
```

executing this script will echo, by default,

```
braceexpand     on
`braceexpand` is currently enabled in this shell
hashall         on
`hashall` is currently enabled in this shell

```
