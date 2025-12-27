# CH2 -- variables
## objectives 
You will know how to

  + define a variable
  + define a variable with specific type or specific attributes
  + access a variable
  + assign a value to a variable

Additionally, you will know how to define an indexed array, use different variants of accessing an index array. And so for an associative array. 

## CH2-1 -- define a variable
### define a global variable
> [!IMPORTANT]
> If you don't initalize value when declaring (i.e. just type an identifier),
>
> the variable is set to NULL value.

> [!IMPORTANT]
> When assigning value (including `initializing`)  with assignment operator `=`,
>
> it is NOT allowed to leave any whitespace on the left-side of `=` and on the right-hand side of `=`

> [!IMPORTANT]
> You can't specify the type of variable and attributes when define a variable without `declare` preserved word.
>
> See CH2-2 for more details.

To define a global variable, you can simply assign a value into an identifier, or even just type an identifier

for example,

```
COUNT=1
```

and

```
COUNT
```

### define a local variable
To define a local variable, you have to use `local` preserved word and assign a value into a variable, or even just use `local` preserved word then simply type an identifier

> [!NOTE]
> To prevent the variable is polluted with incidently in a function when invoking it (it usually happens to perform a variable with same name),
>
> It is highly recommended to declare a local variable inside a function as possible as you can.  

> [!IMPORTANT]
> You can't specify the type of variable and attributes when define a variable without `declare` preserved word.
>
> See CH2-2 for more details.

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

## CH2-2 -- define a variable with specific type or specific attributes
Directly assignment of a variable (such as `VAR=2` and `local `var=2`) is impossible to define a variable with either specific type (and thus the variable MUST be string type)

or specific attributes (and thus the variable is assignable).

To define a variable with specific type or specific attributes, you have to define a variable with `declare` preserved word.

syntax:

```
declare {options}? {variable-name}(={inital-value})?
```

where

`{options}`: one or more short option that specifies the type or specific attributes of variable.

> [!NOTE]
> The default scope of a variable that is declared with `declare` preserved word is current scope.
>
> To force declare a global variable with `declare` preserved word inside a function,
>
> you MUST use `-g` short option
>
> see -g section or more details.

### short options
#### -i
To declare a variable is an integer and thus can perform arthimetic operation like C (such as `NUM=$NUM+5`) rather than use extended C++ style (such as `NUM=$(($NUM+5))`), you have to use `-i` short option.

#### -r
To declare a readonly variable (which can't be assigned by assignment operator after initialization), you have to use `-r` short option.

#### -a
To declare a variable is an indexed array (like array in C++), you have to use `-a` short option.

#### -A
To declare a variable is an associative array (like dictionary in C#), you have to use `-A` short option.

#### -n
Declare a variable with `nameref` attribute that references to the name of referenced variable.

All references, assignments, and attribute modifications to name, except for those using or changing the -n attribute itself, are performed on the referenced variable referenced by value of variable name (concept like pointer in C++)

> [!NOTE]
> The `nameref` attribute can't be applied to array variables.

#### -g
To force declare a global variable with `declare` preserved word  inside a function, you have to use `-g` short option.

If you declare a variable with `declare` preserved word inside a function without `-g` option, then the variable can be ONLY used in the function.

#### -x
To export a variable, you can use `-x` short option with `declare` preserved word or `export` built-in command.

They are equivalent

```
export VAR_A="Value A"
```

and 

```
declare -x VAR_A="Value A"
```

### Examples
#### Example

`declare-example-1.bash`

At top level

```
ORIGINAL_VAR=2
declare -n var=ORIGINAL_VAR

echo "Before changing value of var variable"
echo "ORIGINAL_VAR has value $ORIGINAL_VAR"
echo "var has value $var"

var=3

echo "After changing value of var variable"
echo "ORIGINAL_VAR has value $ORIGINAL_VAR"
echo "var has value $var"
```

it defines a global variable named `ORIGINAL_VAR` and initialize it to 2 (string type)

Then it declares a global variable named `var` that points to `ORIGINAL_VAR` variable.

Then changes value of `var` variable is changed to 3 (string type).

Since `var` variable points to `ORIGINAL_VAR` variable,

the value of `ORIGINAL_VAR` variable will be also changed to 3 (string type).

Thus, executing this script will echo

```
Before changing value of var variable
ORIGINAL_VAR has value 2
var has value 2
After changing value of var variable
ORIGINAL_VAR has value 3
var has value 3

```

#### Example 2

`declare-example-2.bash`

```
GLOBAL_VAR=3

function my_function(){
    local local_var_without_declare_preserved_word=7
    declare local_var_with_declare_preserved_word=5
    declare -g forced_global_var_with_declare_preserved_word=2
}

function main(){
    echo "Before invoking my_function"
    echo "GLOBAL_VAR has value \`$GLOBAL_VAR\`"
    echo "forced_global_var_with_declare_preserved_word has value \`$forced_global_var_with_declare_preserved_word\`"
    echo "local_var_with_declare_preserved_word has value \`$local_var_with_declare_preserved_word\`"
    echo "local_var_without_declare_preserved_word has value \`$local_var_without_declare_preserved_word\`"

    my_function
    echo "After invoking my_function"
    echo "GLOBAL_VAR has value \`$GLOBAL_VAR\`"
    echo "forced_global_var_with_declare_preserved_word has value \`$forced_global_var_with_declare_preserved_word\`"
    echo "local_var_with_declare_preserved_word has value \`$local_var_with_declare_preserved_word\`"
    echo "local_var_without_declare_preserved_word has value \`$local_var_without_declare_preserved_word\`"
}

main 
```

executing this script will echo

```
Before invoking my_function
GLOBAL_VAR has value `3`
forced_global_var_with_declare_preserved_word has value ``
local_var_with_declare_preserved_word has value ``
local_var_without_declare_preserved_word has value ``
After invoking my_function
GLOBAL_VAR has value `3`
forced_global_var_with_declare_preserved_word has value `2`
local_var_with_declare_preserved_word has value ``
local_var_without_declare_preserved_word has value ``

```

#### Example 3

`declare-example-3.bash`

```
function main(){
    declare -i local_var_with_declare_preserved_word=3

    echo "Before increasing"
    echo "local_var_with_declare_preserved_word has value \`$local_var_with_declare_preserved_word\`"

    local_var_with_declare_preserved_word=$local_var_with_declare_preserved_word+1
    
    echo "1) After increasing by 1"
    echo "local_var_with_declare_preserved_word has value \`$local_var_with_declare_preserved_word\`"

    local_var_with_declare_preserved_word=$(($local_var_with_declare_preserved_word+1))

    echo "2) After increasing by 1"
    echo "local_var_with_declare_preserved_word has value \`$local_var_with_declare_preserved_word\`"
}

main 
```

executing this script will echo

```
Before increasing
local_var_with_declare_preserved_word has value `3`
1) After increasing by 1
local_var_with_declare_preserved_word has value `4`
2) After increasing by 1
local_var_with_declare_preserved_word has value `5`

```

#### Example 4

```
function main(){
# 步驟 1: 在父 Shell 中定義兩個變數
PARENT_ONLY="I am local"
export ENVIRONMENT_VAR="I am exported"

# 步驟 2: 啟動一個子程序 (sub-shell, 這裡用 bash)
# 並且在子程序中嘗試印出這兩個變數
bash -c 'echo "Child Shell sees:" ; echo "PARENT_ONLY: $PARENT_ONLY" ; echo "ENVIRONMENT_VAR: $ENVIRONMENT_VAR"'
}

main 
```

executing this script will echo

```
Child Shell sees:
PARENT_ONLY:
ENVIRONMENT_VAR: I am exported

```

#### Example 5

```
function main(){
# 步驟 1: 在父 Shell 中定義兩個變數
PARENT_ONLY="I am local"
declare -x ENVIRONMENT_VAR="I am exported"

# 步驟 2: 啟動一個子程序 (sub-shell, 這裡用 bash)
# 並且在子程序中嘗試印出這兩個變數
bash -c 'echo "Child Shell sees:" ; echo "PARENT_ONLY: $PARENT_ONLY" ; echo "ENVIRONMENT_VAR: $ENVIRONMENT_VAR"'
}

main 
```

executing this script will echo

```
Child Shell sees:
PARENT_ONLY:
ENVIRONMENT_VAR: I am exported

```

## CH2-2 -- access a variable

To access a variable value, you have to simply add `$` followed by a variable.

It will perform variable expansion (see CH11 for more details)

For example

```
COUNT=1
echo "$COUNT" # expand the value of variable `COUNT`
```

and 
 
```
COUNT
echo "$COUNT" # expand the value of variable `COUNT` which is Null value (considered as empty string)
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
### assignment
> [!IMPORTANT]
> When assigning value (including `initializing`)  with assignment operator `=`,
>
> it is NOT allowed to leave any whitespace on the left-side of `=` and on the right-hand side of `=`

To assign a value into a variable, just use assignment operator `=`

syntax:

```
{left-hand-side-variable}={right-hand-side-expression}
```

It will assign the value expanded from the expression that is right hand side of `=` into the variable `{left-hand-side-variable}`  

```
COUNT=1 # define COUNT global variable and initialize COUNT as 1  
$COUNT
COUNT=2 # assign 2 to COUNT variable.
```

### compound assignment
Like C, for binary arthimetic operator and binary bitwise operator, the expression can be simplified using compound assignment operator.

A compound assignment operator consists of one of binary arthimetic operator, binary bitwise operator followed by assignment operator `=`

> [!NOTE]
> Like in `C`, it is NOT allowed to leave whitespace between binary arthimetic operator, binary bitwise operator and the followed assignment operator `=`

syntax:

```
{left-hand-side-variable}{part-of-compound-assignment}={right-hand-side-expression}
```

where

```
{part-of-compound-assignment}:= {logical-operator} | {bitwise-operator}

{logical-operator}:= one of binary logical operators # see CH5 for more details
{bitwise-operator}:= one of binary bitwise operators # see CH5 for more details
```

is equivalent to

```
{left-hand-side-variable}={left-hand-side-variable}{part-of-compound-assignment}{right-hand-side-expression}
```

## CH2-5 -- indexed array
### Examples
#### Example 1
##### code
`indexed-array-example-1.bash`

```
## utility function
## 主要目的
## echo 索引陣列的相關資訊
function print_index_array_info(){
    # 使用 -n (Nameref)，func_arr 現在指向傳入的變數名稱
    local -n func_arr=$1
    
    # 檢查該變數是否存在
    if [[ -z ${func_arr+x} && ${#func_arr[@]} -eq 0 ]]; then
        echo "The array \`$1\` is not defined or completely empty."
    else
        echo "there are \`${#func_arr[@]}\` elements in this array"
        # 取得所有索引 (Indices)
        for i in "${!func_arr[@]}"; do
            echo "item at index [$i]: \`${func_arr[$i]}\`"
        done
    fi

    # 列印func_arr這個nameref所參考到的陣列之結構
    declare -p func_arr
}

## utility wrapper belongs to utility function
## 主要目的
## echo 執行傳入的宣告指令，並捕捉 stderr
function try_to_reparse_command() {
    # 使用 ( ) 開啟 Subshell
    (
        # 在子進程中執行 eval，出錯也只會結束這個子進程
        eval "$@" 2>/tmp/bash_err
    )
    
    # 檢查子進程的回傳狀態
    if [ $? -ne 0 ]; then
        echo "Caught Error: $(cat /tmp/bash_err)"
        return 1
    fi
    return 0
}

function demo_function1(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -a index_array_1
    print_index_array_info ""index_array_1""
}

function demo_function2(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -a index_array_1[0]="test"
    print_index_array_info ""index_array_1""
}

function demo_function3(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -a index_array_1[1]="test"
    print_index_array_info ""index_array_1""
}

function demo_function4(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -i index=0
    declare -a index_array_1[$index]="test"
    print_index_array_info ""index_array_1""
}

function demo_function5(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -a index_array_1["1"]="test"
    print_index_array_info ""index_array_1""
}

function demo_function6(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    local subscript="1"
    declare -a index_array_1["subscript"]="test"
    print_index_array_info ""index_array_1""
}

function demo_function7(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    local subscript="1"
    unset subscript
    declare -a index_array_1["subscript"]="test"
    print_index_array_info ""index_array_1""
}

function demo_function8(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    local superscript="2"
    local subscript="superscript"
    declare -a index_array_1["subscript"]="test"
    print_index_array_info ""index_array_1""
}

function demo_function9(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -a index_array_1[-3]=\"test\"" || echo "can't initialize an indexed array given the negative integer"
    print_index_array_info ""index_array_1""
}

function demo_function10(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -a index_array_1[\"-3\"]=\"test\"" || echo "can't initialize an indexed array given the negative integer"
    print_index_array_info ""index_array_1""
}

function demo_function11(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -i subscript=-3
    try_to_reparse_command "declare -a index_array_1[\$\"subscript\"]=\"test\"" || echo "can't initialize an indexed array given the negative integer"
    print_index_array_info ""index_array_1""
}

function demo_function12(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -i subscript=-3
    try_to_reparse_command "declare -a index_array_1[\"subscript\"]=\"test\"" || echo "can't initialize an indexed array given the negative integer"
    print_index_array_info ""index_array_1""
}

function demo_function13(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    local subscript="-3"
    try_to_reparse_command "declare -a index_array_1[\"subscript\"]=\"test\"" || echo "can't initialize an indexed array given the negative integer"
    print_index_array_info ""index_array_1""
}

function demo_function14(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    local subscript="-0"
    declare -a index_array_1[$subscript]="test"
    print_index_array_info ""index_array_1""
}

function demo_function15(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    local subscript="-0"
    declare -a index_array_1["subscript"]="test"
    print_index_array_info ""index_array_1""
}

function demo_function16(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -a index_array_1[\"0.3\"]=\"test\"" || echo "can't parse an float number into an integer"
    print_index_array_info ""index_array_1""
}

function demo_function17(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -a index_array_1[\"0.3f\"]=\"test\"" || echo "can't parse an float number into an integer"
    print_index_array_info ""index_array_1""
}

function demo_function18(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -a index_array_1[\"0.3d\"]=\"test\"" || echo "can't parse an float number into an integer"
    print_index_array_info ""index_array_1""
}

function demo_function19(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -a index_array_1[\"(4)\"]=\"test\"" || echo "invalid initialization of an index array"
    print_index_array_info ""index_array_1""
}

function demo_function20(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -a index_array_1[\"(-1)*(-5)\"]=\"test\"" || echo "invalid initialization of an index array"
    print_index_array_info ""index_array_1""
}

function demo_function21(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -i superscript=2
    declare -i dummy=4
    local subscript="superscript dummy"
    try_to_reparse_command "declare -a index_array_1[\"subscript\"]=\"test\"" || echo "invalid initialization of an index array"
    print_index_array_info ""index_array_1""
}

function demo_function22(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -i superscript=2
    declare -i dummy=4
    try_to_reparse_command "declare -a index_array_1[\"\$((superscript + dummy))\"]=\"test\"" || echo "invalid initialization of an index array"
    print_index_array_info ""index_array_1""
}

main(){
    demo_function1
    demo_function2
    demo_function3
    demo_function4
    demo_function5
    demo_function6
    demo_function7
    demo_function8
    demo_function9
    demo_function10
    demo_function11
    demo_function12
    demo_function13
    demo_function14
    demo_function15
    demo_function16
    demo_function17
    demo_function18
    demo_function19
    demo_function20
    demo_function21
    demo_function22
}

set +e
main
set -e

```

##### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\arrays\indexed array\indexed-array-example-1.bash"
In `demo_function1` function call,
The array `index_array_1` is not defined or completely empty.
declare -n func_arr="index_array_1"
In `demo_function2` function call,
there are `1` elements in this array
item at index [0]: `test`
declare -n func_arr="index_array_1"
In `demo_function3` function call,
there are `1` elements in this array
item at index [1]: `test`
declare -n func_arr="index_array_1"
In `demo_function4` function call,
there are `1` elements in this array
item at index [0]: `test`
declare -n func_arr="index_array_1"
In `demo_function5` function call,
there are `1` elements in this array
item at index [1]: `test`
declare -n func_arr="index_array_1"
In `demo_function6` function call,
there are `1` elements in this array
item at index [1]: `test`
declare -n func_arr="index_array_1"
In `demo_function7` function call,
there are `1` elements in this array
item at index [0]: `test`
declare -n func_arr="index_array_1"
In `demo_function8` function call,
there are `1` elements in this array
item at index [2]: `test`
declare -n func_arr="index_array_1"
In `demo_function9` function call,
Caught Error: D:\workspace\Bash\Bash tutorial\examples\arrays\indexed array\indexed-array-example-1.bash: line 30: index_array_1[-3]: bad array subscript
can't initialize an indexed array given the negative integer
The array `index_array_1` is not defined or completely empty.
declare -n func_arr="index_array_1"
In `demo_function10` function call,
Caught Error: D:\workspace\Bash\Bash tutorial\examples\arrays\indexed array\indexed-array-example-1.bash: line 30: index_array_1[-3]: bad array subscript
can't initialize an indexed array given the negative integer
The array `index_array_1` is not defined or completely empty.
declare -n func_arr="index_array_1"
In `demo_function11` function call,
Caught Error: D:\workspace\Bash\Bash tutorial\examples\arrays\indexed array\indexed-array-example-1.bash: line 30: index_array_1[subscript]: bad array subscript
can't initialize an indexed array given the negative integer
The array `index_array_1` is not defined or completely empty.
declare -n func_arr="index_array_1"
In `demo_function12` function call,
Caught Error: D:\workspace\Bash\Bash tutorial\examples\arrays\indexed array\indexed-array-example-1.bash: line 30: index_array_1[subscript]: bad array subscript
can't initialize an indexed array given the negative integer
The array `index_array_1` is not defined or completely empty.
declare -n func_arr="index_array_1"
In `demo_function13` function call,
Caught Error: D:\workspace\Bash\Bash tutorial\examples\arrays\indexed array\indexed-array-example-1.bash: line 30: index_array_1[subscript]: bad array subscript
can't initialize an indexed array given the negative integer
The array `index_array_1` is not defined or completely empty.
declare -n func_arr="index_array_1"
In `demo_function14` function call,
there are `1` elements in this array
item at index [0]: `test`
declare -n func_arr="index_array_1"
In `demo_function15` function call,
there are `1` elements in this array
item at index [0]: `test`
declare -n func_arr="index_array_1"
In `demo_function16` function call,
Caught Error: D:\workspace\Bash\Bash tutorial\examples\arrays\indexed array\indexed-array-example-1.bash: line 30: 0.3: syntax error: invalid arithmetic operator (error token is ".3")
can't parse an float number into an integer
The array `index_array_1` is not defined or completely empty.
declare -n func_arr="index_array_1"
In `demo_function17` function call,
Caught Error: D:\workspace\Bash\Bash tutorial\examples\arrays\indexed array\indexed-array-example-1.bash: line 30: 0.3f: syntax error: invalid arithmetic operator (error token is ".3f")
can't parse an float number into an integer
The array `index_array_1` is not defined or completely empty.
declare -n func_arr="index_array_1"
In `demo_function18` function call,
Caught Error: D:\workspace\Bash\Bash tutorial\examples\arrays\indexed array\indexed-array-example-1.bash: line 30: 0.3d: syntax error: invalid arithmetic operator (error token is ".3d")
can't parse an float number into an integer
The array `index_array_1` is not defined or completely empty.
declare -n func_arr="index_array_1"
In `demo_function19` function call,
The array `index_array_1` is not defined or completely empty.
declare -n func_arr="index_array_1"
In `demo_function20` function call,
The array `index_array_1` is not defined or completely empty.
declare -n func_arr="index_array_1"
In `demo_function21` function call,
Caught Error: D:\workspace\Bash\Bash tutorial\examples\arrays\indexed array\indexed-array-example-1.bash: line 30: superscript dummy: syntax error in expression (error token is "dummy")
invalid initialization of an index array
The array `index_array_1` is not defined or completely empty.
declare -n func_arr="index_array_1"
In `demo_function22` function call,
The array `index_array_1` is not defined or completely empty.
declare -n func_arr="index_array_1"

```

## CH2-6 -- associative array
### Examples
#### Example 1
##### code
`associative-array-example-1.bash`

```
## utility function
## 主要目的
## echo 索引陣列的相關資訊
function print_associative_array_info(){
    # 使用 -n (Nameref)，func_arr 現在指向傳入的變數名稱
    local -n func_arr=$1
    
    # 檢查該變數是否存在
    if [[ -z ${func_arr+x} && ${#func_arr[@]} -eq 0 ]]; then
        echo "The array \`$1\` is not defined or completely empty."
    else
        echo "there are \`${#func_arr[@]}\` elements in this array"
        # 取得所有索引 (Indices)
        for i in "${!func_arr[@]}"; do
            echo "item at index [$i]: \`${func_arr[$i]}\`"
        done
    fi

    # 列印func_arr這個nameref所參考到的陣列之結構
    declare -p func_arr
}

## utility wrapper belongs to utility function
## 主要目的
## echo 執行傳入的宣告指令，並捕捉 stderr
function try_to_reparse_command() {
    # 使用 ( ) 開啟 Subshell
    (
        # 在子進程中執行 eval，出錯也只會結束這個子進程
        eval "$@" 2>/tmp/bash_err
    )
    
    # 檢查子進程的回傳狀態
    if [ $? -ne 0 ]; then
        echo "Caught Error: $(cat /tmp/bash_err)"
        return 1
    fi
    return 0
}

function demo_function1(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -A associative_array_1
    print_associative_array_info "associative_array_1"
}

function demo_function2(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -A associative_array_1[0]="test"
    print_associative_array_info "associative_array_1"
}

function demo_function3(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -A associative_array_1[1]="test"
    print_associative_array_info "associative_array_1"
}

function demo_function4(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -i index=0
    declare -A associative_array_1[$index]="test"
    print_associative_array_info "associative_array_1"
}

function demo_function5(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -A associative_array_1["1"]="test"
    print_associative_array_info "associative_array_1"
}

function demo_function6(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    local subscript="1"
    declare -A associative_array_1["subscript"]="test"
    print_associative_array_info "associative_array_1"
}

function demo_function7(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    local subscript="1"
    unset subscript
    declare -A associative_array_1["subscript"]="test"
    print_associative_array_info "associative_array_1"
}

function demo_function8(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    local superscript="2"
    local subscript="superscript"
    declare -A associative_array_1["subscript"]="test"
    print_associative_array_info "associative_array_1"
}

function demo_function9(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -A associative_array_1[-3]=\"test\"" || echo "can't initialize an indexed array given the negative integer"
    print_associative_array_info "associative_array_1"
}

function demo_function10(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -A associative_array_1[\"-3\"]=\"test\"" || echo "can't initialize an indexed array given the negative integer"
    print_associative_array_info "associative_array_1"
}

function demo_function11(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -i subscript=-3
    try_to_reparse_command "declare -A associative_array_1[\$\"subscript\"]=\"test\"" || echo "can't initialize an indexed array given the negative integer"
    print_associative_array_info "associative_array_1"
}

function demo_function12(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -i subscript=-3
    try_to_reparse_command "declare -A associative_array_1[\"subscript\"]=\"test\"" || echo "can't initialize an indexed array given the negative integer"
    print_associative_array_info "associative_array_1"
}

function demo_function13(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    local subscript="-3"
    try_to_reparse_command "declare -A associative_array_1[\"subscript\"]=\"test\"" || echo "can't initialize an indexed array given the negative integer"
    print_associative_array_info "associative_array_1"
}

function demo_function14(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    local subscript="-0"
    declare -A associative_array_1[$subscript]="test"
    print_associative_array_info "associative_array_1"
}

function demo_function15(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    local subscript="-0"
    declare -A associative_array_1["subscript"]="test"
    print_associative_array_info "associative_array_1"
}

function demo_function16(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -A associative_array_1[\"0.3\"]=\"test\"" || echo "can't parse an float number into an integer"
    print_associative_array_info "associative_array_1"
}

function demo_function17(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -A associative_array_1[\"0.3f\"]=\"test\"" || echo "can't parse an float number into an integer"
    print_associative_array_info "associative_array_1"
}

function demo_function18(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -A associative_array_1[\"0.3d\"]=\"test\"" || echo "can't parse an float number into an integer"
    print_associative_array_info "associative_array_1"
}

function demo_function19(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -A associative_array_1[\"(4)\"]=\"test\"" || echo "invalid initialization of an index array"
    print_associative_array_info "associative_array_1"
}

function demo_function20(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    try_to_reparse_command "declare -A associative_array_1[\"(-1)*(-5)\"]=\"test\"" || echo "invalid initialization of an index array"
    print_associative_array_info "associative_array_1"
}

function demo_function21(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -i superscript=2
    declare -i dummy=4
    local subscript="superscript dummy"
    try_to_reparse_command "declare -A associative_array_1[\"subscript\"]=\"test\"" || echo "invalid initialization of an index array"
    print_associative_array_info "associative_array_1"
}

function demo_function22(){
    echo "In \`${FUNCNAME[0]}\` function call,"
    declare -i superscript=2
    declare -i dummy=4
    try_to_reparse_command "declare -A associative_array_1[\"\$((superscript + dummy))\"]=\"test\"" || echo "invalid initialization of an associative array"
    print_associative_array_info "associative_array_1"
}

main(){
    demo_function1
    demo_function2
    demo_function3
    demo_function4
    demo_function5
    demo_function6
    demo_function7
    demo_function8
    demo_function9
    demo_function10
    demo_function11
    demo_function12
    demo_function13
    demo_function14
    demo_function15
    demo_function16
    demo_function17
    demo_function18
    demo_function19
    demo_function20
    demo_function21
    demo_function22
}

set +e
main
set -e

```
##### output
executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\arrays\associative array\associative-array-example-1.bash"
In `demo_function1` function call,
The array `associative_array_1` is not defined or completely empty.
declare -n func_arr="associative_array_1"
In `demo_function2` function call,
there are `1` elements in this array
item at index [0]: `test`
declare -n func_arr="associative_array_1"
In `demo_function3` function call,
there are `1` elements in this array
item at index [1]: `test`
declare -n func_arr="associative_array_1"
In `demo_function4` function call,
there are `1` elements in this array
item at index [0]: `test`
declare -n func_arr="associative_array_1"
In `demo_function5` function call,
there are `1` elements in this array
item at index [1]: `test`
declare -n func_arr="associative_array_1"
In `demo_function6` function call,
there are `1` elements in this array
item at index [subscript]: `test`
declare -n func_arr="associative_array_1"
In `demo_function7` function call,
there are `1` elements in this array
item at index [subscript]: `test`
declare -n func_arr="associative_array_1"
In `demo_function8` function call,
there are `1` elements in this array
item at index [subscript]: `test`
declare -n func_arr="associative_array_1"
In `demo_function9` function call,
The array `associative_array_1` is not defined or completely empty.
declare -n func_arr="associative_array_1"
In `demo_function10` function call,
The array `associative_array_1` is not defined or completely empty.
declare -n func_arr="associative_array_1"
In `demo_function11` function call,
The array `associative_array_1` is not defined or completely empty.
declare -n func_arr="associative_array_1"
In `demo_function12` function call,
The array `associative_array_1` is not defined or completely empty.
declare -n func_arr="associative_array_1"
In `demo_function13` function call,
The array `associative_array_1` is not defined or completely empty.
declare -n func_arr="associative_array_1"
In `demo_function14` function call,
there are `1` elements in this array
item at index [-0]: `test`
declare -n func_arr="associative_array_1"
In `demo_function15` function call,
there are `1` elements in this array
item at index [subscript]: `test`
declare -n func_arr="associative_array_1"
In `demo_function16` function call,
The array `associative_array_1` is not defined or completely empty.
declare -n func_arr="associative_array_1"
In `demo_function17` function call,
The array `associative_array_1` is not defined or completely empty.
declare -n func_arr="associative_array_1"
In `demo_function18` function call,
The array `associative_array_1` is not defined or completely empty.
declare -n func_arr="associative_array_1"
In `demo_function19` function call,
The array `associative_array_1` is not defined or completely empty.
declare -n func_arr="associative_array_1"
In `demo_function20` function call,
The array `associative_array_1` is not defined or completely empty.
declare -n func_arr="associative_array_1"
In `demo_function21` function call,
The array `associative_array_1` is not defined or completely empty.
declare -n func_arr="associative_array_1"
In `demo_function22` function call,
The array `associative_array_1` is not defined or completely empty.
declare -n func_arr="associative_array_1"

```
