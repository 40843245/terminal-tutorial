# CH7 -- comparison
## objectives
You will know how to 

  + compare two variables
  + check a variable is set, unset
  + check a variable is in specific type

## CH7-1 -- compare two integers
> [!NOTE]
> You can't compare two numbers using symbol (such as `!=`) inside `[[]]`.
>
> You have to use flag (like in IL) to do so.

> [!NOTE]
> You can compare two numbers using symbol (such as `!=`) inside `(())` C-style arithmetic spread operator.

<details>
<summary>
inside `[[]]`
</summary>
  
| flag | description |
| :-- | :-- |
| `-eq` | equal to |
| `-ne` | not equal to |
| `lt` | less than |
| `gt` | greater than |
| `le` | less than or equal to |
| `ge` | greater than or equal to |

</details>


<details>
<summary>
inside `(())` C-style arithmetic spread operator.
</summary>
  
| operator | description |
| :-- | :-- |
| `==` | equal to |
| `!=` | not equal to |
| `<` | less than |
| `>` | greater than |
| `<=` | less than or equal to |
| `>=` | greater than or equal to |

</details>

### Examples
#### Example 1
To check status code `STATUS_CODE` is equal to 0.

```
STATUS_CODE=0
```

You can write

```
if [[ $STATUS_CODE -eq 0 ]];
```

or equivalently

```
if ((STATUS_CODE == 0)); 
```

## CH7-2 -- compare two strings
Inside `[[]]`, we can use these
  
| operator or options | description |
| :-- | :-- |
| `==` | equal to |
| `!=` | not equal to |
| `` | non-zero length |
| `-n` | non-zero length |
| `-z` | zero-length |

</details>

see [bash的string testing](https://docs.google.com/spreadsheets/d/1_lipmlEwKus0CVZgdS93Zda5gASe4g2bqrd4JsmQZM4/edit?gid=1895558629#gid=1895558629)

## CH7-3 -- check a variable is set or unset
Inside `[[]]`, we can use `-v` followed by a variable name to check the variable exists in memory (and thus, also check the variable is unset (exclusive to empty string)

### Examples
#### Example 1

`check-a-variable-exists-example-1.bash`

```
STR1="string1"

function print_info(){
    local -n _ref=$1
    if [[ -v _ref ]]; then 
        echo "The variable named \`"$1"\` does NOT exist in memory. It is unset"
    else
        echo "The variable named \`"$1"\` exists in memory. It is set."
    fi
}

main(){
    local str1="string1"
    local str2="string2"
    unset str2

    print_info str1
    print_info STR1
    print_info STR2
    print_info str2
    print_info str3
}

STR2="string2"

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\comparison\check-a-variable-exists-example-1.bash"
The variable named `str1` does NOT exist in memory. It is unset
The variable named `STR1` does NOT exist in memory. It is unset
The variable named `STR2` does NOT exist in memory. It is unset
The variable named `str2` exists in memory. It is set.
The variable named `str3` exists in memory. It is set.

```


## CH7-4 -- check a file
see [bash的File Testing](https://docs.google.com/spreadsheets/d/1sSjh6nQffVeyuqQo4JFjFje6DZ7LzkrtOl_s688bUEM/edit?gid=1954033718#gid=1954033718)

and [CH28](https://github.com/40843245/terminal-tutorial/blob/main/terminal/Bash%20terminal/tutorial/CH28%20--%20existence%20of%20a%20file%20or%20directory_TOC.md) for more details
