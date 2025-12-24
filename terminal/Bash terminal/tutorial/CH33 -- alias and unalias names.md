# CH33 -- alias and unalias names
## objectives
You will know use of `alias` and `unalias` including

  + give it an alias
  + unalias a specific aliased name
  + unalias all aliased names
  + look at aliased names

## notice
> [!WARNING]
> To make Bash engine consider it is a aliased name that needs to be expanded, you have to enable the functionality `expand_aliases`
>
> About how to enable a functionality, see CH3.
>
> see example in notice section of CH34 to learn how to use it.

> [!WARNING]
> To make other scripts be unaffected by **literal text substitutions** with incident,
>
> always disable `alias_expanded` functionality after main script executes.

## CH33-1 -- give it an alias
### `alias`

To define an alias name,

```
alias {aliased-name}='{aliased-content}'
```

It will copy-and-paste the content `{aliased-content}` without wrapped by qoutation (single quotation `''`, double quotation `""`)

when Bash engine treats `{aliased-name}` is an aliased name and thus needs to expand it during the parsing phrase at compiled time 

(see CH34 for the more understanding of parsing with Bash engine)

Then you can use the aliased-name same as using built-in command

```
{aliased-name} {more-things}?
```

where 

`{more-things}` can be anything you want.

or with `eval` built-in command (but has different meaning and might behave differently compared to the above one)

```
eval \"{aliased-name} {more-things}?\" 
```

> [!WARNING]
> `eval` bulit-in command forces Bash engine to re-parse it when it is executed.
>
> For more details, see CH34.

## CH33-2 -- unalias a specific aliased name
### `unalias`

syntax:

```
unalias {aliased-name}
```

where 

`{aliased-name}` is an aliased name.

It will unalias `{aliased-name}`

## CH33-3 -- unalias all aliased names
### `unalias`

syntax:

```
unalias
```

It will unalias all aliased names.

## CH33-4 -- look at aliased names

syntax:

```
alias
```

It will list all aliased names.

## Examples
### Example 1
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
