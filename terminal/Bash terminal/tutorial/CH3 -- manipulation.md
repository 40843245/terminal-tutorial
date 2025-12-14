# CH3 -- manipulation
## objectives
You will know how to 

  + manipulate a number 
  + manipulate a list of string

## CH3-1 -- arithmetic operation
To do an arithmetic operation,

you can simply use exteneded C style spread opeator (if it is an unary expression), or 

use `let` built-in command, or 

use `$(( ... ))` spread operator.

> [!IMPORTANT]
> The obsolete of `$(( ... ))` is `$[]`

### Examples
#### Example 1

To increase `COUNT` global variable by 1,

you can use extended C++ style

```
((COUNT++))
```

also, you can use `let` built-in command

```
let COUNT++
```

also, you can use `$(( ... ))` arithmetically spread operator.

```
COUNT=(($COUNT+1))
```

## CH3-2 -- manipulate a string
To get length of string,

use `$[#<variable-name>]`.

`$[<variable-name>]` expands the variable

`#`:`#<variable-name>` get the length of `<variable-name>`
