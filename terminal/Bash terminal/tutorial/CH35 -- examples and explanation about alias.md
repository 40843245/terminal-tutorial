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

since Bash engine parses the whole block (explained in `2. The "Parsing Time" Trap`](https://github.com/40843245/terminal-tutorial/blob/main/terminal/Bash%20terminal/tutorial/CH34%20--%20parsing%20rule%20of%20aliasing%20names.md#2-the-parsing-time-trap
