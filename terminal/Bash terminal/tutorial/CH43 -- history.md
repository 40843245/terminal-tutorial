# CH43 -- history
## objectives
You will learn how to 

  + look at specific record in history

Additionally, you will learn how to 

  + set the number of saving history record

Lastly, you will know the rationale of history record.

## CH43-1 -- rationale of history record
The history records will be written to environment variable `HISTFILE` (defaults to `~/.bash_history` where `~` is the value of environment variable `HOME`)

The history records will be written when Bash terminal is successfully closed, or when `history -w` is executed successfully.

The history records will be read once Bash terminal opens successfully.

## CH43-2 -- operation of history record
+ `history`: will list all history records
+ `history -w {filename}`: will write all commands in this session to history file -- `{filename}`, or the file where the value of environment variable `HISTFILE` if `{filename}` is NOT specified. 
+ `history -c`: clear all history records in current session.
+ `history -d {record}`: delete one history record `{record}`.

### Examples
#### Example 1
Interactions:

```
userJay30@ASUS-B1400CBNGW MINGW64 /c
$ history -c

userJay30@ASUS-B1400CBNGW MINGW64 /c
$ !!
bash: !!: event not found
```

## CH43-3 -- environment variable
### `HISTFILE`
The history records will be written to environment variable `HISTFILE` (defaults to `~/.bash_history` where `~` is the value of environment variable `HOME`)

### `HISTSIZE`
`HISTSIZE` determines the number of history records can be saved in memory.

### `HISTFILESIZE`
`HISTFILESIZE` determines the number of history records can be saved in file where environment variable `HISTFILE` .

### `HISTCONTROL`
`HISTCONTROL`: how it filters history records when they are read at startup of Bash Terminal.

| option | description |
| :-- | :-- |
| `ignorespace` | filters history records that starts with whitespace |
| `ignoredups` | filters history records that are duplicated |

### Examples
#### Example 1
utility modules:

`print-history-info-module.bash`

```
## utility function
## 主要目的:
## 列印history records的相關資訊
function print_history_record_info(){
    local hist_control_str="${HISTCONTROL}"
    local hist_control_str_message=""
    if [[ "${hist_control_str+defined}" -eq "" || "${hist_control_str}" -eq "" ]]; then
        hist_control_str_message="None"
    else
        hist_control_str_message="${hist_control_str}"
    fi
    printf "In the memory, there can save \`%s\` records\n" "${HISTSIZE}"
    printf "In the history file:\`%s\`, there can save \`%s\` records\n" "${HISTFILE}" "${HISTFILESIZE}"
    printf "How does it filter history record when they are be read at startup? \`%s\`\n" "${hist_control_str_message}"
}
```

main script:

`history-record-example-1.bash`

```
# Get the directory where the current script is located 
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

source "$SCRIPT_DIR/../../utility modules/history/print-history-info-module.bash"

main(){
    print_history_record_info
}

main
```

executing this main script will echo

```
$ source "D:\workspace\Bash\Bash tutorial\examples\history\history-record-example-1.bash"
In the memory, there can save `10000` records
In the history file:`/c/Users/userJay30/.bash_history`, there can save `1000000` records
How does it filter history record when they are be read at startup? `None`

```

## CH43-4 -- event designator
+ `!!`: exapnd the previous one command then executes it
+ `!{n}`:  exapnd the first `{n}`th command then executes it where `{n}` is an positive integer.
+ `!-{n}`:  exapnd the previous `{n}` command then executes it where `{n}` is an positive integer.
+ `!{string}`:  exapnd command that is the first occurrence that starts with the string `{string}`, where `{string}` is a string without quotation, then executes it.
+ `!?{string}?`:  exapnd command that is the first occurrence that contains the string `{string}`, where `{string}` is a string without quotation, then executes it.
+ `!#`: the number of commands typed so far in this session.
+ `^{string1}^{string2}^` : Repeat the previous one command, replacing `{string1}` to `{string2}` where `{string1}` is a string without quotation and `{string1}` is a string without quotation.

### Examples
#### Example 1
Interactions:

In Git Bash terminal,

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ !!
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ !1
bash: !1: event not found

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ !-1
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ cd "C:"

userJay30@ASUS-B1400CBNGW MINGW64 /c
$ !-2
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 /c
$ !-1
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 /c
$ !e
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 /c
$ !?echo?
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 /c
$ !?echo
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 /c
$

```

## CH43-5 -- word designator
Word designator is the last part that will be inserted into event designator.

A word designator MUST start with `:`, then 

followed by a nonnegative integer 

where

`{n}` indicates

  + the `{n}`th option or argument of the command about history expansion if `{n}` is a postive integer, or
  + the command itself if `{n}` equals to the integer 0.

or followed by one of the following

+ `^`: indicates the first option or argument (equivalent to `1`)
+ `$`: inidcates the last option or argument
+ `%`: the most recently searched matching pattern
+ `*` : all options or arguments (equivalent to `1-$`)
+ `{x}-{y}` for an nonnegative integer `{x}`, `{y}`, and `{y}` is greater than `{x}`

where

`{x}-{y}` indicates starts from `{x}`th to {y}`th , both inclusive.

+ `{x}*` indicates starts from `{x}th` to last (equivalent to `{x}:$`)
+ `{x}*` indicates starts from `{x}th` to last two.
  

For example,

executing 

```
!!
```

will expand the previous one command and then executing it

So, executing 

```
!!:0
```

will expand the previous one command (without any options and arguments) and then executing it.

So, executing 

```
!!:1
```

will expand the first option or argument of previous one command and then executing it.

So, executing 

```
!!:^
```

will expand the first option or argument of previous one command and then executing it.

So, executing 

```
!!:$
```

will expand the last option or argument of previous one command and then executing it.

So, executing 

```
!!:2-5
```

will expand the from 2th option or arguments to 5th of previous one command and then executing it.

So, executing 

```
!!:2*
```

will expand the 2th option or argument to last one of previous one command and then executing it.

So, executing 

```
!!:2-
```

will expand the 2th option or argument to last two cof previous one command and then executing it.

### Examples
#### Example 1
Interactions:

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ echo "$HOME" "$PWD" "Hello World"
/c/Users/userJay30 /c/Users/userJay30 Hello World

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ !!
echo "$HOME" "$PWD" "Hello World"
/c/Users/userJay30 /c/Users/userJay30 Hello World

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ !! "This is a sunny day"
echo "$HOME" "$PWD" "Hello World" "This is a sunny day"
/c/Users/userJay30 /c/Users/userJay30 Hello World This is a sunny day

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ !!:0 "This is a sunny day"
echo "This is a sunny day"
This is a sunny day

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ !!:1
"This is a sunny day"
bash: This is a sunny day: command not found

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ echo "$HOME" "$PWD" "Hello World" "This is a sunny day"
/c/Users/userJay30 /c/Users/userJay30 Hello World This is a sunny day

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ !!:2
"$PWD"
bash: /c/Users/userJay30: Is a directory

```

#### Example 2
Interactions:

```
userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ cd "D:\workspace\Bash\Bash tutorial"

userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ pwd
/d/workspace/Bash/Bash tutorial

userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ working_current_parent_directory="..$PWD"

userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ cd_working_current_parent_directory_command="eval \"cd \"$working_current_parent_directory\" \" "

userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ echo "$cd_working_current_parent_directory_command"
eval "cd "../d/workspace/Bash/Bash tutorial" "

userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ echo $cd_working_current_parent_directory_command
eval "cd "../d/workspace/Bash/Bash tutorial" "

userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ echo "$HOME" "$PWD" $working_current_parent_directory $cd_working_current_parent_directory_command
/c/Users/userJay30 /d/workspace/Bash/Bash tutorial ../d/workspace/Bash/Bash tutorial eval "cd "../d/workspace/Bash/Bash tutorial" "

userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ echo !!:^
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ echo !-2:2
echo "$PWD"
/d/workspace/Bash/Bash tutorial

userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ echo !-3:4
echo $cd_working_current_parent_directory_command
eval "cd "../d/workspace/Bash/Bash tutorial" "

userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ !-4:4
$cd_working_current_parent_directory_command
bash: cd ../d/workspace/Bash/Bash: No such file or directory

userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ eval !-5:4
eval $cd_working_current_parent_directory_command
bash: cd: too many arguments
```
