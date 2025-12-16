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

## CH3-2 -- look at all functionalites are turned on or off
### set -o
The built-in command `set` with short option `-o` (`set -o`)

will list all functionalies active status (it is enabled or not)
