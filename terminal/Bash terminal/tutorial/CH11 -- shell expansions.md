# CH11 -- shell expansions
## objectives
You will know

    + brace expansion
    + tilde expansion
    + parameter and variable expansion
    + command substitution
    + arithmetic expansion
    + process substitution
    + word splitting
    + filename expansion
    + quote removal
    + precedence of these expansions

## CH11-1 -- precedence of these expansions

The precedence from highest to lowest are as follows

    + brace expansion
    + tilde expansion, parameter and variable expansion, arithmetic expansion (done in inside-to-outside fashion), command substitution (done in left-to-right fashion), process substitution (if system supports it)
    + word splitting
    + filename expansion
    + quote removal

## CH11-2 -- brace expansion
Given a sequence that is wrapped by `{}` and seperated by `,`,  it will perform brace expansions on the sequence,

generating a sequence of string by replacing each element in the string that contains the sequence that is used for brace expansions (i.e. contains `{}`).

### Examples
#### Example 1

`brace-expansion-example-1.bash`

```
echo a{d,c,b}e
```

In above example,

| sequence used for brace expansions | `n`th iteration | element | generated string  in `n`th iteration |
| :-- | :-- | :-- | :-- |
| `{d,c,b}` | 0 | `d` | `ade` |
| `{d,c,b}` | 1 | `c` | `ace` |
| `{d,c,b}` | 2 | `b` | `abe` |

combining them together will get a sequence of string

```
ade ace abe
```

Therefore,

```
a{d,c,b}e
```

will be expanded into

```
ade ace abe
```

And thus, executing this script will echo

```
ade ace abe

```

#### Example 2

```
mkdir -p project/{src,bin,doc}
```

In above example,

| sequence used for brace expansions | `n`th iteration | element | generated string  in `n`th iteration |
| :-- | :-- | :-- | :-- |
| `{src,bin,doc}` | 0 | `src` | `project/src` |
| `{src,bin,doc}` | 1 | `bin` | `project/bin` |
| `{src,bin,doc}` | 2 | `doc` | `project/doc` |

combining them together will get a sequence of string

```
project/src project/bin project/doc
```

Therefore,

```
mkdir -p project/{src,bin,doc}
```

is equivalent to

```
mkdir -p project/src project/bin project/doc
```

executing this script will create these folders respectively

    + `src` folder under `project` folder relative to current path
    + `bin` folder under `project` folder relative to current path
    + `doc` folder under `project` folder relative to current path

#### Example 3

```
echo {1..5}
```

In above example, the `1..5` will be expanded to `1,2,3,4,5`

Thus

```
echo {1..5}
```

is equivalent to

```
echo {1,2,3,4,5}
```

| sequence used for brace expansions | `n`th iteration | element | generated string  in `n`th iteration |
| :-- | :-- | :-- | :-- |
| `{1,2,3,4,5}` | 0 | `1` | `1` |
| `{1,2,3,4,5}` | 1 | `2` | `2` |
| `{1,2,3,4,5}` | 2 | `3` | `3` |
| `{1,2,3,4,5}` | 3 | `4` | `4` |
| `{1,2,3,4,5}` | 4 | `5` | `5` |

Therefore, it is equivalent to

```
echo 1 2 3 4 5
```

executing this script will echo

```
1 2 3 4 5

```

## CH11-3 -- tilde expansion
**`tilde-prefix` definition**

In case of a word begins with an unquoted tilde character (‘~’), all of the characters up to the first unquoted slash (i.e. `/` but execlusive to `'/'` and `"/"`) are considered a `tilde-prefix` (if exists), or all characters are considered as a `tilde-prefix`, if there is no unquoted slash.

for example,

| word | `tilde-prefix` | description |
| :- | :- | :-- |
| `~` | `~` | no first occurence of unquoated slash `/` |
| `~/workspace` | `~` | `/` at second char is the first occurence of unquoated slash `/` |
| `~current-directory/workspace` | `~current-directory` | `/` between `y` and `w` is the first occurence of unquoated slash `/` |
| `~current-directory/workspace/git` | `~current-directory` |  `/` between `y` and `w` is the first occurence of unquoated slash `/` |
| `~current-directory/workspace/git` | `~current-directory` |  `/` between `y` and `w` is the first occurence of unquoated slash `/` |
| `~~current-directory"/"workspace/git` | `~~current-directory"/"workspace` | `/` between `e` and `g` is the first occurence of unquoated slash `/`. Note that `"/"` bwteen `y` and `w` are quoated (by double quotations)  |
| `~~current-directory'/'workspace/git` | `~~current-directory"/"workspace` | `/` between `e` and `g` is the first occurence of unquoated slash `/`. Note that `"/"` bwteen `y` and `w` are quoated (by single quotations)  |
| `parent-directory/~current-directory/workspace/git` | `` | the word does NOT begin with `~` |

Only if a `tilde-prefix` does NOT contain any quotations (i.e. single quotation `'` or double quotations `"`) and backslash `\`, the tilde expansion will be validate.

In the format of

```
~{login-name}/
```

`{login-name}` is the string after the beginning tilde `~` of the `tilde-prefix`

Here if `{login-name}` is an empty string (i.e. **`tilde-prefix` is `~`**), 

then **`~` will be expanded into `$HOME`** (a kind of a system environment path variable)

| user inputs | `tilde-prefix` | `{login-name}` | perform tilde expansion or not | description | 
| :-- | :-- | :-- | :-- | :-- |
| `~` | `~` | `` | yes | your home directory |
| `~` | `~` | `` | yes | your home directory |
| `~john/` | `~john` | `` | yes | john's home directory |
| `~"john"/` | `~"john"` | `"john"` | no | considered as an unexpaned string `~john/` |
| `~ /` | `~` | `` | `` | your home directory followed by a whitespace ` ` then root directory |

### system environment path variable
(If tilde-prefix exists) some of specific tilde-prefix will be expanded into a system environment path variable in Shell

| `tilde-prefix` | system environment path variable | description |
| :-- | :-- | :-- |
| `~` | `$HOME` | your home directory | 
| `~+` | `$PWD` | current working direcotry | 
| `~-` | `$OLDPWD` | old present working direcotry (previous working directory) |

### command
(If tilde-prefix exists) some of specific tilde-prefix will be expanded into a command in Shell

| `tilde-prefix` | command | description |
| :-- | :-- | :-- |
| `~N` | `dirs +N` | expanded as the `N`th of directory stacking trace from left to right | 
| `~+N` | `dirs +N` | expanded as the `N`th of directory stack from left to right | 
| `~-N` | `dirs -N` | expanded as the `N`th of directory stack trace from right to left | 

where

`N` is a nonnegative integer.

> [!NOTE]
> `dirs`: is a shorthand of *dir*ectory *s*tack, it will accessing the `N`th of directory stack (from left to right or from right to left) and expand the result.

> [!TIP]
> see CH16 for more details and usage

## CH11-4 -- parameter and variable expansion
### Examples
#### Example 1
`parameter-expansion-example-1.bash`

```
main(){
    local v=123
    echo ${v-unset}
    echo ${v:-unset-or-null}
    unset v
    echo ${v-unset}
    unset
    v=
    echo ${v-unset}
    echo ${v:-unset-or-null}
}

main
```

executing this script will echo

```
123
123
unset

unset-or-null

```

#### Example 2
`parameter-expansion-example-2.bash`

```
main(){
    local var=123
    unset var
    : ${var=DEFAULT}
    echo $var   
    var=
    : ${var=DEFAULT}
    echo $var
    var=
    : ${var:=DEFAULT}
    echo $var
    unset var
    : ${var:=DEFAULT}
    echo $var
}

main
```

executing this script will echo

```
DEFAULT

DEFAULT
DEFAULT

```

#### Example 3
`parameter-expansion-example-3.bash`

```
main(){
    local var=123
    # sets v as empty string (which can be considered as NULL value)
    var=
    # performs a null or unset safetly check 
    # Since var is an empty string (which can be considered as NULL value) 
    # it throws an user-defined error 
    # `var: var is unset or null`
    # and thus NOT executing the following commands
    : ${var:?var is unset or null} 
    
    # performs an unset safetly check 
    # Since var is not unset
    # it will be expanded as the value of var 
    # and thus echoing empty string
    echo ${var?var is unset}

    # unsets v
    unset var
    : ${var?var is unset}

    # performs a null or unset safetly check 
    # Since var is an empty string (which can be considered as NULL value) 
    # it throws an user-defined error 
    # `var: var is unset or null`
    # and thus NOT executing the following commands
    : ${var:?var is unset or null}

    var=123

    # performs a null or unset safetly check 
    # Since var is neither an empty string (which can be considered as NULL value) nor unset, 
    # it will be expanded as the value of var 
    # and thus echoing `123`
    echo ${var:?var is unset or null}
}

main
```

executing this script will throw an user-defined error at line 10

```
$ "D:\workspace\Bash\Bash tutorial\examples\shell expansions\parameter expansion\parameter-expansion-example-3.bash"
D:\workspace\Bash\Bash tutorial\examples\shell expansions\parameter expansion\parameter-expansion-example-3.bash: line 10: var: var is unset or null

```

#### Example 4
This example illustrates how to get specific part of string (like `string.Substring` method in `C#`) 

`parameter-expansion-example-4.bash`

```
main(){
    local str=01234567890abcdefgh
    echo ${str:7}
    echo ${str:7:0}
    echo ${str:7:2}
    echo ${str:7:-2}
    echo ${str: -7}
    echo ${str: -7:0}
    echo ${str: -7:2}
    echo ${str: -7:-2}

    set -- 01234567890abcdefgh
    echo ${1:7}
    echo ${1:7:0}
    echo ${1:7:-2}
    echo ${1:7:2}
    echo ${1: -7}
    echo ${1: -7:0}
    echo ${1: -7:2}
    echo ${1: -7:-2}
}

main
```

executing this script will echo

```
7890abcdefgh

78
7890abcdef
bcdefgh

bc
bcdef
7890abcdefgh

7890abcdef
78
bcdefgh

bc
bcdef

```

#### Example 5
This example also illustrates how to get specific part of string (like `string.Substring` method in `C#`) 

`parameter-expansion-example-5.bash`

```
main(){
    arr[0]=01234567890abcdefgh
    echo ${arr[0]:7}
    echo ${arr[0]:7:0}
    echo ${arr[0]:7:2}
    echo ${arr[0]:7:-2}
    echo ${arr[0]: -7}
    echo ${arr[0]: -7:0}
    echo ${arr[0]: -7:2}
    echo ${arr[0]: -7:-2}
}

main
```

executing this script will echo

```
7890abcdefgh

78
7890abcdef
bcdefgh

bc
bcdef

```

#### Example 6
This example illustrates how to get specific part of all arguments that are set. 

`parameter-expansion-example-6.bash`

```
main(){
    set -- 1 2 3 4 5 6 7 8 9 0 a b c d e f g h
    echo ${@:7}
    echo ${@:7:0}
    echo ${@:7:2}
    echo ${@:7:-2}
    echo ${@: -7:2}
    echo ${@:0}
    echo ${@:0:2}
    echo ${@: -7:0}
}

main
```


executing this script will throw an error at line 6, terminating the script

```
$ "D:\workspace\Bash\Bash tutorial\examples\shell expansions\parameter expansion\parameter-expansion-example-6.bash"
7 8 9 0 a b c d e f g h

7 8
D:\workspace\Bash\Bash tutorial\examples\shell expansions\parameter expansion\parameter-expansion-example-6.bash: line 6: -2: substring expression < 0

```

#### Example 7
This example also illustrates how to get specific part of all arguments that are set. 

`parameter-expansion-example-7.bash`

```
main(){
    set -- 1 2 3 4 5 6 7 8 9 0 a b c d e f g h
    echo ${@:0}
    echo ${@:0:2}
    echo ${@: -7:0}
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\shell expansions\parameter expansion\parameter-expansion-example-7.bash"
D:\workspace\Bash\Bash tutorial\examples\shell expansions\parameter expansion\parameter-expansion-example-7.bash 1 2 3 4 5 6 7 8 9 0 a b c d e f g h
D:\workspace\Bash\Bash tutorial\examples\shell expansions\parameter expansion\parameter-expansion-example-7.bash 1

```

#### Example 8
This example illustrates how to get specific range of an array

`parameter-expansion-example-8.bash`

```
main(){
    arr=(0 1 2 3 4 5 6 7 8 9 0 a b c d e f g h)
    echo ${arr[@]:7}
    echo ${arr[@]:7:2}
    echo ${arr[@]: -7:2}
    echo ${arr[@]: -7:-2}
    echo ${arr[@]:0}
    echo ${arr[@]:0:2}
    echo ${arr[@]: -7:0}
}

main
```


executing this script will throw an error at line 6, terminating the script

```
$ "D:\workspace\Bash\Bash tutorial\examples\shell expansions\parameter expansion\parameter-expansion-example-8.bash"
7 8 9 0 a b c d e f g h
7 8
b c
D:\workspace\Bash\Bash tutorial\examples\shell expansions\parameter expansion\parameter-expansion-example-8.bash: line 6: -2: substring expression < 0

```

#### Example 9
This example also illustrates how to get specific range of an array

`parameter-expansion-example-9.bash`

```
main(){
    arr=(0 1 2 3 4 5 6 7 8 9 0 a b c d e f g h)
    echo ${arr[@]:0}
    echo ${arr[@]:0:2}
    echo ${arr[@]: -7:0}
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\shell expansions\parameter expansion\parameter-expansion-example-9.bash"
0 1 2 3 4 5 6 7 8 9 0 a b c d e f g h
0 1

```

#### Example 10

`pattern-removal-example-1.bash`

```
main(){
    local file_name="image.jpg.png"
    
    ## pattern removal
    ## `${parameter#word}`
    ## remove the shortest prefix
    echo "${file_name#*.}"

    ## pattern removal
    ## `${parameter##word}`
    ## remove the longest prefix
    echo "${file_name##*.}"

    ## pattern removal
    ## `${parameter%word}`
    ## remove the shortest postfix
    echo "${file_name%.*}"

    ## pattern removal
    ## `${parameter%%word}`
    ## remove the longest postfix
    echo "${file_name%%.*}"
}

main
```

executing this script will echo

```
jpg.png
png
image.jpg
image

```
