# CH2 -- variables
## objectives 
You will know how to

  + define a variable
  + access a variable
  + assign a value to a variable

## CH2-1 -- define a variable
> [!IMPORTANT]
> If you don't initalize value when declaring (i.e. just type an identifier),
>
> the variable is set to NULL value.

> [!IMPORTANT]
> When assigning value (including `initializing`)  with assignment operator `=`,
>
> it is NOT allowed to leave any whitespace on the left-side of `=` and on the right-hand side of `=`

To define a global variable, you can simply assign a value into an identifier, or even just type an identifier

for example,

```
COUNT=1
```

and

```
COUNT
```

To define a local variable, you have to use `local` preserved word and assign a value into a variable, or even just use `local` preserved word then simply type an identifier

> [!NOTE]
> To prevent the variable is polluted with incidently in a function when invoking it (it usually happens to perform a variable with same name),
>
> It is highly recommend to declare a local variable inside a function as possible as you can.  

for example,

```
parse_args(){
  local integer1=1
}
```

and

```
parse_args(){
  local integer1
}
```

## CH2-2 -- access a variable

To access a variable, you have to simply add `$` followed by a variable.

for example

```
COUNT=1
$COUNT # access the variable `COUNT`
```

and 
 
```
COUNT
$COUNT # access the variable `COUNT` which is NULL value
```

> [!NOTE]
> note that it accesses a variable by dynamic scope.
>
> when accessing the variable, it will find the nearest scope which defines it on the stack trace.

### Examples
#### Example 1

`variables-example-1.bash`

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
echo "At top level, before invoking func1, var = $var"
func1
echo "At top level, after invoking func1, var = $var"
```

executing this script will echo

```
At top level, before invoking func1, var = global
In func2, var = func1 local
At top level, after invoking func1, var = global

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

  then echos message

  invokes `func1`

  echos message

Next, let's analyze the step of execution of the script, its stack strace, and the information of defined variables.

In the script, it defines a global variable named `var` with value `"global"` then echos "At top level, before invoking func1, var = $var"

invokes `func1`, echos "At top level, after invoking func1, var = $var"

Then in `func1` it defines a local variable named `var` with value `func1 local` then it invokes `func2`

In `func2` it echos `In func2, var = $var`

Here is its stack strace, and the information of defined variables.

| stack trace | defines variables | assignment |
| :-- | :-- | :-- |
| at top level | global variable `var` | `"global"` (determined at initialization) | 
| `func1` | local variable `var` | `"func1 local"` (determined at initialization) | 
| `func2` | | | `"func1 local"` in context of `echo "In func2, var = $var"` | `local var='func1 local'` (in `func1`) |

| context | variable name | variable value | found at |
| :-- | :-- | :-- | :-- |
| `echo "At top level, before invoking func1, var = $var"` (at top level) | `var` | `"global"` | `var=global` (at top level) |
| `echo "In func2, var = $var"` (inside `func2`)| `var` | `"func1 local"`| `local var='func1 local'` (in `func1`) |
| `echo "At top level, after invoking func1, var = $var"` (at top level) | `var` | `"func1 local"`| `local var='func1 local'` (in `func1`) |

#### Example 2
`variables-example-2.bash`

```
func1()
{
  var='func1 local'
  func2
}

func2()
{
  echo "In func2, var = $var"
}

var=global
echo "At top level, before invoking func1, var = $var"
func1
echo "At top level, after invoking func1, var = $var"
```

executing this script will echo

```
At top level, before invoking func1, var = global
In func2, var = func1 local
At top level, after invoking func1, var = func1 local

```

We can see the variable named `var` is polluted when invoking `func1`.

## CH2-3 -- assign a value to a variable
> [!IMPORTANT]
> When assigning value (including `initializing`)  with assignment operator `=`,
>
> it is NOT allowed to leave any whitespace on the left-side of `=` and on the right-hand side of `=`

To assign a value into a variable, just use assignment operator `=`

```
COUNT=1 # define COUNT global variable and initialize COUNT as 1  
$COUNT
COUNT=2 # assign 2 to COUNT variable.
```
