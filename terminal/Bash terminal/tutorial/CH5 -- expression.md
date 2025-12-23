# CH5 -- expression
## objectives
You will know how to

  + express an expression
  + perform an arithmetic operation
  + get length of string

## CH5-1 -- introduction of an expression
An expression contains math operation, string operation.

```
{expression_part} := ( ${variable} | {returned_value_by_function}| {expression_part} )
{expression} := {expression_part} {operator} {expression_part}

# ... etc
```

### Examples
#### Example 1
```
declare -i integer1 = 2
declare -i integer2 = 3

$(integer1 + integer2) # <-- this is an expression
```

#### Example 2
```
declare -i integer1 = 2

$integer1 # <-- this is an expression
```

#### Example 3
```
TOTAL = 0
# sum of two integers
sum(){
  if [["$#" -ne 2]]; then
    echo "the number of arguments MUST be exactly 2;">&2
    return 1
  fi
  local $integer1 = "$1";
  local $integer2 = "$2";

  OPTID = 1
  TOTAL = $integer1 + $integer2
  return 0
}

main(){
  sum 1 2 # <-- this is an expression
  sum 2 3 # <-- this is an expression
}
```

## CH5-2 -- an expression about logical operator

| expression | type | description | pros | cons | notes |
| :-- | :-- | :-- | :-- | :-- | :-- |
| `[]` | command | alias of `test` command | | NO features that `[[]]` has | MUST: Always quotate vaiable with double qoutation. |
| `[[]]` | composite command | | <ul><li>No word splitting and globbing</li><li>supports logical operator inside `[[]]`</li><li>support Regex inside `[[]]`</li></ul> | BEST PRACTICE: Always quotate vaiable with double qoutation. |

### Examples
#### Example 1
To check need_to_display variable is equal to "true" and need_to_echo variable is "true"

```
TRUE = "true"
FALSE = "false"
need_to_display = "$TRUE"
need_to_echo = "$TRUE"
```

you can write (BUT NOT RECOMMENDED)

```
need_to_output = [ "$need_to_display" -eq "$TRUE" ] && [ "$need_to_echo" -eq "$TRUE" ]
```

or equivalently (RECOMMENDED)

```
need_to_output = [[ "$need_to_display" -eq "$TRUE" && "$need_to_echo" -eq "$TRUE" ]]
```

#### Example 2
```
TRUE = "true"
FALSE = "false"
need_to_echo = "$TRUE"
displayed_message = "This is a message"
```

you can write (BUT NOT RECOMMENDED)

```
if [ "$need_to_display" -eq "$TRUE" ]; then
  echo "$displayed_message"
fi
```

or equivalently (RECOMMENDED)

```
if [[ "$need_to_display" -eq "$TRUE" ]]; then
  echo "$displayed_message"
fi
```

#### Example 2
```
TRUE = "true"
FALSE = "false"
need_to_echo = "$TRUE"
displayed_message = "This is a message"
```

you can write (BUT NOT RECOMMENDED)

```
if [ "$need_to_display" -eq "$TRUE" ]; then
  echo "$displayed_message"
fi
```

or equivalently (RECOMMENDED)

```
if [[ "$need_to_display" -eq "$TRUE" ]]; then
  echo "$displayed_message"
fi
```

#### Example 3
To match patterns by Regex,

You MUST place `=~` inside `[[]]` rather than inside `[]`

```
# 匹配版本的函式 (version number format:`v<主要版本>.<次要版本>.<修訂版本>`)
matching_version() {

    if [[ "$#" -ne 1 ]]; then
            echo "Error: $func_name requires exactly 1 argument (version number)." >&2
            return 1
    fi
        
    # 匹配 v<主要版本>.<次要版本>.<修訂版本>
    local version_regex="^v([0-9]+)\.([0-9]+)\.([0-9]+)"
    
    if [[ $VERSION_TAG =~ $version_regex ]]; then
        echo "整個匹配: ${BASH_REMATCH[0]}"  # 輸出: v1.2.3
        echo "主要版本: ${BASH_REMATCH[1]}"  # 輸出: 1
        echo "次要版本: ${BASH_REMATCH[2]}"  # 輸出: 2
        echo "修訂版本: ${BASH_REMATCH[3]}"  # 輸出: 3
    else
        echo "this is NOT a version number"
    fi
    
    return 0
}
```

In 

```
[[ $VERSION_TAG =~ $version_regex ]]
```

you can't write it instead

```
[ $VERSION_TAG =~ $version_regex ]
```

## CH5-3 -- arithmetic operation

To do an arithmetic operation,

you can simply use exteneded C++ style (if it is an unary expression), or

use `let` built-in command, or

use `$(( ... ))` arithmetically spread operator.

Some of arithmetic operators like that in `C++`

| type | arithmetic operator | meaning | syntax | description |
| :-- | :-- | :-- | :-- | :-- |
| unary operator | `-` | unary minus | `-{value}` | return the opposite number of `{value}` |
| unary operator | `+` | unary plus | `+{value}` | same as `{value}`|
| unary operator | `++` | prefix increment | `++{variable}` | plus `{variable}` by 1 then return the value of `{variable}` |
| unary operator | `++` | postfix increment | `{variable}++` | return the value of `{variable}` first then plus `{variable}` by 1 |
| unary operator | `--` | prefix decrement | `--{variable}` | subtract `{variable}` by 1 then return the value of `{variable}` |
| unary operator | `--` | postfix decrement | `{variable}--` | return the value of `{variable}` first then subtract `{variable}` by 1 |
| binary operator | `+` | plus two numbers | `{value1}+{value2}` | return the resultant of plus `{value1}` by `{value2}` |
| binary operator | `-` | subtract one number by another number | `{value1}-{value2}` | return the resultant of subtract `{value1}` by `{value2}` |
| binary operator | `*` | multiplication | `{value1}*{value2}` | return the resultant of {value1} times {value2} |
| binary operator | `/` | integer division | `{value1}/{value2}` | return the resultant of dividing `{value2}` by `{value1}` |
| binary operator | `%` | modulus | `{value1}%{value2}` | `{value1}` modulus `{value2}`, take the remainder of `{value2}` from `{value1}`  |
| binary operator | `**` | exponent | `{value1}**{value2}` | `{value1}` power of `{value2}` |

### Examples
#### Example 1

To increase `COUNT` global variable by 1,

you can use extended C++ style


    ((COUNT++))
 

also, you can use `let` built-in command

    let COUNT++
    

also, you can use `$(( ... ))` arithmetically spread operator.

    COUNT=(($COUNT+1))

#### Example 2

`arthimetic-operators-example-1.bash`

```
function perform_binary_arthimetic_operation(){
    declare -i result_after_unary_plus
    declare -i result_after_unary_minus
    declare -i result_after_addition
    declare -i result_after_subtraction
    declare -i result_after_multiplication
    declare -i result_after_integer_division
    declare -i result_after_exponentation
    declare -i result_after_modulus
    
    declare -i value1=$1
    declare -i value2=$2

    result_after_addition=$(( value1 + value2 ))

    echo "=============================================="
    printf "====== perform an binary operation for value:\`%d\` and \`%d\` ======" $value1 $value2
    echo ""

    printf "Plus from \`%d\` to \`%d\` equals to \`%d\`" $value1 $value2 $result_after_addition
    echo ""
    echo ""

    result_after_subtraction=$(( value1 - value2 ))

    printf "Subtracts from \`%d\` to \`%d\` equals to \`%d\`" $value1 $value2 $result_after_subtraction
    echo ""
    echo ""

    result_after_multiplication=$(( value1 * value2 ))

    printf "\`%d\` times \`%d\` equals to \`%d\`" $value1 $value2 $result_after_multiplication
    echo ""
    echo ""

    result_after_integer_division=$(( value1 / value2 ))

    printf "\`%d\` integer divided by \`%d\` equals to \`%d\`" $value1 $value2 $result_after_integer_division
    echo ""
    echo ""

    result_after_exponentation=$(( value1 ** value2 ))

    printf "\`%d\` power of \`%d\` equals to \`%d\`" $value1 $value2 $result_after_exponentation
    echo ""
    echo ""

    result_after_modulus=$(( value1 % value2 ))

    printf "\`%d\` modulus \`%d\` equals to \`%d\`" $value1 $value2 $result_after_modulus
    echo ""
    echo ""   

    echo "=============================================="
}

function perform_unary_arthimetic_operation(){
    declare -i result_after_unary_plus
    declare -i result_after_unary_minus

    declare -i result_after_prefix_increment
    declare -i result_after_postfix_increment
    declare -i result_after_prefix_decrement
    declare -i result_after_postfix_decrement
    
    declare -i value1=$1
    declare -i value1_backup=value1

    echo "=============================================="
    printf "====== perform an unary operation for value:\`%d\` ======" $value1
    echo ""

    echo "Before performing unary plus,"
    printf "value1:\`%d\`" $value1 
    echo ""
    echo ""

    result_after_unary_plus=$((+value1))

    echo "After performing prefix increment,"
    printf "The returned value:\`%d\`" $result_after_prefix_increment
    echo ""
    printf "value1:\`%d\`" $value1 
    echo ""

    echo "Before performing unary minus,"
    printf "value1:\`%d\`" $value1 
    echo ""

    result_after_unary_minus=$((-value1))

    echo "After performing prefix increment,"
    printf "The returned value:\`%d\`" $result_after_unary_minus
    echo ""
    printf "value1:\`%d\`" $value1 
    echo ""
    echo ""

    echo "Before performing prefix increment,"
    printf "value1:\`%d\`" $value1 
    echo ""
    echo ""

    # backup value from `value1` to `value1_backup`
    value1_backup=$((value1))
    
    result_after_prefix_increment=$(( ++value1 ))

    echo "After performing prefix increment,"
    printf "The returned value:\`%d\`" $result_after_prefix_increment
    echo ""
    printf "value1:\`%d\`" $value1 
    echo ""
    echo ""

    # restore value from `value1_backup` to `value1`
    value1=$((value1_backup))

    echo "Before performing postfix increment,"
    printf "value1:\`%d\`" $value1 
    echo ""
    echo ""

    # backup value from `value1` to `value1_backup`
    value1_backup=$((value1))

    result_after_postfix_increment=$(( value1++ ))

    echo "After performing postfix increment,"
    printf "The returned value:\`%d\`" $result_after_postfix_increment
    echo ""
    printf "value1:\`%d\`" $value1 
    echo ""
    echo ""

    # restore value from `value1_backup` to `value1`
    value1=$((value1_backup))

    echo "Before performing prefix decrement,"
    printf "value1:\`%d\`" $value1 
    echo ""
    echo ""

    # backup value from `value1` to `value1_backup`
    value1_backup=$((value1))

    result_after_prefix_increment=$(( --value1 ))

    echo "After performing prefix decrement,"
    printf "The returned value:\`%d\`" $result_after_prefix_increment
    echo ""
    printf "value1:\`%d\`" $value1 
    echo ""
    echo ""

    # restore value from `value1_backup` to `value1`
    value1=$((value1_backup))

    echo "Before performing postfix decrement,"
    printf "value1:\`%d\`" $value1 
    echo ""
    echo ""

    # backup value from `value1` to `value1_backup`
    value1_backup=$((value1))

    result_after_postfix_decrement=$(( --value1 ))

    echo "After performing postfix decrement,"
    printf "The returned value:\`%d\`" $result_after_postfix_decrement
    echo ""
    printf "value1:\`%d\`" $value1 
    echo ""

    # backup value from `value1` to `value1_backup`
    value1_backup=$((value1))

    echo "=============================================="
}

main(){
    perform_unary_arthimetic_operation 2 
    perform_binary_arthimetic_operation 2 3
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\operations\arthimetic operators\arthimetic-operators-example-1.bash"
==============================================
====== perform an unary operation for value:`2` ======
Before performing unary plus,
value1:`2`

After performing prefix increment,
The returned value:`0`
value1:`2`
Before performing unary minus,
value1:`2`
After performing prefix increment,
The returned value:`-2`
value1:`2`

Before performing prefix increment,
value1:`2`

After performing prefix increment,
The returned value:`3`
value1:`3`

Before performing postfix increment,
value1:`2`

After performing postfix increment,
The returned value:`2`
value1:`3`

Before performing prefix decrement,
value1:`2`

After performing prefix decrement,
The returned value:`1`
value1:`1`

Before performing postfix decrement,
value1:`2`

After performing postfix decrement,
The returned value:`1`
value1:`1`
==============================================
==============================================
====== perform an binary operation for value:`2` and `3` ======
Plus from `2` to `3` equals to `5`

Subtracts from `2` to `3` equals to `-1`

`2` times `3` equals to `6`

`2` integer divided by `3` equals to `0`

`2` power of `3` equals to `8`

`2` modulus `3` equals to `2`

==============================================

```

## CH5-3 -- get length of string
To get length of string, just simply use `#` followed by a varible then wrapped with `${}`


### Examples

#### Example 1
To get length of `STR` global variable,

you can 

```
${#"STR"}
```

or 

```
$(( ${#VARIABLE} ))
```

> [!IMPORTANT]
> `${#"STR"}` returns the length of string `STR` and string type.

> [!IMPORTANT]
> `$(( ${#VARIABLE} ))` returns the length of string `STR` and number type.


