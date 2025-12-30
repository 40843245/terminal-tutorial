# CH22 -- anonymous pipe
## objectives
You will learn how to

  + use anonymous

## CH22-1 -- syntax and description
syntax:

Represented with RE.

```
(time (\-p)?)? (!)? {connection}
```

where

```
{connection}:= {commands}
{commands}:= {command} ({pipeline-operators} {command})*
{pipeline-operators}:= {standard-pipeline} | {pipeline-with-return-value}
{standard-pipeline}:= "|"
{pipeline-with-return-value}:= "|&"
```

> [!NOTE]
> `|&` is shorthand of `2>&1 |` which means stream out from stderr, stdout into next command among the consecutive commands then pass it to next command

- pipeline operators:

+ `|`: After finishing executing this command, executes next command without arguments
+ `|&`: After finishing executing this command, executes next command with arguments passed from this command with value -- returned value (exit code) of this command

- return value

Case 1: If `pipefail` functionality is NOT enabled (it is the default),

  The connection will return the exit code of last executed command regardless the exit code that is returned by other commands 

Case 2: Otherwise

  The connection will return the exit code of last executed command with non-zero exit code (indicating failure)
  
`!` negates the value that will be returned of the connection.

## CH22-2 -- rationales
### independency
When starting to execute one among the consecutive commands, it is executed in different subshell.

So, they are independent. Thus, 

+ when value of one local, global variable without exporting (using `export` built-in command) is changed, typically, it will NOT affect in other and main shell.

> [!IMPORTANT]
> Exceptions of use case:
>
> + `lastpipe` functionality is disabled and job control is closed.
>
> In above example, last command will be executed in main shell. So that you can set the value of variable in last command and make it affect. 

## CH22-3 -- precedence
The following has the highest to lowest precedence, respectively

1. connection using pipeline operators
2. redirection

## CH22-4 -- special variable
### `PIPESTATUS`
special variable `PIPESTATUS` is an indexed array that stores the exit code of the previous one commands (if it uses pipeline), or previous one command (if it does NOT use pipeline)

> [!NOTE]
> `PIPESTATUS` has a feature: `flicker`, so that onces after the other command executes, the value of it will be changed.
>
> Thus, please store the value of it immediately after executing a command.
>
>> [!DO]
>> ```
>> find "$directory_to_find" -name "$pattern_to_match" | wc -l
>> rc=("${PIPESTATUS[@]}")
>> ```
>
>> [!DON'T]
>> ```
>> find "$directory_to_find" -name "$pattern_to_match" | wc -l
>> echo "DON'T DO LIKE THIS"
>> rc=("${PIPESTATUS[@]}")
>> ``` 


## Examples
### Example 1
This example illustrates how to get the number of entries in specified directory with matching pattern (using regex) through `find` command

`pipeline-example-1.bash`

```
function pipeline_commands(){
    local directory_to_find="$1"
    local pattern_to_match="$2"

    # 定義一個NameRef，參考第三個參數的值
    local -n _ref_target=$3

    # 定義該NameRef為整數型別
    declare -i _ref_target
    find "$directory_to_find" -name "$pattern_to_match" |& wc -l |& xargs printf "there are \`%d\` entries that satifies the matching pattern \`${pattern_to_match}\` in this directory \`${directory_to_find}\`\n"

    _ref_target=$?
}

function print_pipeline_commands(){
    declare -i result=$1
    local message="" 
    if [[ $result -eq 0 ]]; then
        message="The last command among above commands using pipeline is success executed with exit code \`${result}\`"
    else
        message="The last command among above commands using pipeline is executed with failure -- exit code \`${result}\`"
    fi
    echo "$message"
}

main(){
    local directory_to_find=""
    local pattern_to_match=""
    declare -i returned_value=0
    declare -i index=1

    printf "use case \`%d\`:\n" ${index}
    directory_to_find="/d/workspace/Bash/Bash tutorial/outputs/examples"
    pattern_to_match="*.txt"

    pipeline_commands "$directory_to_find" "$pattern_to_match" returned_value
    print_pipeline_commands $returned_value

    ((index++))

    printf "use case \`%d\`:\n" ${index}
    directory_to_find="/d/workspace/Bash/Bash tutorial/outputs/examples"
    pattern_to_match="*.bash"

    pipeline_commands "$directory_to_find" "$pattern_to_match" returned_value
    print_pipeline_commands $returned_value

    ((index++))
}

main
```

executing this script will echo

```
$ source "D:\workspace\Bash\Bash tutorial\examples\pipeline\pipeline-example-1.bash"
use case `1`:
there are `5` entries that satifies the matching pattern `*.txt` in this directory `/d/workspace/Bash/Bash tutorial/outputs/examples`
The last command among above commands using pipeline is success executed with exit code `0`
use case `2`:
there are `2` entries that satifies the matching pattern `*.bash` in this directory `/d/workspace/Bash/Bash tutorial/outputs/examples`
The last command among above commands using pipeline is success executed with exit code `0`

```

verification:

This verifies the script works correctly.

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ find '/d/workspace/Bash/Bash tutorial/outputs/examples' -name '*.txt' | wc -l
5

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ find '/d/workspace/Bash/Bash tutorial/outputs/examples' -name '*.bash' | wc -l
2

```

### Example 2
This example illustrates how to use `PIPESTATUS` special variable.

utility modules:

`has-same-elements-module.bash`

```
## utility function
## 主要目的:
## 判斷一個indexed array中所有的元素是否皆為第二個參數的值
## > [!NOTE]
## > 第一個參數的值一定要為indexed array且所有元素皆為integer。
## > 
## > 第二個參數的值一定要為integer
function has_same_elements(){
    local -n arr=$1
    declare -i target_integer=$2
    local -n _ref_target=$3
    declare -i _ref_target

    _ref_target=0
    for item in "${arr[@]}"; do
        if [[ $item -ne $target_integer ]]; then
            # 不同
            _ref_target=1
            break;
        fi
    done
}
```

main script:

`pipeline-example-2.bash`

```
# Get the directory where the current script is located 
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

source "$SCRIPT_DIR/../../utility modules/array/indexed array/has-same-elements-module.bash"

function pipeline_commands(){
    local directory_to_find="$1"
    local pattern_to_match="$2"
    # 定義一個NameRef，參考第三個參數的值
    local -n _ref_target=$3

    # 定義該NameRef為整數型別
    declare -i _ref_target

    # 定義一個NameRef，參考第四個參數的值
    local -n _ref_rc=$4

    find "$directory_to_find" -name "$pattern_to_match" | wc -l | xargs printf "there are \`%d\` entries that satifies the matching pattern \`${pattern_to_match}\` in this directory \`${directory_to_find}\`\n" || exit 1
    _ref_rc=("${PIPESTATUS[@]}")
    _ref_target=$?
}

function print_pipeline_commands(){
    declare -i result=$1
    local -n _error_arr_ref=$2
    local message="" 
    declare -i error_arr_exit_code=1

    has_same_elements _error_arr_ref 0 error_arr_exit_code
    
    if [[ -z "${_error_arr_ref[*]}" ]]; then
        message="Unknown error!!! The array is unset or empty.\n"
    elif [[ $result -eq 0 && $error_arr_exit_code -eq 0 ]]; then
            message="The commands using pipeline is executed with success with exit code \`${result}\`"
    else
        message="The commands using pipeline is executed with failure with exit code \`${result}\`\n"
        message="${message}Detailed message as following:\n"
        for i in "${!_error_arr_ref[@]}"; do
            message="${message}At [${i}]th command, exit code:\`${_error_arr_ref[$i]}\`\n" 
        done
    fi
    printf "$message"
}

main(){
    local directory_to_find=""
    local pattern_to_match=""
    declare -i returned_value=0
    declare -i index=1
    declare -a error_message_array

    printf "use case \`%d\`:\n" ${index}
    directory_to_find="/d/workspace/Bash/Bash tutorial/outputs/examples"
    pattern_to_match="*.txt"

    pipeline_commands "$directory_to_find" "$pattern_to_match" returned_value error_message_array
    print_pipeline_commands $returned_value error_message_array

    ((index++))

    printf "use case \`%d\`:\n" ${index}
    directory_to_find="/d/workspace/Bash/Bash tutorial/outputs/examples"
    pattern_to_match="*.bash"

    pipeline_commands "$directory_to_find" "$pattern_to_match" returned_value error_message_array
    print_pipeline_commands $returned_value error_message_array

    ((index++))

    printf "use case \`%d\`:\n" ${index}
    directory_to_find="/d/workspace/Bash/Bash tutorial/non-exist-directory/"
    pattern_to_match="*.bash"

    pipeline_commands "$directory_to_find" "$pattern_to_match" returned_value error_message_array
    print_pipeline_commands $returned_value error_message_array

    ((index++))
}

set +o pipefail
main
```

executing main script will throw errors and echo

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ source "D:\workspace\Bash\Bash tutorial\examples\pipeline\pipeline-example-2.bash"
use case `1`:
there are `5` entries that satifies the matching pattern `*.txt` in this directory `/d/workspace/Bash/Bash tutorial/outputs/examples`
The commands using pipeline is executed with failure with exit code `0`
Detailed message as following:
At [0]th command, exit code:`0`
At [1]th command, exit code:`0`
At [2]th command, exit code:`0`
use case `2`:
there are `2` entries that satifies the matching pattern `*.bash` in this directory `/d/workspace/Bash/Bash tutorial/outputs/examples`
The commands using pipeline is executed with failure with exit code `0`
Detailed message as following:
At [0]th command, exit code:`0`
At [1]th command, exit code:`0`
At [2]th command, exit code:`0`
use case `3`:
find: ‘/d/workspace/Bash/Bash tutorial/non-exist-directory/’: No such file or directory
there are `0` entries that satifies the matching pattern `*.bash` in this directory `/d/workspace/Bash/Bash tutorial/non-exist-directory/`
The commands using pipeline is executed with failure with exit code `0`
Detailed message as following:
At [0]th command, exit code:`1`
At [1]th command, exit code:`0`
At [2]th command, exit code:`0`

```
