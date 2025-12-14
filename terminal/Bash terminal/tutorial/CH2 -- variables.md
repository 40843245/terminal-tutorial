# CH2 -- variables
## objectives 
You will know how to

  + define a variable
  + access a variable
  + assign a value to a variable

## CH2-1 -- define a variable
To define a global variable, you can simply assign a value into a variable.

```
COUNT=1
```

To define a local variable, you have to use `local` variable and assign a value into a variable.

```
parse_args(){
  local integer1=1
}
```

## CH2-1 -- define a variable
To define a global variable, you can simply assign a value into a variable.

```
COUNT=1
```

To define a local variable, you have to use `local` variable and assign a value into a variable.

```
parse_args(){
  local integer1=1
}
```

## CH2-2 -- access a variable
To access a variable, you have to simply add `$` followed by a variable

```
COUNT=1
$COUNT # access the variable `count`
```

> [!NOTE]
> note that it accesses a variable by dynamic scope.
>
> when accessing the variable, it will find the nearest scope which defines it on the stack trace.

### Examples
#### Example 1
```
func1()
{
  local var='func1 local'
  func2
}

func2()
{
  echo "In func2, var = $var"
}

var=global

func1
```

It will output

```
In func2, var = func1 local
```

(here `$var` in func2 is `func1 local`)

Why?

Let's analyze the script first.

+ It defines a function named `func1`, inside the function,

  it defines a local variable named `var` with value `func1 local`

  then it invokes `func2`

+ It defines a function named `func2`, inside the function.

  it echos a message using `$var`.

+ At top level, it defines a global variable named `var` with value `"global"`

  then invokes `func1`

Next, let's analyze the step of execution of the script, its stack strace, and the information of defined variables.

In the script, it defines a global variable named `var` with value `"global"` then invokes `func1`

Then in `func1` it defines a local variable named `var` with value `func1 local` then it invokes `func2`

In `func2` it echos `In func2, var = $var`

Here is its stack strace, and the information of defined variables.

| stack trace | defines variables | assignment | variable value in context of which | founded by |
| :-- | :-- | :-- | :-- | :-- |
| at top level | global variable `var` | `"global"` (determined at initialization) |  `"global"` in context of `var=global` | `var=global` |
| `func1` | local variable `var` | `"func1 local"` (determined at initialization) | `"func1 local"` in context of `local var='func1 local'`| `local var='func1 local'` |
| `func2` | | | `"func1 local"` in context of `echo "In func2, var = $var"` | `local var='func1 local'` (in `func1`) |

## CH2-3 -- assign a value to a variable
To assign a value into a variable through `=`,

```
COUNT=1 # define COUNT global variable and initialize COUNT as 1  
$COUNT
COUNT=2 # assign 2 to COUNT variable.
```
