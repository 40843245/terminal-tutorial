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

## CH7-4 -- get type of a variable
### `declare -p`
`declare -p` followed by a variable name will echo the structure (definition, variable name, variable value) of a variable.

Thus, you can stream the data from stdout (stands for standard output stream) to `/dev/null` and assigns it to a variable.

Then you can parse it with regex (see CH14 for more details) or using `grep` (a built-in command in Unix/Linux environment)

### Examples
#### Example 1
`typeof-a-variable-example-1.bash`

```
function print_info(){
    print_info1 "$1"
    print_info2 "$1"
}

function print_info1(){
    local info
    info=$(declare -p "$1" 2>/dev/null)
    
    if [[ -z "$info" ]]; then
        echo "Variable '$1' is undefined."
    elif [[ "$info" =~ "declare -a" ]]; then
        echo "$1 is an Indexed Array."
    elif [[ "$info" =~ "declare -A" ]]; then
        echo "$1 is an Associative Array."
    elif [[ "$info" =~ "declare -i" ]]; then
        echo "$1 is an Integer."
    else
        echo "$1 is a String (default)."
    fi
}

function print_info2(){
    local info
    info=$(declare -p "$1" 2>/dev/null)
    
    if [[ -z "$info" ]]; then
        echo "Variable '$1' is undefined."
    elif grep -q "declare -a" <<< "$info"; then
        echo "$1 is an Indexed Array."
    elif grep -q "declare -A" <<< "$info"; then
        echo "$1 is an Associative Array."
    elif grep -q "declare -i" <<< "$info"; then
        echo "$1 is an Integer."
    else
        echo "$1 is a String (default)."
    fi
}

main(){
    local str1="string1"
    declare -i integer1=1
    declare -a indexed_array1=(1 2 3)
    declare -A associative_array1=(["0"]="value0")
    print_info str1 
    print_info integer1
    print_info indexed_array1
    print_info associative_array1
    print_info unset_variable1
}

main 
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\declare\typeof-a-variable-example-1.bash"
str1 is a String (default).
str1 is a String (default).
integer1 is an Integer.
integer1 is an Integer.
indexed_array1 is an Indexed Array.
indexed_array1 is an Indexed Array.
associative_array1 is an Associative Array.
associative_array1 is an Associative Array.
Variable 'unset_variable1' is undefined.
Variable 'unset_variable1' is undefined.

```

## CH7-5 -- check a file
see [bash的File Testing](https://docs.google.com/spreadsheets/d/1sSjh6nQffVeyuqQo4JFjFje6DZ7LzkrtOl_s688bUEM/edit?gid=1954033718#gid=1954033718)

and [CH28](https://github.com/40843245/terminal-tutorial/blob/main/terminal/Bash%20terminal/tutorial/CH28%20--%20existence%20of%20a%20file%20or%20directory_TOC.md) for more details
