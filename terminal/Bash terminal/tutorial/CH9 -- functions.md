# CH9 -- functions
## objectives
You will know how to

  + define a function
  + inoke a function
  + special variables about functions
  + return in a function
    
## CH9-1 -- define a function
It doesn't like that in high-level programming language.

You just can define a function name without parameters.

> [!NOTE]
> To receive parameters, you must use a special variable `$@`

syntax:

```
<function_name>() {compound-command}?{
    # function body
    # defines statements here
}
```

or

```
function <function_name>() {compound-command}?{
    # function body
    # defines statements here
}
```

or

```
function <function_name> {compound-command}?{
    # function body
    # defines statements here
}
```

where

`{compound-command}` is a compound command.

## CH9-2 -- inoke a function
To invoke a function, simply execute the command

```
<function_name> (<flags> <arguments_value>?)*
```

## CH9-3 -- return an exit code
To return an exit code, simply use `return` keyword.

syntax:

```
return <exit_code>
```

where

`<exit_code>` is a nonnegative integer 0~255.

> [!IMPORTANT]
> You can ONLY return an exit code an nonnegative integer 0~255. 
>
> You can't return a string.
>
> However, you can assign the value into a global variable to simulate to return a string.

### Examples
#### Example 1

`return-example-1.bash`

```
$ "D:\workspace\Bash\Bash tutorial\examples\loop\repetitive loop\return\return-example-1.bash"
The function named `func1` is invoked.
Before calling `func2` in the function named `func1`
The function named `func2` is invoked.
Before calling `func3` in the function named `func2`
The function named `func3` is invoked.
Before calling `func4` in the function named `func3`
The function named `func4` is invoked.
End of the function named `func4` call.
After calling `func4` in the function named `func3`
End of the function named `func3` call.
After calling `func3` in the function named `func2`
End of the function named `func2` call.
After calling `func2` in the function named `func1`
End of the function named `func1` call.

```

# CH9-4 -- special variables about functions
CC\`@`: all passed arguments separator by first character o` `$I.

`$*`: all passed arguments that are expanded as a string. 

When quotated by double quotations, it will expand a string and it will be splitted by first character of `IFS` (which is usually a whitespace) 

`$#`: number of all passed arguments

`${0}` or `$0`: the called function name (inside a function) or shell script name (inside a top level of shell script)

`${1}` or `$1`: the first passed argument (zero-based index)

`${2}` or `$2`: the second passed argument (zero-based index)

and so on

`${9}` or `$9`: the nineth passed argument (zero-based index)

`$10`: the second tenth argument (zero-based index)

> [!NOTE]
> `$` followed a positive integer `n` means that expanding the `n`th argument value.

> [!NOTE]
> Since the parser only parses one digit after symbol `$`,
>
> `{}` in `${n}` can't be omitted for all n is greater than or equal to 10.
>
> Otherwise, you will get an unexpected result.


