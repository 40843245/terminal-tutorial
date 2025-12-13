# CH9 -- functions
## objectives
You will know how to

  + define a function
  + inoke a function
  + special variables about functions

## CH9-1 -- define a function
It doesn't like that in high-level programming language.

You just can define a function name without parameters.

> [!NOTE]
> To receive parameters, you must use a special variable `$@`

syntax:

```
<function_name>(){
    # function body
    # defines statements here
}
```

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

## CH9-4 -- special variables about functions
`$@`: all passed arguments

`$*`: all passed arguments that are expanded as a string. 

When quotated by double quotations, it will expand a string and it will be splitted by first character of `IFS` (which is usually a whitespace) 

`$#`: number of all passed arguments

`$0`: the called function name (inside a function) or shell script name (inside a top level of shell script)

`$1`: the first passed argument (zero-based index)

`$2`: the second passed argument (zero-based index)

and so on

