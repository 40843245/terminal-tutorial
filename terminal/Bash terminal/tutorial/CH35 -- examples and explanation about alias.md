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

When parsing `shopt -s expand_aliases` statement during this parsing phrase enable the functionality `expand_aliases` which allow to expand the alias during parsing phrase at compile time.

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

Therefore, after 2th parsing phrase, it will be expanded into

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
