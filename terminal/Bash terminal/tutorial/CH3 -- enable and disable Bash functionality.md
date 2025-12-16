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
