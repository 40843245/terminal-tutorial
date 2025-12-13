# CH4 -- expression
## objectives
You will know how to

  + express an expression

## CH4-1 -- introduction of an expression
An expression contains math operation, string operation.

```
{expression_part} := ( ${variable} | {returned_value_by_function}| {expression_part} )
{expression} := {expression_part} {operator} {expression_part}

# ...
```

### Examples
### Example 1
```
integer1 = 2
integer2 = 3

$integer1 + $integer2 # <-- this is an expression
```

#### Example 2
```
integer1 = 2

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

## CH4-2 -- an expression about logical operator

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
