# CH3 -- enable and disable Bash functionality
## objectives
You will know how to

  + enable and disable functionality
  + look at all functionalites are turned on or off

## CH3-1 -- enable and disable Bash functionality
### set
The built-in command `set` can enable or disable Bash functionalities.

syntax:

```
set {one-symbol-for-enabling-or-disabling}{short-option-for-functionality} {other-argumments}*
```

where

`{one-symbol-for-enabling-or-disabling}` can be one of `-` or `+`

| `{one-symbol-for-enabling-or-disabling}` | enable or disable |
| :-- | :-- |
| `-` | enable |
| `+` | disable |

and

`{short-option-for-functionality}` is a letter (usually is abbreviated from a word) that determines which functionality will be enabled or disabled

| `{short-option-for-functionality}` | functionality |
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
> assign the result (after brace expansion) into a variable using `$()`.
>
> For example,
>
> changing above example into
>
> ```
> local local_expanded_string=$(echo {1..5})
> ```

> [!IMPORTANT]
> Note that the preceedence of expansion (see CH19 for more details).
>
> Since brace expansion takes preceedence of variable expansion,
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
### set -o
The built-in command `set` with short option `-o` (`set -o`)

will list all functionalies active status (it is enabled or not)

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

