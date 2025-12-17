# CH3 -- enable and disable Bash functionality
## objectives
You will know how to

  + enable and disable functionality
  + look at all functionalites are turned on or off
  + look at one functionality is turned on or off

extra bonus

  + look at many functionalities are BOTH enabed 
  + look at at least one of functionalities is enabled


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

utility module:

`check-one-functionality-is-enabled-module.bash`

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
```

main script:

`check-one-functionality-is-enabled-example-1.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

# Source the module using an absolute path derived from the script location
source "$SCRIPT_DIR/../../utility modules/functionality enabled/check-one-functionality-is-enabled-module.bash"

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

executing main script will echo, by default,

```
braceexpand     on
`braceexpand` is currently enabled in this shell
hashall         on
`hashall` is currently enabled in this shell

```

## CH3-4 -- look at many functionalities are BOTH enabed 
### Examples
#### Example 1

utility modules:

`check-many-functionalities-both-enabled-module.bash`

```
function is_all_functionalities_enabled(){
    local query="$1"
    # 遍歷輸入的每一個字元
    for (( i=0; i<${#query}; i++ )); do
        local char="${query:$i:1}"
        if [[ $- != *"$char"* ]]; then 
            return 1 # 只要有一個沒找到，就回傳失敗
        fi
    done
    return 0 # 全部都找到了
}
```

`joining-an-array-module.bash`

```
# !/bin/bash

### 模組名稱: joinning-an-array-module.bash

## 主要函式
## 目的:
## 串接參數至array。
## 串接這種多個參數`"apple" "orange"`至array。
## 
## 詳細實作細節:
## 將第零個引數當成seperator來join第一個引數至最後一個引數成以下形式
##
## $2c$1c$3
## 
## where `c` is a character, `$2`為第一個引數
##
## 注意事項:
## + 第零個引數當成seperator必須為一個字元。
## + 以zero-based index為準。
## 
## 回傳值:
## return 退出代碼代表成功地串接array。
## echo 出串接完的字串，當失敗地串接array時，將echo ""
##
## Case 1:當引數值不合法時
## return(失敗)退出代碼。並echo空字串
##
## Case 2:當引數值不合法時
## return(成功)退出代碼。並echo串接完後的字串
function join_an_array(){
    local seperator="$1"
    declare -i seperator_length=${#seperator}
    if [[ $seperator_length != 1 ]]; then
        # when the seperator is NOT a character, 
        # return exit code with failure
        # echo empty string as failure
        echo ""
        return 1 
    fi
    local args=("${@:2}")

    # core of logic 
    # 保存原本的`$IFS`
    local old_ifs=$IFS

    # 將其暫時設為目標分隔符
    IFS=$seperator

    # 將陣列元素展開為單一字串
    local joined_str="${args[*]}"
    
    # 還原 `$IFS`
    IFS=$old_ifs

    # return exit code with success
    # echo string after joined (also with success)
    echo "$joined_str"
    return 0
}

```

main script:

`check-many-functionalities-both-enabled-example-1.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

# Source the module using an absolute path derived from the script location
source "$SCRIPT_DIR/../../utility modules/functionality enabled/check-many-functionalities-both-enabled-module.bash"

source "$SCRIPT_DIR/../../utility modules/joining/joining-an-array-module.bash"

function print_active_status(){

    declare -i is_active=$1
    local parameters_for_functionalities="${@:2}"
    local joined_functionality_names=$(join_an_array "," $parameters_for_functionalities)

    if [[ $is_active == 0 ]]; then
        echo "\`$joined_functionality_names\` are BOTH currently enabled in this shell"
    else
        echo "\`$joined_functionality_names\` are NOT BOTH currently enabled in this shell"
    fi
}

main(){
    declare -i is_active=0

    set -o | grep braceexpand
    set -o | grep hashall

    is_all_functionalities_enabled Bh
    is_active=$?
    print_active_status $is_active "braceexpand" "hashall" 
}

main
```

executing main script will echo

```
braceexpand     on
hashall         on
`braceexpand,hashall` are BOTH currently enabled in this shell

```

## CH3-5 -- look at at least one of functionalities is enabled
### Examples
#### Example 1

utility modules:

`check-at-least-one-of-functionalities-is-enabled-module.bash`

```
function is_any_functionalities_enabled(){
    local query="$1"
    # 遍歷輸入的每一個字元
    for (( i=0; i<${#query}; i++ )); do
        local char="${query:$i:1}"
        if [[ $- == *"$char"* ]]; then 
            return 0 # 只要有一個找到，就回傳成功
        fi
    done
    return 1 # 全部都沒有找到
}
```

`joining-an-array-module.bash`

```
# !/bin/bash

### 模組名稱: joinning-an-array-module.bash

## 主要函式
## 目的:
## 串接參數至array。
## 串接這種多個參數`"apple" "orange"`至array。
## 
## 詳細實作細節:
## 將第零個引數當成seperator來join第一個引數至最後一個引數成以下形式
##
## $2c$1c$3
## 
## where `c` is a character, `$2`為第一個引數
##
## 注意事項:
## + 第零個引數當成seperator必須為一個字元。
## + 以zero-based index為準。
## 
## 回傳值:
## return 退出代碼代表成功地串接array。
## echo 出串接完的字串，當失敗地串接array時，將echo ""
##
## Case 1:當引數值不合法時
## return(失敗)退出代碼。並echo空字串
##
## Case 2:當引數值不合法時
## return(成功)退出代碼。並echo串接完後的字串
function join_an_array(){
    local seperator="$1"
    declare -i seperator_length=${#seperator}
    if [[ $seperator_length != 1 ]]; then
        # when the seperator is NOT a character, 
        # return exit code with failure
        # echo empty string as failure
        echo ""
        return 1 
    fi
    local args=("${@:2}")

    # core of logic 
    # 保存原本的`$IFS`
    local old_ifs=$IFS

    # 將其暫時設為目標分隔符
    IFS=$seperator

    # 將陣列元素展開為單一字串
    local joined_str="${args[*]}"
    
    # 還原 `$IFS`
    IFS=$old_ifs

    # return exit code with success
    # echo string after joined (also with success)
    echo "$joined_str"
    return 0
}

```

main script:

`check-at-least-one-of-functionalities-is-enabled-example-1.bash`

```
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

# Source the module using an absolute path derived from the script location
source "$SCRIPT_DIR/../../utility modules/functionality enabled/check-at-least-one-of-functionalities-is-enabled-module.bash"

source "$SCRIPT_DIR/../../utility modules/joining/joining-an-array-module.bash"

function print_active_status(){

    declare -i is_active=$1
    local parameters_for_functionalities="${@:2}"
    local joined_functionality_names=$(join_an_array "," $parameters_for_functionalities)

    if [[ $is_active == 0 ]]; then
        echo "At least one of functionalities \`$joined_functionality_names\` is currently enabled in this shell"
    else
        echo "All functionalities \`$joined_functionality_names\` are BOTH currently disabled in this shell"
    fi
}

main(){
    declare -i is_active=0

    echo "---------------- 測試1 ----------------"
    set -o | grep braceexpand
    set -o | grep hashall

    is_any_functionalities_enabled Bh
    is_active=$?
    print_active_status $is_active "braceexpand" "hashall" 
    echo "---------------- end of 測試1 ----------------"

    echo "---------------- 測試2 ----------------"
    set -o | grep braceexpand
    set -o | grep errexit

    is_any_functionalities_enabled Be
    is_active=$?
    print_active_status $is_active "braceexpand" "errexit" 
    echo "---------------- end of 測試2 ----------------"

    echo "---------------- 測試3 ----------------"
    set -o | grep xtrace
    set -o | grep errexit

    is_any_functionalities_enabled xe
    is_active=$?
    print_active_status $is_active "xtrace" "errexit" 
    echo "---------------- end of 測試3 ----------------"
}

main
```

executing main script will echo

```
---------------- 測試1 ----------------
braceexpand     on
hashall         on
At least one of functionalities `braceexpand,hashall` is currently enabled in this shell
---------------- end of 測試1 ----------------
---------------- 測試2 ----------------
braceexpand     on
errexit         off
At least one of functionalities `braceexpand,errexit` is currently enabled in this shell
---------------- end of 測試2 ----------------
---------------- 測試3 ----------------
xtrace          off
errexit         off
All functionalities `xtrace,errexit` are BOTH currently disabled in this shell
---------------- end of 測試3 ----------------

```
