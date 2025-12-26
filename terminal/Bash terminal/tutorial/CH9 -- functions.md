# CH9 -- functions
## objectives
You will know how to

  + define a function
  + inoke a function
  + special variables about functions
  + return an exit code in a function
  + shift positional arguments

Additionally, you will know function call is stored -- function stack trace.

Thus, you can get current name (though there is an another simpler way), the name of top level, and the name of callee that is invoked at at top level.   
    
## CH9-1 -- define a function
It doesn't like that in high-level programming language.

You just can define a function name without parameters.

> [!NOTE]
> To receive parameters, you must use a special variable `$@`

syntax:

```
{function_name}(){compound-command}?{
    # function body
    # defines statements here
}
```

or

```
function {function_name}() {compound-command}?{
    # function body
    # defines statements here
}
```

or

```
function {function_name} {compound-command}?{
    # function body
    # defines statements here
}
```

where

`{compound-command}` is a compound command.

## CH9-2 -- inoke a function
To invoke a function, simply execute the command

```
{function_name} ({option} {arguments_value}?)*
```

where

```
{option}:= {short-option} | {long-option}

{short-option}:= available short option of the user-defined function or command
{long-option}:= available long option of the user-defined function or command
```

> [!IMPORTANT]
> short option always starts with `-` but not start with `--` 
>
> long option always starts with `--`

## CH9-3 -- return an exit code
To return an exit code, simply use `return` keyword.

syntax:

```
return <exit_code>
```

where

`<exit_code>` is a nonnegative integer 0~255.

0 means success (boolean true)

non-zero means failure (boolean false)

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
function func1(){
    echo "The function named \`func1\` is invoked."

    echo "Before calling \`func2\` in the function named \`func1\`"
    func2
    echo "After calling \`func2\` in the function named \`func1\`"
    
    echo "End of the function named \`func1\` call."
}

function func2(){
    echo "The function named \`func2\` is invoked."
    
    echo "Before calling \`func3\` in the function named \`func2\`"
    func3
    echo "After calling \`func3\` in the function named \`func2\`"

    echo "End of the function named \`func2\` call."
}

function func3(){
    echo "The function named \`func3\` is invoked."

    echo "Before calling \`func4\` in the function named \`func3\`"
    func4
    echo "After calling \`func4\` in the function named \`func3\`"

    echo "End of the function named \`func3\` call."
    return 2 
}

function func4(){
    echo "The function named \`func4\` is invoked."

    echo "End of the function named \`func4\` call."
}


main(){
    func1
}

main
```

executing this script will echo

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

## CH9-4 -- special variables about functions
`\@`: all passed arguments separator by first character of `$IFS`.

`$*`: all passed arguments that are expanded as a string. 

When quotated by double quotations, it will expand a string and it will be splitted by first character of `IFS` (which is usually a whitespace) 

`$#`: number of all passed arguments

`$FUNCNAME`: a special variable stores current function stack trace (Like stack trace in `C#`)

`${FUNCNAME[0]}`: the invoking function (if it is access inside a function) or null value (if it is access at top level)

`${FUNCNAME[1]}`: the caller who call the function or null value (if it is access at top level or invoked at top level)

`${0}` or `$0`: shell script name

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

## CH9-5 -- shift positional arguments
### `shift`
built-in command `shift` can shift `n` positional arguments from right to left

where 

`n` MUST be a positive integer.

If `n` is NOT specified, it uses one as default value.

syntax:

```
shift {n}
```

#### Examples
##### Example 1

`shift-example-1.bash`

```
function func1(){
    local args="$@"
    local arg1="$1"
    local arg2="$2"
    local arg3="$3"
    local arg4="$4"
    local arg5="$5"
    local arg6="$6"

    echo "before shifting"

    echo "Arguments that store values before unshifted"
    echo "args:\`$args\`"
    echo "arg1:\`$arg1\`"
    echo "arg2:\`$arg2\`"
    echo "arg3:\`$arg3\`"
    echo "arg4:\`$arg4\`"
    echo "arg5:\`$arg5\`"
    echo "arg6:\`$arg6\`"

    shift 2

    local shifted_args="$@"
    local shifted_arg1="$1"
    local shifted_arg2="$2"
    local shifted_arg3="$3"
    local shifted_arg4="$4"
    local shifted_arg5="$5"
    local shifted_arg6="$6"

    echo "after shifting 2 positional argument from right to left"

    echo "Arguments that store values before unshifted"
    echo "args:\`$args\`"
    echo "arg1:\`$arg1\`"
    echo "arg2:\`$arg2\`"
    echo "arg3:\`$arg3\`"
    echo "arg4:\`$arg4\`"
    echo "arg5:\`$arg5\`"
    echo "arg6:\`$arg6\`"
    
    echo "Arguments that store values after unshifted"
    echo "shifted_args:\`$shifted_args\`"
    echo "shifted_arg1:\`$shifted_arg1\`"
    echo "shifted_arg2:\`$shifted_arg2\`"
    echo "shifted_arg3:\`$shifted_arg3\`"
    echo "shifted_arg4:\`$shifted_arg4\`"
    echo "shifted_arg5:\`$shifted_arg5\`"
    echo "shifted_arg6:\`$shifted_arg6\`"
}

main(){
    func1 "apple" "banana" "orange" "kiwi" "grape" "passionate" "papaya" "peach" "tomato"
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\functions\shift\shift-example-1.bash"
before shifting
Arguments that store values before unshifted
args:`apple banana orange kiwi grape passionate papaya peach tomato`
arg1:`apple`
arg2:`banana`
arg3:`orange`
arg4:`kiwi`
arg5:`grape`
arg6:`passionate`
after shifting 2 positional argument from right to left
Arguments that store values before unshifted
args:`apple banana orange kiwi grape passionate papaya peach tomato`
arg1:`apple`
arg2:`banana`
arg3:`orange`
arg4:`kiwi`
arg5:`grape`
arg6:`passionate`
Arguments that store values after unshifted
shifted_args:`orange kiwi grape passionate papaya peach tomato`
shifted_arg1:`orange`
shifted_arg2:`kiwi`
shifted_arg3:`grape`
shifted_arg4:`passionate`
shifted_arg5:`papaya`
shifted_arg6:`peach`

```

## CH9-6 -- function stack trace
### introduction
Function call stack stores the order of function calls.

