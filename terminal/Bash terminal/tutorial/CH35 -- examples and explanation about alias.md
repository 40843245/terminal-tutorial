# CH35 -- examples and explanation about alias
## Examples
### Example 1
#### code
`alias-example-1.bash`

```
shopt -s expand_aliases

alias say_hello='echo Hello from Alias!'
alias say_dirty_word='echo dirty word from Alias!' 

main(){
    echo "--- current list of alias ---"
    alias

    # use alias
    say_hello
    echo ""
    say_dirty_word

    echo "--- current list of alias ---"
    alias

    unalias say_hello

    echo "--- current list of alias ---"
    alias

    unalias -a

    echo "--- current list of alias ---"
    alias
}

main

shopt -u expand_aliases
```

#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-1.bash"
--- current list of alias ---
alias say_dirty_word='echo dirty word from Alias!'
alias say_hello='echo Hello from Alias!'
Hello from Alias!

dirty word from Alias!
--- current list of alias ---
alias say_dirty_word='echo dirty word from Alias!'
alias say_hello='echo Hello from Alias!'
--- current list of alias ---
alias say_dirty_word='echo dirty word from Alias!'
--- current list of alias ---

```

#### explanation
1. The Bash engine parses this code block at first

```
shopt -s expand_aliases

alias say_hello='echo Hello from Alias!'
alias say_dirty_word='echo dirty word from Alias!' 

main(){
    # ...
}

main

shopt -u expand_aliases
```

When parsing 

```
shopt -s expand_aliases
```

statement during this parsing phrase enable the functionality `expand_aliases` which allow to expand the alias during parsing phrase at compile time.

When parsing

```
alias say_hello='echo Hello from Alias!'
```

during this parsing phrase,

it gives an alias named `say_hello` for which will be expanded to `echo Hello from Alias!` at parsing phrase (if it can) then insert it into a mapping table.

When parsing

```
alias say_dirty_word='echo dirty word from Alias!' 
```

during this parsing phrase,

similarly, it gives an alias named `say_dirty_word` for which will be expanded to `echo dirty word from Alias!` at parsing phrase (if it can) then insert it into a mapping table.

2. Next, it parses the code block `main` function

```
main(){
    echo "--- current list of alias ---"
    alias

    # use alias
    say_hello
    echo ""
    say_dirty_word

    echo "--- current list of alias ---"
    alias

    unalias say_hello

    echo "--- current list of alias ---"
    alias

    unalias -a

    echo "--- current list of alias ---"
    alias
}
```

When parsing

```
say_hello
```

during this parsing phrase,

Bash engine will expand an alias named `say_hello` to `echo Hello from Alias!` 

It can expand the alias because `expand_aliases` functionality is enabled and it can found an alias named `say_hello` from mapping table (it is inserted at 1th parsing phrase)

When parsing

```
say_dirty_word
```

during this parsing phrase,

similarly, Bash engine will expand an alias named `say_dirty_word` to `echo dirty word from Alias!` 

It can expand the alias because `expand_aliases` functionality is enabled and it can found an alias named `say_dirty_word` from mapping table (it is inserted at 1th parsing phrase)

Therefore, the code block will be expanded into

```
main(){
    echo "--- current list of alias ---"
    alias

    # use alias
    echo Hello from Alias!
    echo ""
    echo dirty word from Alias!

    echo "--- current list of alias ---"
    alias

    unalias say_hello

    echo "--- current list of alias ---"
    alias

    unalias -a

    echo "--- current list of alias ---"
    alias
}
```

3. At the end of 1th parsing phrase,

It parses 

```
shopt -u expand_aliases
```

which will disable `expand_aliases` functionality, disallowing Bash engine to expand the alias at parsing phrase.

### Example 2
#### code
`alias-example-2.bash`

```
shopt -s expand_aliases

alias alias_func1='func1'
alias alias_func2='func2'
alias alias_func3='func3'

function func1(){
    echo "In function named \`func1\`"
}

function func2(){
    echo "In function named \`${FUNCNAME[0]}\`"
}

function func3(){
    echo "In function named \`${FUNCNAME[0]}\`, it receives these arguments \`$#\`"
}

main(){
    echo "--- current list of alias ---"
    alias

    # use alias

    echo "call function named \`func1\` by using function name"
    func1
    echo "call function named \`func1\` by using alias name"
    alias_func1

    echo "call function named \`func2\` by using function name"
    func2
    echo "call function named \`func2\` by using alias name"
    alias_func2

    echo "call function named \`func3\` by using function name"
    func3 "Hello World"
    echo "call function named \`func1\` by using alias name"
    alias_func3 "Hello World"
}

main

shopt -u expand_aliases
```

#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-2.bash"
--- current list of alias ---
alias alias_func1='func1'
alias alias_func2='func2'
alias alias_func3='func3'
call function named `func1` by using function name
In function named `func1`
call function named `func1` by using alias name
In function named `func1`
call function named `func2` by using function name
In function named `func2`
call function named `func2` by using alias name
In function named `func2`
call function named `func3` by using function name
In function named `func3`, it receives these arguments `1`
call function named `func1` by using alias name
In function named `func3`, it receives these arguments `1`

```

#### explanation
1. Bash engine parses this code block at first

```
shopt -s expand_aliases

alias alias_func1='func1'
alias alias_func2='func2'
alias alias_func3='func3'

function func1(){
    # ...
}

function func2(){
    # ...
}

function func3(){
    # ...
}

main(){
    # ...
}

main

shopt -u expand_aliases
```

When parsing

```
alias alias_func1='func1'
```

during this parsing phrase,

it gives an alias named `alias_func1` for which will be expanded to `func1` at parsing phrase (if it can) then insert it into a mapping table.

Similarly, when parsing

```
alias alias_func2='func2'
```

during this parsing phrase,

it gives an alias named `alias_func2` for which will be expanded to `func2` at parsing phrase (if it can) then insert it into a mapping table.

Similarly, when parsing

```
alias alias_func3='func3'
```

during this parsing phrase,

it gives an alias named `alias_func3` for which will be expanded to `func3` at parsing phrase (if it can) then insert it into a mapping table.

2. Next, it parses `func1`, `func2`,`func3`,`main` function respectively.

Look at `main` function body

```
main(){
    echo "--- current list of alias ---"
    alias

    # use alias

    echo "call function named \`func1\` by using function name"
    func1
    echo "call function named \`func1\` by using alias name"
    alias_func1

    echo "call function named \`func2\` by using function name"
    func2
    echo "call function named \`func2\` by using alias name"
    alias_func2

    echo "call function named \`func3\` by using function name"
    func3 "Hello World"
    echo "call function named \`func1\` by using alias name"
    alias_func3 "Hello World"
}
```

When parsing

```
alias_func1
```

during this parsing phrase,

Bash engine will expand an alias named `alias_func1` to `func1` 

It can expand the alias because `expand_aliases` functionality is enabled and it can found an alias named `alias_func1` from mapping table (it is inserted at 1th parsing phrase)

Again, when parsing

```
alias_func2
```

during this parsing phrase,

Bash engine will expand an alias named `alias_func2` to `func2` 

It can expand the alias because `expand_aliases` functionality is enabled and it can found an alias named `alias_func2` from mapping table (it is inserted at 1th parsing phrase)

Again, when parsing

```
alias_func3
```

during this parsing phrase,

Bash engine will expand an alias named `alias_func3` to `func3` 

It can expand the alias because `expand_aliases` functionality is enabled and it can found an alias named `alias_func3` from mapping table (it is inserted at 1th parsing phrase)

Therefore, the code block will be expanded into

```
main(){
    echo "--- current list of alias ---"
    alias

    # use alias

    echo "call function named \`func1\` by using function name"
    func1
    echo "call function named \`func1\` by using alias name"
    func1

    echo "call function named \`func2\` by using function name"
    func2
    echo "call function named \`func2\` by using alias name"
    func2

    echo "call function named \`func3\` by using function name"
    func3 "Hello World"
    echo "call function named \`func1\` by using alias name"
    func3 "Hello World"
}
```

3. At the end of 1th parsing phrase,

It parses 

```
shopt -u expand_aliases
```

which will disable `expand_aliases` functionality, disallowing Bash engine to expand the alias at parsing phrase.

### Example 3
#### code
`alias-example-3.bash`

```
shopt -s expand_aliases

function func1(){
    echo "In function named \`func1\`"
}

function func2(){
    echo "In function named \`${FUNCNAME[0]}\`"
}


function func3(){
    echo "In function named \`${FUNCNAME[0]}\`, it receives these arguments \`$#\`"
}

alias alias_func1='func1'
alias alias_func2='func2'
alias alias_func3='func3'

main(){
    echo "--- current list of alias ---"
    alias

    # use alias

    echo "call function named \`func1\` by using function name"
    func1
    echo "call function named \`func1\` by using alias name"
    alias_func1

    echo "call function named \`func2\` by using function name"
    func2
    echo "call function named \`func2\` by using alias name"
    alias_func2

    echo "call function named \`func3\` by using function name"
    func3 "Hello World"
    echo "call function named \`func1\` by using alias name"
    alias_func3 "Hello World"
}

main

shopt -u expand_aliases
```

#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-3.bash"
--- current list of alias ---
alias alias_func1='func1'
alias alias_func2='func2'
alias alias_func3='func3'
call function named `func1` by using function name
In function named `func1`
call function named `func1` by using alias name
In function named `func1`
call function named `func2` by using function name
In function named `func2`
call function named `func2` by using alias name
In function named `func2`
call function named `func3` by using function name
In function named `func3`, it receives these arguments `1`
call function named `func1` by using alias name
In function named `func3`, it receives these arguments `1`

```

#### explanation
Similar to explanation in example 2.

Here, the aliased name is parsed after parsing `func1`,`func2`,`func3` does NOT affect the behavior because there are no aliases need to be expanded.

### Example 4
#### code
`alias-example-4.bash`

```
shopt -s expand_aliases

function func1(){
    echo "In function named \`func1\`"
}

function func2(){
    echo "In function named \`${FUNCNAME[0]}\`"
}

function func3(){
    echo "In function named \`${FUNCNAME[0]}\`, it receives these arguments \`$#\`"
}

main(){
    echo "--- current list of alias ---"
    alias

    alias alias_func1='func1'
    alias alias_func2='func2'
    alias alias_func3='func3'

    # use alias

    echo "call function named \`func1\` by using function name"
    func1
    echo "call function named \`func1\` by using alias name"
    alias_func1

    echo "call function named \`func2\` by using function name"
    func2
    echo "call function named \`func2\` by using alias name"
    alias_func2

    echo "call function named \`func3\` by using function name"
    func3 "Hello World"
    echo "call function named \`func1\` by using alias name"
    alias_func3 "Hello World"
}

main

shopt -u expand_aliases
```

#### output
executing this script will throw errors and echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-4.bash"
--- current list of alias ---
call function named `func1` by using function name
In function named `func1`
call function named `func1` by using alias name
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-4.bash: line 28: alias_func1: command not found
call function named `func2` by using function name
In function named `func2`
call function named `func2` by using alias name
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-4.bash: line 33: alias_func2: command not found
call function named `func3` by using function name
In function named `func3`, it receives these arguments `1`
call function named `func1` by using alias name
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-4.bash: line 38: alias_func3: command not found

```

#### explanation
The reason why throwing this error with error message

```
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-4.bash: line 28: alias_func1: command not found
```

saying `command not found` is

1. During Bash engine parses the code block -- `main` body,

When parsing

```
alias_func1
```

it will NOT find an alias named `alias_func1` from the mapping table, even though it is inserted before the statement `alias_func1`, 

since Bash engine parses the whole `main` block (explained in [2. The "Parsing Time" Traps](https://github.com/40843245/terminal-tutorial/blob/main/terminal/Bash%20terminal/tutorial/CH34%20--%20parsing%20rule%20of%20aliasing%20names.md#2-the-parsing-time-trap))

More precisely said, 

the alias name `alias_func1` will be inserted into mapping table when executing

```
alias alias_func1='func1'
```

inside the `main` function.

However, it is NOT executed when parsing the whole `main` block (NOTE: not `main` command itself)

And so on for `alias_func2`, `alias_func3`

Thus, `main` body is expanded to

```
main(){
    echo "--- current list of alias ---"
    alias

    alias alias_func1='func1'
    alias alias_func2='func2'
    alias alias_func3='func3'

    # use alias

    echo "call function named \`func1\` by using function name"
    func1
    echo "call function named \`func1\` by using alias name"
    alias_func1

    echo "call function named \`func2\` by using function name"
    func2
    echo "call function named \`func2\` by using alias name"
    alias_func2

    echo "call function named \`func3\` by using function name"
    func3 "Hello World"
    echo "call function named \`func1\` by using alias name"
    alias_func3 "Hello World"
}
```

Consequently, when executing `main` function, 

the Bash engine can't find the command `alias_func1`, 

throwing an error like `command not found`

### Example 5
#### code
`alias-example-5.bash`

```
shopt -s expand_aliases

alias alias_func1='func1'
alias alias_func2='func2'

function func1(){
    echo "In function named \`func1\`"
}

function func2(){
    echo "In function named \`${FUNCNAME[0]}\`"
}

function func3(){
    echo "In function named \`${FUNCNAME[0]}\`, it receives these arguments \`$#\`"
}

main(){
    echo "--- current list of alias ---"
    alias

    alias alias_func3='func3'
    
    function func3(){
        echo "Defined in function named \`main\` scope. In function named \`${FUNCNAME[0]}\`, it receives these arguments \`$#\`"
    }

    # use alias

    echo "call function named \`func1\` by using function name"
    func1
    echo "call function named \`func1\` by using alias name"
    alias_func1

    echo "call function named \`func2\` by using function name"
    func2
    echo "call function named \`func2\` by using alias name"
    alias_func2

    echo "call function named \`func3\` by using function name"
    func3 "Hello World"
    echo "call function named \`func1\` by using alias name"
    alias_func3 "Hello World"
}

main

shopt -u expand_aliases
```

#### output
executing this script will throw errors and echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-5.bash"
--- current list of alias ---
alias alias_func1='func1'
alias alias_func2='func2'
call function named `func1` by using function name
In function named `func1`
call function named `func1` by using alias name
In function named `func1`
call function named `func2` by using function name
In function named `func2`
call function named `func2` by using alias name
In function named `func2`
call function named `func3` by using function name
Defined in function named `main` scope. In function named `func3`, it receives these arguments `1`
call function named `func1` by using alias name
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-5.bash: line 43: alias_func3: command not found

```

#### explanation
The reason why throws an error `alias_func3` command not found is same as the reason in example 4. Skip it.

### Example 6
#### code
`alias-example-6.bash"`

```
shopt -s expand_aliases

main(){
    echo "--- current list of alias ---"
    alias
    
    alias say_hello='echo Hello from Alias!'
    alias say_dirty_word='echo dirty word from Alias!' 

    # use alias
    eval "say_hello"
    echo ""
    eval "say_dirty_word"
}

main

shopt -u expand_aliases
```
#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-6.bash"
--- current list of alias ---
Hello from Alias!

dirty word from Alias!

```

#### explanation
In this example, there are no error due to re-parsing using `eval` bulit-in command.

Take this statement as example.

```
eval "say_hello"
```

In this statement,

`eval` will force Bash engine to re-parse with argument `say_hello`.

When re-parsing with argument `say_hello`, it can be found from the mapping table 

since it is inserted into when executing this statement

```
alias say_hello='echo Hello from Alias!'
```

defined in `main` function, but before executing

```
eval "say_hello"
```

### Example 7
#### code
`alias-example-7.bash`

```
shopt -s expand_aliases

function func1(){
    echo "In function named \`func1\`"
}

function func2(){
    echo "In function named \`${FUNCNAME[0]}\`"
}

function func3(){
    echo "In function named \`${FUNCNAME[0]}\`, it receives these arguments \`$#\`"
}

main(){
    echo "--- current list of alias ---"
    alias

    alias alias_func1='func1'
    alias alias_func2='func2'
    alias alias_func3='func3'

    # use alias
    echo "call function named \`func1\` by using function name"
    func1
    echo "call function named \`func1\` by using alias name"
    eval "alias_func1"

    echo "call function named \`func2\` by using function name"
    func2
    echo "call function named \`func2\` by using alias name"
    eval "alias_func2"

    echo "call function named \`func3\` by using function name"
    func3 "Hello World"
    echo "call function named \`func1\` by using alias name"
    eval "alias_func3 \"Hello World\""
}

main

shopt -u expand_aliases
```

#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-7.bash"
--- current list of alias ---
call function named `func1` by using function name
In function named `func1`
call function named `func1` by using alias name
In function named `func1`
call function named `func2` by using function name
In function named `func2`
call function named `func2` by using alias name
In function named `func2`
call function named `func3` by using function name
In function named `func3`, it receives these arguments `1`
call function named `func1` by using alias name
In function named `func3`, it receives these arguments `1`

```

#### explanation
Similar to explanation in example 6, it uses `eval` to force Bash engine to re-parse it with arguments.

### Example 8
#### code
`alias-example-8.bash`

```
shopt -s expand_aliases

alias alias_alias_func1='alias_func1'
alias alias_alias_func2='alias_func2'
alias alias_alias_func3='alias_func3'

alias alias_func1='func1'
alias alias_func2='func2'
alias alias_func3='func3'

function func1(){
    echo "In function named \`func1\`"
}

function func2(){
    echo "In function named \`${FUNCNAME[0]}\`"
}

function func3(){
    echo "In function named \`${FUNCNAME[0]}\`, it receives these arguments \`$#\`"
}

main(){
    echo "--- current list of alias ---"
    alias

    # use alias
    echo "call function named \`func1\` by using function name"
    func1
    echo "call function named \`func1\` by using alias name"
    eval "alias_func1"
    echo "call function named \`func1\` by using alias name of alias name"
    eval "alias_alias_func1"

    echo "call function named \`func2\` by using function name"
    func2
    echo "call function named \`func2\` by using alias name"
    eval "alias_func2"
    echo "call function named \`func2\` by using alias name of alias name"
    eval "alias_alias_func2"

    echo "call function named \`func3\` by using function name"
    func3 "Hello World"
    echo "call function named \`func1\` by using alias name"
    eval "alias_func3 \"Hello World\""
    echo "call function named \`func2\` by using alias name of alias name"
    eval "alias_alias_func3 \"Hello World\""
}

main

shopt -u expand_aliases
```
#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-8.bash"
--- current list of alias ---
alias alias_alias_func1='alias_func1'
alias alias_alias_func2='alias_func2'
alias alias_alias_func3='alias_func3'
alias alias_func1='func1'
alias alias_func2='func2'
alias alias_func3='func3'
call function named `func1` by using function name
In function named `func1`
call function named `func1` by using alias name
In function named `func1`
call function named `func1` by using alias name of alias name
In function named `func1`
call function named `func2` by using function name
In function named `func2`
call function named `func2` by using alias name
In function named `func2`
call function named `func2` by using alias name of alias name
In function named `func2`
call function named `func3` by using function name
In function named `func3`, it receives these arguments `1`
call function named `func1` by using alias name
In function named `func3`, it receives these arguments `1`
call function named `func2` by using alias name of alias name
In function named `func3`, it receives these arguments `1`

```
#### explanation
Similar to explanation of exmaple 6. 

Additionally, the example illustrates that 

    + an alias can be aliased by other aliases.
    + re-parse chain using `eval`:

        When re-parsing, the Bash engine will parse an alias again and again until it is NOT parsed to an alias  

### Example 9
#### code
`alias-example-9.bash`

```
shopt -s expand_aliases

alias alias_alias_func1='alias_func1'
alias alias_alias_func2='alias_func2'
alias alias_alias_func3='alias_func3'

alias alias_func1='func1'
alias alias_func2='func2'
alias alias_func3='func3'

function func1(){
    echo "In function named \`func1\`"
}

function func2(){
    echo "In function named \`${FUNCNAME[0]}\`"
}

function func3(){
    echo "In function named \`${FUNCNAME[0]}\`, it receives these arguments \`$#\`"
}

main(){
    echo "--- current list of alias ---"
    alias

    # use alias
    echo "call function named \`func1\` by using function name"
    func1
    echo "call function named \`func1\` by using alias name"
    eval "alias_func1"
    echo "call function named \`func1\` by using alias name of alias name"
    eval "alias_alias_func1"

    echo "call function named \`func2\` by using function name"
    func2
    echo "call function named \`func2\` by using alias name"
    alias_func2
    echo "call function named \`func2\` by using alias name of alias name"
    alias_alias_func2

    echo "call function named \`func3\` by using function name"
    func3 "Hello World"
    echo "call function named \`func1\` by using alias name"
    alias_func3 "Hello World"
    echo "call function named \`func2\` by using alias name of alias name"
    alias_alias_func3 "Hello World"
}

main

shopt -u expand_aliases
```
#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-9.bash"
--- current list of alias ---
alias alias_alias_func1='alias_func1'
alias alias_alias_func2='alias_func2'
alias alias_alias_func3='alias_func3'
alias alias_func1='func1'
alias alias_func2='func2'
alias alias_func3='func3'
call function named `func1` by using function name
In function named `func1`
call function named `func1` by using alias name
In function named `func1`
call function named `func1` by using alias name of alias name
In function named `func1`
call function named `func2` by using function name
In function named `func2`
call function named `func2` by using alias name
In function named `func2`
call function named `func2` by using alias name of alias name
In function named `func2`
call function named `func3` by using function name
In function named `func3`, it receives these arguments `1`
call function named `func1` by using alias name
In function named `func3`, it receives these arguments `1`
call function named `func2` by using alias name of alias name
In function named `func3`, it receives these arguments `1`

```
#### explanation
Similar to the explanation of example 6. Skip it.

### Example 10
#### code
utility module:

`shopt-many-functionalities-both-enabled-module.bash`

```
### 模組
### 目的:
### 給定一系列的選項名稱(shopt能接受的選項)，檢查所有功能是否處於啟用狀態

## 主要函式
## 目的:
## 給定一系列的選項名稱(shopt能接受的選項)，檢查所有功能是否處於啟用狀態
## 回傳值:
## 0: 所有功能是否處於啟用狀態
## 1:其他
function is_all_functionalities_enabled(){
    for item in "$@"; do
        shopt -q "$item"
        if [ $? -eq 0 ]; then
            return 1
        fi
    done
    return 0
}
```

main script:

`alias-example-10.bash`

```
shopt -s expand_aliases

# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

alias are_all_functionalities_enabled='is_all_functionalities_enabled '

source "$SCRIPT_DIR/../../utility modules/functionality enabled/shopt-many-functionalities-both-enabled-module.bash"

main(){
    declare -i enabled=0;
    local status_text=""
    local args

    type is_all_functionalities_enabled

    echo "--- current list of alias ---"
    alias

    # use alias
    args="expand_aliases"
    echo "call function named \`is_all_functionalities_enabled\` by using function name"
    is_all_functionalities_enabled "$args"
    enabled=$?
    if [[ $enabled -eq 0 ]]; then
        status_text="These functionalities \`$args\` are both enabled"
    else
        status_text="These functionalities \`$args\` are NOT both enabled"
    fi
    echo "$status_text"

    args="expand_aliases"
    echo "call function named \`is_all_functionalities_enabled\` by using alias name"
    eval "are_all_functionalities_enabled \"$args\""
    enabled=$?
    if [[ $enabled -eq 0 ]]; then
        status_text="These functionalities \`$args\` are both enabled"
    else
        status_text="These functionalities \`$args\` are NOT both enabled"
    fi
    echo "$status_text"

    args="expand_aliases"
    echo "call function named \`is_all_functionalities_enabled\` by using alias name"
    are_all_functionalities_enabled "$args"
    enabled=$?
    if [[ $enabled -eq 0 ]]; then
        status_text="These functionalities \`$args\` are both enabled"
    else
        status_text="These functionalities \`$args\` are NOT both enabled"
    fi
    echo "$status_text"
}

main

shopt -u expand_aliases
```
#### output
executing this main script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-10.bash"
is_all_functionalities_enabled is a function
is_all_functionalities_enabled ()
{
    for item in "$@";
    do
        shopt -q "$item";
        if [ $? -eq 0 ]; then
            return 1;
        fi;
    done;
    return 0
}
--- current list of alias ---
alias are_all_functionalities_enabled='is_all_functionalities_enabled '
call function named `is_all_functionalities_enabled` by using function name
These functionalities `expand_aliases` are NOT both enabled
call function named `is_all_functionalities_enabled` by using alias name
These functionalities `expand_aliases` are NOT both enabled
call function named `is_all_functionalities_enabled` by using alias name
These functionalities `expand_aliases` are NOT both enabled

```
#### explanation
Although the alias statement

```
alias are_all_functionalities_enabled='is_all_functionalities_enabled '
```

`is_all_functionalities_enabled` function has NOT been imported yet,

it is unrelated to behavior 

since Bash engine will expand the alias name (here, is `are_all_functionalities_enabled`) to its content (here, is `is_all_functionalities_enabled `) using `alias` built-in command

if it can (i.e. the alias name can be found from mapping table and it is allowed to Bash engine expands the alias name 

### Example 11
#### code
utility module:

`alias-name-defined-in-external-source-example-1.bash`

```
shopt -s expand_aliases

alias alias_func3='func3'

function func3(){
    echo "In function named \`${FUNCNAME[0]}\`, it receives these arguments \`$#\`"
}

shopt -u expand_aliases
```

main script:

`alias-example-11.bash`

```
shopt -s expand_aliases

# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

source "$SCRIPT_DIR/../../utility modules/alias/alias-name-defined-in-external-source-example-1.bash"

main(){
    echo "--- current list of alias ---"
    alias

    # use alias
    echo "call function named \`func3\` defined on external source by using function name"
    func3 "expand_aliases"
    echo "call function named \`func3\` defined on external source by using alias name"
    eval "alias_func3 \"expand_aliases\""
    echo "call function named \`func3\` defined on external source by using alias name"
    alias_func3 "expand_aliases"
}

main

shopt -u expand_aliases
```
#### output
executing this main script will throw errors and echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-11.bash"
--- current list of alias ---
alias alias_func3='func3'
call function named `func3` defined on external source by using function name
In function named `func3`, it receives these arguments `1`
call function named `func3` defined on external source by using alias name
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-11.bash: line 16: alias_func3: command not found
call function named `func3` defined on external source by using alias name
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-11.bash: line 18: alias_func3: command not found

```
#### explanation
Although it is inserted to mapping table at parsing phrase at source file,

the `expand_aliases` functionality is disabled at the end of source file.

Thus, it is NOT allowed to expand the alias name with Bash engine when parsing at compiled phrase and re-parsing at execution phrase

### Example 12
#### code
utility modules

`alias-name-defined-in-external-source-example-2.bash`

```
shopt -s expand_aliases

alias alias_func3='func3'

function func3(){
    echo "In function named \`${FUNCNAME[0]}\`, it receives these arguments \`$#\`"
}
```

main script:

`alias-example-12.bash`

```
shopt -s expand_aliases

# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

source "$SCRIPT_DIR/../../utility modules/alias/alias-name-defined-in-external-source-example-2.bash"

main(){
    echo "--- current list of alias ---"
    alias

    # use alias
    echo "call function named \`func3\` defined on external source by using function name"
    func3 "expand_aliases"
    echo "call function named \`func3\` defined on external source by using alias name"
    eval "alias_func3 \"expand_aliases\""
    echo "call function named \`func3\` defined on external source by using alias name"
    alias_func3 "expand_aliases"
}

main

shopt -u expand_aliases
```

#### output
executing this main script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-12.bash"
--- current list of alias ---
alias alias_func3='func3'
call function named `func3` defined on external source by using function name
In function named `func3`, it receives these arguments `1`
call function named `func3` defined on external source by using alias name
In function named `func3`, it receives these arguments `1`
call function named `func3` defined on external source by using alias name
In function named `func3`, it receives these arguments `1`

```

#### explanation
The alias can be expaneded when parsing at parsing phrase.

### Example 13
#### code
utility modules:

`alias-name-defined-in-external-source-example-3.bash`

```
shopt -s expand_aliases

function func3(){
    echo "In function named \`${FUNCNAME[0]}\`, it receives these arguments \`$#\`"
}
```

main script:

`alias-example-13.bash`

```
shopt -s expand_aliases

alias alias_func3='func3'
# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

source "$SCRIPT_DIR/../../utility modules/alias/alias-name-defined-in-external-source-example-3.bash"

main(){
    echo "--- current list of alias ---"
    alias

    # use alias
    echo "call function named \`func3\` defined on external source by using function name"
    func3 "expand_aliases"
    echo "call function named \`func3\` defined on external source by using alias name"
    eval "alias_func3 \"expand_aliases\""
    echo "call function named \`func3\` defined on external source by using alias name"
    alias_func3 "expand_aliases"
}

main

shopt -u expand_aliases
```
#### output
executing this main script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-13.bash"
--- current list of alias ---
alias alias_func3='func3'
call function named `func3` defined on external source by using function name
In function named `func3`, it receives these arguments `1`
call function named `func3` defined on external source by using alias name
In function named `func3`, it receives these arguments `1`
call function named `func3` defined on external source by using alias name
In function named `func3`, it receives these arguments `1`

```
#### explanation
Similar to explanation in Example 12

### Example 14
#### code
utility modules:

`alias-name-defined-in-external-source-example-4.bash`

```
shopt -s expand_aliases

function func3(){
    echo "In function named \`${FUNCNAME[0]}\`, it receives these arguments \`$#\`"
}
```

main script:

`alias-example-14.bash`

```
shopt -s expand_aliases

# Get the directory where the current script is located
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

source "$SCRIPT_DIR/../../utility modules/alias/alias-name-defined-in-external-source-example-4.bash"

alias alias_func3='func3'

main(){
    echo "--- current list of alias ---"
    alias

    # use alias
    echo "call function named \`func3\` defined on external source by using function name"
    func3 "expand_aliases"
    echo "call function named \`func3\` defined on external source by using alias name"
    eval "alias_func3 \"expand_aliases\""
    echo "call function named \`func3\` defined on external source by using alias name"
    alias_func3 "expand_aliases"
}

main

shopt -u expand_aliases
```

#### output
executing this main script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-14.bash"
--- current list of alias ---
alias alias_func3='func3'
call function named `func3` defined on external source by using function name
In function named `func3`, it receives these arguments `1`
call function named `func3` defined on external source by using alias name
In function named `func3`, it receives these arguments `1`
call function named `func3` defined on external source by using alias name
In function named `func3`, it receives these arguments `1`

```
#### explanation
Similar to explanation in Example 13

### Example 15
#### code
`alias-example-15.bash`

```
shopt -s expand_aliases

alias echo_var='echo "var:\`$var\`"'
alias echo_var_twice='echo_var echo_var'

main(){
    echo "--- current list of alias ---"
    alias
    declare -i var=2

    echo "before echoing variable using alias name"
    printf "var:\`%d\`" $var
    
    # use alias
    eval "echo_var"

    echo "after echoing variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    echo_var

    echo "after echoing variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    eval "echo_var_twice"

    echo "after echoing variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""

    echo_var_twice

    echo "after echoing variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""
}

main

shopt -u expand_aliases
```

#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-15.bash"
--- current list of alias ---
alias echo_var='echo "var:\`$var\`"'
alias echo_var_twice='echo_var echo_var'
before echoing variable using alias name
var:`2`var:`2`
after echoing variable using alias name
var:`2`
var:`2`
after echoing variable using alias name
var:`2`
var:`2` echo_var
after echoing variable twice using alias name
var:`2`
var:`2` echo_var
after echoing variable twice using alias name
var:`2`

```

#### explanation
Since the alias named `echo_var` is expanded to a string that does NOT end with whitspace ` `,
when parsing and re-parsing, the alias named `echo_var_twice` will be expaned to `echo "var:\`$var\`" echo_var`
(`echo_var_twice`->`echo_var echo_var`->`echo "var:\`$var\`" echo_var` as the first occurence of alias name will be expanded as the expanded string does NOT end with whitespace)

### Example 16
#### code
`alias-example-16.bash`

```
shopt -s expand_aliases

alias echo_var='echo "var:\`$var\`" '
alias echo_var_twice='echo_var echo_var'

main(){
    echo "--- current list of alias ---"
    alias
    declare -i var=2

    echo "before echoing variable using alias name"
    printf "var:\`%d\`" $var
    
    # use alias
    eval "echo_var"

    echo "after echoing variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    echo_var

    echo "after echoing variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    eval "echo_var_twice"

    echo "after echoing variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""

    echo_var_twice

    echo "after echoing variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""
}

main

shopt -u expand_aliases
```
#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-16.bash"
--- current list of alias ---
alias echo_var='echo "var:\`$var\`" '
alias echo_var_twice='echo_var echo_var'
before echoing variable using alias name
var:`2`var:`2`
after echoing variable using alias name
var:`2`
var:`2`
after echoing variable using alias name
var:`2`
var:`2` echo var:`2`
after echoing variable twice using alias name
var:`2`
var:`2` echo var:`2`
after echoing variable twice using alias name
var:`2`

```
#### explanation
Since the alias named `echo_var` is expanded to a string that ends with whitspace ` `,
when parsing and re-parsing, the alias named `echo_var_twice` will be expaned to `echo "var:\`$var\`"  echo "var:\`$var\`" `
(`echo_var_twice`->`echo_var echo_var`->`echo "var:\`$var\`"  echo "var:\`$var\`" ` as the first occurence of alias name will be expanded as the expanded string ends with whitespace)

### Example 17
#### code
`alias-example-17.bash`

```
shopt -s expand_aliases

alias echo_var='echo "var:\`$var\`" '
alias echo_var_twice='echo_var; echo_var;'

main(){
    echo "--- current list of alias ---"
    alias
    declare -i var=2

    echo "before echoing variable using alias name"
    printf "var:\`%d\`" $var
    
    # use alias
    eval "echo_var"

    echo "after echoing variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    echo_var

    echo "after echoing variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    eval "echo_var_twice"

    echo "after echoing variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""

    echo_var_twice

    echo "after echoing variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""
}

main

shopt -u expand_aliases
```
#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-17.bash"
--- current list of alias ---
alias echo_var='echo "var:\`$var\`" '
alias echo_var_twice='echo_var; echo_var;'
before echoing variable using alias name
var:`2`var:`2`
after echoing variable using alias name
var:`2`
var:`2`
after echoing variable using alias name
var:`2`
var:`2`
var:`2`
after echoing variable twice using alias name
var:`2`
var:`2`
var:`2`
after echoing variable twice using alias name
var:`2`

```
#### explanation
Since the alias named `echo_var` is expanded to a string that ends with whitspace ` `,
when parsing and re-parsing, the alias named `echo_var_twice` will be expaned to `echo "var:\`$var\`" ; echo "var:\`$var\`" ;`
(`echo_var_twice`->`echo_var echo_var`->`echo "var:\`$var\`" ; echo "var:\`$var\`" ;` as the first occurence of alias name will be expanded as the expanded string ends with whitespace)

### Example 18
#### code
`alias-example-18.bash`

```
shopt -s expand_aliases

alias echo_var='echo "var:\`$var\`" '
alias echo_var_twice='var=5; echo_var;'

main(){
    echo "--- current list of alias ---"
    alias
    declare -i var=2

    echo "before echoing variable using alias name"
    printf "var:\`%d\`" $var
    
    # use alias
    eval "echo_var"

    echo "after echoing variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    echo_var

    echo "after echoing variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    eval "echo_var_twice"

    echo "after echoing variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""

    echo_var_twice

    echo "after echoing variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""
}

main

shopt -u expand_aliases
```
#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-18.bash"
--- current list of alias ---
alias echo_var='echo "var:\`$var\`" '
alias echo_var_twice='var=5; echo_var;'
before echoing variable using alias name
var:`2`var:`2`
after echoing variable using alias name
var:`2`
var:`2`
after echoing variable using alias name
var:`2`
var:`5`
after echoing variable twice using alias name
var:`5`
var:`5`
after echoing variable twice using alias name
var:`5`

```
#### explanation
Since the alias named `echo_var` is expanded to a string that ends with whitspace ` `,
when parsing and re-parsing, the alias named `echo_var_twice` will be expaned to `var=5; echo "var:\`$var\`" ;`
(`echo_var_twice`->`var=5; echo_var`->`var=5; echo "var:\`$var\`" ;` as the first occurence of alias name will be expanded as the expanded string ends with whitespace)

### Example 19
#### code
`alias-example-19.bash`

```
shopt -s expand_aliases

GLOBAL_VAR=4

alias set_var='var=$(( GLOBAL_VAR++ )); '
alias set_var_twice='set_var set_var'

main(){
    echo "--- current list of alias ---"
    alias
    local var=2

    echo "before setting variable using alias name"
    printf "var:\`%d\`" $var
    
    # use alias
    eval "set_var"

    echo "after setting variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    set_var

    echo "after setting variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    eval "set_var_twice"

    echo "after setting variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""

    set_var_twice

    echo "after setting variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""

    set_var_twice

    echo "after setting variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""
}

main

shopt -u expand_aliases
```
#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-19.bash"
--- current list of alias ---
alias set_var='var=$(( GLOBAL_VAR++ )); '
alias set_var_twice='set_var set_var'
before setting variable using alias name
var:`2`after setting variable using alias name
var:`4`
after setting variable using alias name
var:`5`
after setting variable twice using alias name
var:`7`
after setting variable twice using alias name
var:`9`
after setting variable twice using alias name
var:`11`

```
#### explanation
Since the alias named `set_var` is expanded to a string that ends with whitspace ` `,
when parsing and re-parsing, the alias named `echo_var_twice` will be expaned to `var=$(( GLOBAL_VAR++ )); var=$(( GLOBAL_VAR++ )); `
(`set_var_twice`->`set_var set_var`->`var=$(( GLOBAL_VAR++ )); var=$(( GLOBAL_VAR++ )); ` as the first occurence of alias name will be expanded as the expanded string ends with whitespace)

### Example 20
#### code
`alias-example-20.bash`

```
shopt -s expand_aliases

GLOBAL_VAR=4

alias set_var='var=$(( GLOBAL_VAR++ )); '
alias set_var_twice='set_var set_var'

main(){
    echo "--- current list of alias ---"
    alias
    local var=2

    echo "before setting variable using alias name"
    printf "var:\`%d\`" $var
    
    # use alias
    GLOBAL_VAR=40
    eval "set_var"

    echo "after setting variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    set_var

    echo "after setting variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    GLOBAL_VAR=80
    eval "set_var_twice"

    echo "after setting variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""

    set_var_twice

    echo "after setting variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""

    set_var_twice

    echo "after setting variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""
}

main

shopt -u expand_aliases
```
#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-20.bash"
--- current list of alias ---
alias set_var='var=$(( GLOBAL_VAR++ )); '
alias set_var_twice='set_var set_var'
before setting variable using alias name
var:`2`after setting variable using alias name
var:`40`
after setting variable using alias name
var:`41`
after setting variable twice using alias name
var:`81`
after setting variable twice using alias name
var:`83`
after setting variable twice using alias name
var:`85`
```
#### explanation
similar to explanation in example 19

### Example 21
#### code
`alias-example-21.bash`

```
shopt -s expand_aliases

GLOBAL_VAR=4

alias set_var='var=$((GLOBAL_VAR++)) '
alias set_var_twice='set_var; GLOBAL_VAR=40; set_var;'

main(){
    echo "--- current list of alias ---"
    alias
    local var=2

    echo "before setting variable using alias name"
    printf "var:\`%d\`" $var
    
    # use alias
    echo "1)---------------------"
    eval "set_var"

    echo "after setting variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    set_var

    echo "after setting variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    eval "set_var_twice"

    echo "after setting variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""

    set_var_twice

    echo "after setting variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""
    
    echo "2)---------------------"
    eval "set_var"

    echo "after setting variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    set_var

    echo "after setting variable using alias name"
    printf "var:\`%d\`" $var
    echo ""

    eval "set_var_twice"

    echo "after setting variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""

    set_var_twice

    echo "after setting variable twice using alias name"
    printf "var:\`%d\`" $var
    echo ""
}

main

shopt -u expand_aliases
```
#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-21.bash"
--- current list of alias ---
alias set_var='var=$((GLOBAL_VAR++)) '
alias set_var_twice='set_var; GLOBAL_VAR=40; set_var;'
before setting variable using alias name
var:`2`1)---------------------
after setting variable using alias name
var:`4`
after setting variable using alias name
var:`5`
after setting variable twice using alias name
var:`40`
after setting variable twice using alias name
var:`40`
2)---------------------
after setting variable using alias name
var:`41`
after setting variable using alias name
var:`42`
after setting variable twice using alias name
var:`40`
after setting variable twice using alias name
var:`40`

```
#### explanation
similar to explanation in example 19

### Example 22
#### code
`alias-example-22.bash`

```
shopt -s expand_aliases

alias alias_unset='unset '
alias command_alias_unset='command alias_unset '
alias alias_command_unset='command unset '

UNSET_HISTORY_ARRAY_STR="" # define a new empty array represented as string

## override an existing command `unset`
## suppose:
## the first positional argument ALWAYS be a valid identifieer, it NEVER contain whitespace and commas 
function unset(){
    local arg1="$1"
    UNSET_HISTORY_ARRAY_STR="$UNSET_HISTORY_ARRAY_STR,$arg1"
    # call non-overridden existing command `unset`
    command unset $arg1
} 

function print_integer_variable_value(){
    declare -i integer1=$1
    declare -i integer2=$2
    declare -i integer3=$3
    declare -i integer4=$4
    declare -i integer5=$5

    printf "integer1:\`%d\`\n" $integer1
    printf "integer2:\`%d\`\n" $integer2
    printf "integer3:\`%d\`\n" $integer3
    printf "integer4:\`%d\`\n" $integer4
    printf "integer5:\`%d\`\n" $integer5
}

function print_string_variable_value(){
    local str1="$1"
    local str2="$2"
    local str3="$3"
    local str4="$4"
    local str5="$5"

    printf "str1:\`%s\`\n" $str1
    printf "str2:\`%s\`\n" $str2
    printf "str3:\`%s\`\n" $str3
    printf "str4:\`%s\`\n" $str4
    printf "str5:\`%s\`\n" $str5
}

function print_unset_history_array_str(){
    local unset_history_array_str="$1"

    printf "unset_history_array_str:\`%s\`\n" $unset_history_array_str
}

main(){
    declare -i integer1=1
    declare -i integer2=2
    declare -i integer3=3
    declare -i integer4=4
    declare -i integer5=5

    local str1="string1"
    local str2="string2"
    local str3="string3"
    local str4="string4"
    local str5="string5"

    echo "--- current list of alias ---"
    alias

    echo "before unsetting variable using alias name"
    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    echo ""

    # use alias

    echo "1)---------------------"
    eval "alias_unset integer1"
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""

    echo "2)---------------------"
    eval "command unset integer2"
    echo "after unsetting variable without using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""

    echo "3)---------------------"
    eval "command_alias_unset integer3"
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""
    
    echo "4)---------------------"
    eval "alias_command_unset integer4"
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""
    
    echo "5)---------------------"
    eval "unset integer5"
    echo "after unsetting variable without using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""

    echo "6)---------------------"
    alias_unset str1
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""

    echo "7)---------------------"
    command unset str2
    echo "after unsetting variable without using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""

    echo "8)---------------------"
    command_alias_unset str3
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""
    
    echo "9)---------------------"
    alias_command_unset str4
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""
    
    echo "10)---------------------"
    unset str5
    echo "after unsetting variable without using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""
}

main

shopt -u expand_aliases
```
#### output
executing this script will throw errors and echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-22.bash"
--- current list of alias ---
alias alias_command_unset='command unset '
alias alias_unset='unset '
alias command_alias_unset='command alias_unset '
before unsetting variable using alias name
integer1:`1`
integer2:`2`
integer3:`3`
integer4:`4`
integer5:`5`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`

1)---------------------
after unsetting variable using alias name
integer1:`2`
integer2:`3`
integer3:`4`
integer4:`5`
integer5:`0`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`
unset_history_array_str:`,integer1`

2)---------------------
after unsetting variable without using alias name
integer1:`3`
integer2:`4`
integer3:`5`
integer4:`0`
integer5:`0`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`
unset_history_array_str:`,integer1`

3)---------------------
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-22.bash: line 95: alias_unset: command not found
after unsetting variable using alias name
integer1:`3`
integer2:`4`
integer3:`5`
integer4:`0`
integer5:`0`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`
unset_history_array_str:`,integer1`

4)---------------------
after unsetting variable using alias name
integer1:`3`
integer2:`5`
integer3:`0`
integer4:`0`
integer5:`0`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`
unset_history_array_str:`,integer1`

5)---------------------
after unsetting variable without using alias name
integer1:`3`
integer2:`0`
integer3:`0`
integer4:`0`
integer5:`0`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`
unset_history_array_str:`,integer1,integer5`

6)---------------------
after unsetting variable using alias name
integer1:`3`
integer2:`0`
integer3:`0`
integer4:`0`
integer5:`0`
str1:`string2`
str2:`string3`
str3:`string4`
str4:`string5`
str5:``
unset_history_array_str:`,integer1,integer5,str1`

7)---------------------
after unsetting variable without using alias name
integer1:`3`
integer2:`0`
integer3:`0`
integer4:`0`
integer5:`0`
str1:`string3`
str2:`string4`
str3:`string5`
str4:``
str5:``
unset_history_array_str:`,integer1,integer5,str1`

8)---------------------
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-22.bash: line 140: alias_unset: command not found
after unsetting variable using alias name
integer1:`3`
integer2:`0`
integer3:`0`
integer4:`0`
integer5:`0`
str1:`string3`
str2:`string4`
str3:`string5`
str4:``
str5:``
unset_history_array_str:`,integer1,integer5,str1`

9)---------------------
after unsetting variable using alias name
integer1:`3`
integer2:`0`
integer3:`0`
integer4:`0`
integer5:`0`
str1:`string3`
str2:`string5`
str3:``
str4:``
str5:``
unset_history_array_str:`,integer1,integer5,str1`

10)---------------------
after unsetting variable without using alias name
integer1:`3`
integer2:`0`
integer3:`0`
integer4:`0`
integer5:`0`
str1:`string3`
str2:``
str3:``
str4:``
str5:``
unset_history_array_str:`,integer1,integer5,str1,str5`


```
#### explanation
1. About `alias_unset: command not found` error,

the error occurs due to `command alias_unset` will NOT be expanded by Bash engine parser as command substitution has higher precedence than alias substitution (see [precedence in CH34](https://github.com/40843245/terminal-tutorial/blob/main/terminal/Bash%20terminal/tutorial/CH34%20--%20parsing%20rule%20of%20aliasing%20names.md#4-precedence-and-overriding) for more details)

`command_alias_unset`-> `command alias_unset `-> executing `alias_unset`-> throw `alias_unset: command not found` error

2. About `integer4` is NOT contained in `UNSET_HISTORY_ARRAY_STR` global variable after `eval "alias_command_unset integer4"`,

due to the statement executes `unset` built-in command rather than  executing the user-defined function named `unset`

When executing `eval "alias_command_unset integer4`, it will force Bash engine to re-parse with argument `alias_command_unset integer4`,

Then `alias_command_unset` is expanded to `command unset`, thus executing `command unset integer4`, and thus simply unset integer4 the user-defined ,as `command {command-name}` built-in command executes `{command-name}` built-in command (if `{command-name}` is a built-in command) first, rather than executing the user-defined function first. Therefore, executing the statement will NOT add `integer4` into `UNSET_HISTORY_ARRAY_STR` global variable.

executing `eval "alias_command_unset integer4"`-> re-parsing `alias_command_unset integer4` -> executing `command unset integer4` -> simply unset `integer4` -> NOT adding `integer4` into `UNSET_HISTORY_ARRAY_STR` global variable

### Example 23
#### code
`alias-example-23.bash`

```
shopt -s expand_aliases

alias alias_unset='unset '
alias command_alias_unset='command alias_unset '
alias command_eval_alias_unset='command eval "alias_unset '
alias alias_command_unset='command unset '

UNSET_HISTORY_ARRAY_STR="" # define a new empty array represented as string

## override an existing command `unset`
## suppose:
## the first positional argument ALWAYS be a valid identifieer, it NEVER contain whitespace and commas 
function unset(){
    local arg1="$1"
    UNSET_HISTORY_ARRAY_STR="$UNSET_HISTORY_ARRAY_STR,$arg1"
    # call non-overridden existing command `unset`
    command unset arg1
} 

function print_integer_variable_value(){
    declare -i integer1=$1
    declare -i integer2=$2
    declare -i integer3=$3
    declare -i integer4=$4
    declare -i integer5=$5
    declare -i integer6=$6
    declare -i integer7=$7

    printf "integer1:\`%d\`\n" $integer1
    printf "integer2:\`%d\`\n" $integer2
    printf "integer3:\`%d\`\n" $integer3
    printf "integer4:\`%d\`\n" $integer4
    printf "integer5:\`%d\`\n" $integer5
    printf "integer6:\`%d\`\n" $integer6
    printf "integer7:\`%d\`\n" $integer7
}

function print_string_variable_value(){
    local str1="$1"
    local str2="$2"
    local str3="$3"
    local str4="$4"
    local str5="$5"

    printf "str1:\`%s\`\n" $str1
    printf "str2:\`%s\`\n" $str2
    printf "str3:\`%s\`\n" $str3
    printf "str4:\`%s\`\n" $str4
    printf "str5:\`%s\`\n" $str5
}

function print_unset_history_array_str(){
    local unset_history_array_str="$1"

    printf "unset_history_array_str:\`%s\`\n" $unset_history_array_str
}

main(){
    declare -i integer1=1
    declare -i integer2=2
    declare -i integer3=3
    declare -i integer4=4
    declare -i integer5=5
    declare -i integer6=6
    declare -i integer7=7

    local str1="string1"
    local str2="string2"
    local str3="string3"
    local str4="string4"
    local str5="string5"

    echo "--- current list of alias ---"
    alias

    echo "before unsetting variable using alias name"
    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5 $integer6 $integer7
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    echo ""

    # use alias

    echo "1)---------------------"
    eval "alias_unset integer1"
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5 $integer6 $integer7
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""

    echo "2)---------------------"
    eval "command unset integer2"
    echo "after unsetting variable without using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5 $integer6 $integer7
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""

    echo "3)---------------------"
    eval "command_alias_unset integer3"
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5 $integer6 $integer7
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""
    
    echo "3.1)---------------------"
    eval "command_eval_alias_unset integer6\""
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5 $integer6 $integer7
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""

    echo "3.2)---------------------"
    eval "eval \"command_alias_unset\" integer7"
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5 $integer6 $integer7
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""

    echo "4)---------------------"
    eval "alias_command_unset integer4"
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5 $integer6 $integer7
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""
    
    echo "5)---------------------"
    eval "unset integer5"
    echo "after unsetting variable without using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5 $integer6 $integer7
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""

    echo "6)---------------------"
    alias_unset str1
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5 $integer6 $integer7
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""

    echo "7)---------------------"
    command unset str2
    echo "after unsetting variable without using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5 $integer6 $integer7
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""

    echo "8)---------------------"
    command_alias_unset str3
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5 $integer6 $integer7
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""
    
    echo "9)---------------------"
    alias_command_unset str4
    echo "after unsetting variable using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5 $integer6 $integer7
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""
    
    echo "10)---------------------"
    unset str5
    echo "after unsetting variable without using alias name"

    print_integer_variable_value $integer1 $integer2 $integer3 $integer4 $integer5 $integer6 $integer7
    print_string_variable_value $str1 $str2 $str3 $str4 $str5
    print_unset_history_array_str "$UNSET_HISTORY_ARRAY_STR"
    echo ""
}

main

shopt -u expand_aliases
```

#### output
executing this script will throw errors and echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-23.bash"
--- current list of alias ---
alias alias_command_unset='command unset '
alias alias_unset='unset '
alias command_alias_unset='command alias_unset '
alias command_eval_alias_unset='command eval "alias_unset '
before unsetting variable using alias name
integer1:`1`
integer2:`2`
integer3:`3`
integer4:`4`
integer5:`5`
integer6:`6`
integer7:`7`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`

1)---------------------
after unsetting variable using alias name
integer1:`1`
integer2:`2`
integer3:`3`
integer4:`4`
integer5:`5`
integer6:`6`
integer7:`7`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`
unset_history_array_str:`,integer1`

2)---------------------
after unsetting variable without using alias name
integer1:`1`
integer2:`3`
integer3:`4`
integer4:`5`
integer5:`6`
integer6:`7`
integer7:`0`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`
unset_history_array_str:`,integer1`

3)---------------------
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-23.bash: line 102: alias_unset: command not found
after unsetting variable using alias name
integer1:`1`
integer2:`3`
integer3:`4`
integer4:`5`
integer5:`6`
integer6:`7`
integer7:`0`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`
unset_history_array_str:`,integer1`

3.1)---------------------
after unsetting variable using alias name
integer1:`1`
integer2:`3`
integer3:`4`
integer4:`5`
integer5:`6`
integer6:`7`
integer7:`0`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`
unset_history_array_str:`,integer1,integer6`

3.2)---------------------
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-23.bash: line 120: alias_unset: command not found
after unsetting variable using alias name
integer1:`1`
integer2:`3`
integer3:`4`
integer4:`5`
integer5:`6`
integer6:`7`
integer7:`0`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`
unset_history_array_str:`,integer1,integer6`

4)---------------------
after unsetting variable using alias name
integer1:`1`
integer2:`3`
integer3:`5`
integer4:`6`
integer5:`7`
integer6:`0`
integer7:`0`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`
unset_history_array_str:`,integer1,integer6`

5)---------------------
after unsetting variable without using alias name
integer1:`1`
integer2:`3`
integer3:`5`
integer4:`6`
integer5:`7`
integer6:`0`
integer7:`0`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`
unset_history_array_str:`,integer1,integer6,integer5`

6)---------------------
after unsetting variable using alias name
integer1:`1`
integer2:`3`
integer3:`5`
integer4:`6`
integer5:`7`
integer6:`0`
integer7:`0`
str1:`string1`
str2:`string2`
str3:`string3`
str4:`string4`
str5:`string5`
unset_history_array_str:`,integer1,integer6,integer5,str1`

7)---------------------
after unsetting variable without using alias name
integer1:`1`
integer2:`3`
integer3:`5`
integer4:`6`
integer5:`7`
integer6:`0`
integer7:`0`
str1:`string1`
str2:`string3`
str3:`string4`
str4:`string5`
str5:``
unset_history_array_str:`,integer1,integer6,integer5,str1`

8)---------------------
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-23.bash: line 165: alias_unset: command not found
after unsetting variable using alias name
integer1:`1`
integer2:`3`
integer3:`5`
integer4:`6`
integer5:`7`
integer6:`0`
integer7:`0`
str1:`string1`
str2:`string3`
str3:`string4`
str4:`string5`
str5:``
unset_history_array_str:`,integer1,integer6,integer5,str1`

9)---------------------
after unsetting variable using alias name
integer1:`1`
integer2:`3`
integer3:`5`
integer4:`6`
integer5:`7`
integer6:`0`
integer7:`0`
str1:`string1`
str2:`string3`
str3:`string5`
str4:``
str5:``
unset_history_array_str:`,integer1,integer6,integer5,str1`

10)---------------------
after unsetting variable without using alias name
integer1:`1`
integer2:`3`
integer3:`5`
integer4:`6`
integer5:`7`
integer6:`0`
integer7:`0`
str1:`string1`
str2:`string3`
str3:`string5`
str4:``
str5:``
unset_history_array_str:`,integer1,integer6,integer5,str1,str5`


```
#### explanation
See 1th and 2th point of explanation in example 22 for similar issue.

1. About executing `eval "command_eval_alias_unset integer6\""` adds `integer6` into `UNSET_HISTORY_ARRAY_STR` global variable,

When executing `eval "command_eval_alias_unset integer6\""`, it will force Bash engine to re-parse with argument `command_eval_alias_unset integer6\"`. `command_eval_alias_unset integer6\"` is expanded to `command eval "alias_unset integer6"`,

Then it will force Bash engine to re-parse with argument `alias_unset integer6`. `alias_unset integer6` is expanded to `unset integer6`, thus executing user-defined function `unset`, adding `integer6` into `UNSET_HISTORY_ARRAY_STR` global variable.

executing `eval "command_eval_alias_unset integer6\""` -> re-parse with argument `command_eval_alias_unset integer6\"` -> executing `command eval "alias_unset integer6"` -> re-parse with argument `alias_unset integer6` -> executing user-defined function `unset` with argument integer6` ->
adding `integer6` into `UNSET_HISTORY_ARRAY_STR` global variable.

### Example 24
#### code
`alias-example-24.bash`

```
shopt -s expand_aliases
set +e

alias alias_of_define_user_defined_function='
function func1(){
    local -n counter=$1

    echo "++++++++++++++++++++++++++"
    echo "In ${FUNCNAME[0]} function,"
    echo "The counter is \`$counter\`,the passed argument is \`\"$counter\"\`" 
    ((counter++))
    echo "End of ${FUNCNAME[0]} function"
    echo "++++++++++++++++++++++++++"
}
'
alias define_user_defined_function_using_alias_name='alias_of_define_user_defined_function '
alias call_alias_of_define_user_defined_function_without_arguments='func1 '
alias call_alias_of_define_user_defined_function_with_arguments='func1 $counter'

function subfunction(){
    declare -i counter=$1
    
    echo "0)-------------------------"
    echo "before calling function using alias name"
    printf "counter:\`%d\`\n" $counter

    echo "1)-------------------------"
    echo "after calling function using alias name"
    eval "call_alias_of_define_user_defined_function_without_arguments $counter"
    printf "counter:\`%d\`\n" $counter

    echo "2)-------------------------"
    echo "after calling function using alias name"
    eval "eval \"call_alias_of_define_user_defined_function_without_arguments $counter\" "
    printf "counter:\`%d\`\n" $counter

    echo "3)-------------------------"
    echo "after calling function using alias name"
    eval "call_alias_of_define_user_defined_function_with_arguments "
    printf "counter:\`%d\`\n" $counter

    echo "4)-------------------------"
    echo "after calling function using alias name"
    eval "eval \"call_alias_of_define_user_defined_function_with_arguments\" "
    printf "counter:\`%d\`\n" $counter
}

main(){
    subfunction 3
}

main

set -e
shopt -u expand_aliases
```

#### output
executing this script will throw errors and echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-24.bash"
0)-------------------------
before calling function using alias name
counter:`3`
1)-------------------------
after calling function using alias name
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-24.bash: line 29: func1: command not found
counter:`3`
2)-------------------------
after calling function using alias name
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-24.bash: line 34: func1: command not found
counter:`3`
3)-------------------------
after calling function using alias name
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-24.bash: line 39: func1: command not found
counter:`3`
4)-------------------------
after calling function using alias name
D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-24.bash: line 44: func1: command not found
counter:`3`

```
#### explanation
1. About `func1: command not found` error,

Although `func1` and its function block is the content of an alias named `alias_of_define_user_defined_function`,

`alias_of_define_user_defined_function` and its related alias have NOT been forced to be re-parsed yet. Thus, the `func1` function has NOT defined yet (and NOT stored in the memory),

causing `func1: command not found` error at

```
    eval "call_alias_of_define_user_defined_function_without_arguments $counter"
```

and

```
    eval "eval \"call_alias_of_define_user_defined_function_without_arguments $counter\" "
```

and

```
    eval "call_alias_of_define_user_defined_function_with_arguments "
```

and

```
    eval "eval \"call_alias_of_define_user_defined_function_with_arguments\" "
```

which BOTH will be expanded and invoke `func1`

### Example 25
#### code
`alias-example-25.bash`

```
shopt -s expand_aliases
set +e

alias alias_of_define_user_defined_function='
function func1(){
    local -n counter=$1

    echo "++++++++++++++++++++++++++"
    echo "In ${FUNCNAME[0]} function,"
    echo "The counter is \`$counter\`,the passed argument is \`\"$counter\"\`" 
    ((counter++))
    echo "> [!NOTE]"
    echo "> This is a user defined-function but in an aliased name"
    echo "End of ${FUNCNAME[0]} function"
    echo "++++++++++++++++++++++++++"
}
'
alias define_user_defined_function_using_alias_name='alias_of_define_user_defined_function '
alias call_alias_of_define_user_defined_function_without_arguments='func1 '
alias call_alias_of_define_user_defined_function_with_arguments='func1 $counter'

function func1(){
    declare -i counter=$1
    echo "++++++++++++++++++++++++++"
    echo "In ${FUNCNAME[0]} function,"
    echo "The counter is \`$counter\`,the passed argument is \`\"$counter\"\`" 
    echo "> [!NOTE]"
    echo "> This is a user defined-function"
    echo "End of ${FUNCNAME[0]} function"
    echo "++++++++++++++++++++++++++"
}

function subfunction(){
    declare -i counter=$1
    
    echo "0)-------------------------"
    echo "before calling function using alias name"
    printf "counter:\`%d\`\n" $counter

    echo "1)-------------------------"
    echo "after calling function using alias name"
    eval "call_alias_of_define_user_defined_function_without_arguments $counter"
    printf "counter:\`%d\`\n" $counter

    echo "2)-------------------------"
    echo "after calling function using alias name"
    eval "eval \"call_alias_of_define_user_defined_function_without_arguments $counter\" "
    printf "counter:\`%d\`\n" $counter

    echo "3)-------------------------"
    echo "after calling function using alias name"
    eval "call_alias_of_define_user_defined_function_with_arguments "
    printf "counter:\`%d\`\n" $counter

    echo "4)-------------------------"
    echo "after calling function using alias name"
    eval "eval \"call_alias_of_define_user_defined_function_with_arguments\" "
    printf "counter:\`%d\`\n" $counter

    echo "5)-------------------------"
    echo "after calling function without using alias name"
    func1 $counter
    printf "counter:\`%d\`\n" $counter

    echo "6)-------------------------"
    echo "after calling function without using alias name"
    eval "func1 $counter"
    printf "counter:\`%d\`\n" $counter
}

main(){
    subfunction 3
}

main

set -e
shopt -u expand_aliases
```
#### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\alias\alias-example-25.bash"
0)-------------------------
before calling function using alias name
counter:`3`
1)-------------------------
after calling function using alias name
++++++++++++++++++++++++++
In func1 function,
The counter is `3`,the passed argument is `"3"`
> [!NOTE]
> This is a user defined-function
End of func1 function
++++++++++++++++++++++++++
counter:`3`
2)-------------------------
after calling function using alias name
++++++++++++++++++++++++++
In func1 function,
The counter is `3`,the passed argument is `"3"`
> [!NOTE]
> This is a user defined-function
End of func1 function
++++++++++++++++++++++++++
counter:`3`
3)-------------------------
after calling function using alias name
++++++++++++++++++++++++++
In func1 function,
The counter is `3`,the passed argument is `"3"`
> [!NOTE]
> This is a user defined-function
End of func1 function
++++++++++++++++++++++++++
counter:`3`
4)-------------------------
after calling function using alias name
++++++++++++++++++++++++++
In func1 function,
The counter is `3`,the passed argument is `"3"`
> [!NOTE]
> This is a user defined-function
End of func1 function
++++++++++++++++++++++++++
counter:`3`
5)-------------------------
after calling function without using alias name
++++++++++++++++++++++++++
In func1 function,
The counter is `3`,the passed argument is `"3"`
> [!NOTE]
> This is a user defined-function
End of func1 function
++++++++++++++++++++++++++
counter:`3`
6)-------------------------
after calling function without using alias name
++++++++++++++++++++++++++
In func1 function,
The counter is `3`,the passed argument is `"3"`
> [!NOTE]
> This is a user defined-function
End of func1 function
++++++++++++++++++++++++++
counter:`3`

```
#### explanation
1. After `func1` function call, counter local variable is still be 3.

The reason why it is due to invoking function `func1` that is defined directly rather than defined as content of an alias

Its definition

```
function func1(){
    declare -i counter=$1
    echo "++++++++++++++++++++++++++"
    echo "In ${FUNCNAME[0]} function,"
    echo "The counter is \`$counter\`,the passed argument is \`\"$counter\"\`" 
    echo "> [!NOTE]"
    echo "> This is a user defined-function"
    echo "End of ${FUNCNAME[0]} function"
    echo "++++++++++++++++++++++++++"
}
```

The reason why invoking function `func1` that is defined directly is that
 
    + During top-level parsing phrase (at compiled time), these aliases (including but not be restricted to) `call_alias_of_define_user_defined_function_without_arguments`, `call_alias_of_define_user_defined_function_with_arguments` are inserted into mapping table.
    + When Bash engine is forced to re-parse with argument `call_alias_of_define_user_defined_function_without_arguments $counter` due to execution of `    eval "call_alias_of_define_user_defined_function_without_arguments $counter", these aliases (including but not be restricted to)`call_alias_of_define_user_defined_function_without_arguments`, `call_alias_of_define_user_defined_function_with_arguments` are inserted into mapping table again, overriding the existing one (that is inserted during top-level parsing phrase (at compiled time))
    + `func1` block as content of an alias named `alias_of_define_user_defined_function`has NOT been defined yet since the alias named `alias_of_define_user_defined_function` and its related alias has been NOT re-parsed yet (it is mentioned in explanation of example 24)
    + Thus, when invoking `func1`, it will find that there is ONLY one function named `func1` (the one without postfix increment of counter variable) that has been defined.
    
### Example 26
