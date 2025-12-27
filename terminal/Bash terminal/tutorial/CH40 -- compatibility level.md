# CH40 -- compatibility level
## objectives
You will learn how to

  + set compatibility level

## CH40-1 -- compatibility level
To set compatibility level, you can use `shopt -s` with `{argument}`

where

`{argument}` starts with `compat` followed by release number then minor number (without seperator)

You can also set `BASH_COMPAT` system environment variable.

> [!NOTE]
> When the compatibility level is NOT changed, `BASH_COMPAT` uses default value empty string ``.
>
> When you set `BASH_COMPAT` to empty string ``, it uses default mode.

> [!NOTE]
> If you want to look at Bash version, you have to access the array `BASH_VERSION`

### Examples
#### Example 1
`set-compatibility-level-example-1.bash`

```
main(){
    local comptability_level_at_present="$BASH_COMPAT"
    
    printf "compatibility level at present:\`%s\`\n" "$comptability_level_at_present"
    printf "Ready to change compatibility level version 4.2\n"
    
    shopt -s compat42

    local comptability_level_4_2="$BASH_COMPAT"
    printf "After changing compatibility level version 4.2\n"
    printf "Now, compatibility level is changed to:\`%s\`\n" "$comptability_level_4_2"

    printf "Now, ready to restore compatibility level:\`%s\`\n" "$comptability_level_at_present"

    BASH_COMPAT="$comptability_level_at_present"

    printf "After restoring compatibility level:\`%s\`\n" "$comptability_level_at_present"
    printf "Now, compatibility level is changed to:\`%s\`\n" "$BASH_COMPAT"
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\compatibility level\set-compatibility-level-example-1.bash"
compatibility level at present:``
Ready to change compatibility level version 4.2
After changing compatibility level version 4.2
Now, compatibility level is changed to:`42`
Now, ready to restore compatibility level:``
After restoring compatibility level:``
Now, compatibility level is changed to:``

```
