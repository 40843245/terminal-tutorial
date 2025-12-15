# CH13 -- set and unset a variable
## objectivs
You will know how to 

  + set a value of an variable
  + unset a variable
  + check a variable is unset

## CH13-1 -- set a value of an variable
Once after declaring a variable, the value of the variable is determined, 

even if you don't intialize the variable.

> [!IMPORTANT]
> If you don't initalize value when declaring (i.e. just type an identifier),
>
> the variable is set to NULL value.

After declaring a variable, you can set a value by assign a value with assignment operator `=`.

See [CH2-1 -- define a variable](https://github.com/40843245/terminal-tutorial/blob/main/terminal/Bash%20terminal/tutorial/CH2%20--%20variables.md#ch2-1----define-a-variable) and

[CH2-3 -- assign a value to a variable](https://github.com/40843245/terminal-tutorial/blob/main/terminal/Bash%20terminal/tutorial/CH2%20--%20variables.md#ch2-3----assign-a-value-to-a-variable)

for more details.

## CH13-2 -- unset a variable
### unset
Once the variable is set, you can only unset it by `unset` built-in command.

syntax:

```
unset <variable-name>
```

> [!WARNING]
> NEVER `unset $<variable-name>`.
>
> Otherwise, it will expand the variable-name

> [!IMPORTANT]
> To unset a global variable using `unset` built-in command inside a function will affect the result after invoking it.
>
> But to unset a global variable using `unset` built-in command inside a function will NOT affect the result after invoking it.
>
> see CH2 for more details

## CH13-3 -- check a variable is unset
### expand it through `${<variable-name>+set}`
Expanding it through `${<variable-name>+set}` will check the variable name `<variable-name>` is unset.

If the result is an empty string, it MUST be in unset state (that is it does NOT exist).

Otherwise, it is in set state (BUT its value may be an empty string)

#### Examples
##### Example 1

### expand it through `${<variable-name>-}`
Expanding it through `${<variable-name>-}` will check it the variable name `<variable-name>` is unset or it is an empty string.

If the result is an empty string, it may be in unset state (that is it does NOT exist) or it is in set state (BUT its value is an empty string).

Otherwise, it is in set state (AND its value is NOT an empty string)




