# CH16 -- directory and directory stack
## objectives
You will know 

  + what is directory
  + how to look at current working directory
  + how to look at previous working directory
  + how to change current working directory
  + what is directory stack
  + how to look at directory stack
  + how to push directory stack
  + how to pop up directory stack

## CH16-1 -- introduction of directory
A directory is a parent of a folder or a file.

If a directory has one child or more children, then it is NOT an empty directory.

Otherwise, it is an empty directory.

### Examples
#### Example 1

Given the following structure, 

```
D:\workspace\Bash\Bash tutorial>tree
Folder PATH listing for volume 新增磁碟區
Volume serial number is <volume-serial-number>
D:.
├─examples
│  ├─command grouping
│  ├─declare
│  ├─getopts
│  ├─IFS
│  ├─joining
│  ├─parsing long options
│  ├─Regex
│  ├─shell expansions
│  │  └─brace expansion
│  ├─status of functionalities
│  ├─unset
│  └─variables
└─utility modules
    ├─functionality enabled
    ├─joining
    ├─Regex
    └─unset
```

The directory `D:\workspace\Bash\Bash tutorial` has two folders named `examples` and `utility modules`.

## CH16-2 -- look at current working directory
### `$PWD`
`$PWD`: a shorthand of *p*resent *w*orking *d*irectory. It will be expanded into current working directory.

## CH16-3 -- look at previous working directory
### `$OLDPWD`
`$OLDPWD`: a shorthand of *old* *p*resent *w*orking *d*irectory. It will be expanded into previous working directory.

## CH16-4 -- change current working directory
### `cd`
The built-in command `cd` can change current directory to specific directory

> [!IMPORTANT]
> Executing `cd` command will
>
> 1. assign value of `$PWD` into `$OLDPWD`
>
> 2. assign specific directory into `$PWD`
>
> 3. change it to specific directory. 


## CH16-5 -- introduction of directory stack
A directory stack is a stack that stores the order of directory path.

It use FILO (First-In-Last-Out)

An example illustrates the directory stack after pushing and popping up.

![directory stack-example2](https://github.com/user-attachments/assets/a2e4bc9b-7599-4a3c-aedc-62659bc7d5ca)

## CH16-6 -- look at directory stack
### `$DIRSTACK`
`$DIRSTACK` will list the directory stack from top to down

It will echo the element that firstly pushed first, separating a break line and so on. 

See following example 1 for more understanding.

### `dir -v`
`dir -v` will echo `"$DIRSTACK"`

## CH16-7 -- push directory stack
### `pushd`
The built-in command `pushd` with given specified directory will push the specified directory into directory stack.


## CH16-8 -- pop up directory stack
### `popd`
The built-in command `popd` will pop up the element at top (i.e. the last element that is pushed), 

then echo the directory stack by echoing the element that firstly pushed first, separating a whitespace and so on. 

See following example 1 for more understanding.

## Examples
### Example 1

`look-at-directory-stack-example-1.bash`

```
main(){
    local base_directory="/d/workspace/Bash/Bash tutorial"
    local examples_directory="/d/workspace/Bash/Bash tutorial/examples"
    local examples_declare_directory="/d/workspace/Bash/Bash tutorial/examples/declare"
    local examples_getopts_directory="/d/workspace/Bash/Bash tutorial/examples/getopts"
    local examples_joining_directory="/d/workspace/Bash/Bash tutorial/examples/joining"
    local utility_modules_directory="/d/workspace/Bash/Bash tutorial/utility modules"
    local utility_modules_unset_directory="/d/workspace/Bash/Bash tutorial/utility modules/unset"

    declare -i index=1
    local temp_working_directory="$PWD"
    
    echo "--------- echo info ---------"
    echo "current directory: $PWD" 

    echo "current directory stack:"
    dirs -v
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$base_directory\`"
    cd "$base_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\`"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    dirs -v
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$examples_directory\` and push direcoty \`$examples_directory\` into directory stack"
    pushd "$examples_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\` and push direcoty \`$examples_directory\` into directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    dirs -v
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$examples_declare_directory\` and push direcoty \`$examples_declare_directory\` into directory stack"
    pushd "$examples_declare_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\` and push direcoty \`$examples_declare_directory\` into directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    dirs -v
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$examples_getopts_directory\` and push direcoty \`$examples_getopts_directory\` into directory stack"
    pushd "$examples_getopts_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\` and push direcoty \`$examples_getopts_directory\` into directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    dirs -v
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$examples_joining_directory\` and push direcoty \`$examples_joining_directory\` into directory stack"
    pushd "$examples_joining_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\` and push direcoty \`$examples_joining_directory\` into directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    dirs -v
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$utility_modules_directory\` and push direcoty \`$utility_modules_directory\` into directory stack"
    pushd "$utility_modules_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\` and push direcoty \`$utility_modules_directory\` into directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    dirs -v
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$utility_modules_unset_directory\` and push direcoty \`$utility_modules_unset_directory\` into directory stack"
    pushd "$utility_modules_unset_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\` and push direcoty \`$utility_modules_unset_directory\` into directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    dirs -v
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to pop up from directory stack"
    popd
    echo "successfully popping up directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "current directory stack:"
    dirs -v
    echo "successfully echoing directory stack"

    echo "--------------------- ${index}th operation ---------------------"
    temp_working_directory=~-2
    echo "Ready to change from current directory \`$PWD\` into new directory -- the top 2th of directory stack"
    cd ~-2
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\`"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------------------- ${index}th operation ---------------------"
    temp_working_directory=~+2
    echo "Ready to change from current directory \`$PWD\` into new directory -- the last 2th of directory stack"
    cd ~+2
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\`"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    dirs -v
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"
}

main
```

executing the script will echo (if the directories used in above example BOTH exist) 

```
$ "D:\workspace\Bash\Bash tutorial\examples\directory stack\look-at-directory-stack-example-1.bash"
--------- echo info ---------
current directory: /d/workspace/Bash/Bash tutorial
current directory stack:
 0  /d/workspace/Bash/Bash tutorial
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 1th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial` into new directory `/d/workspace/Bash/Bash tutorial`
successfully change from old directory `/d/workspace/Bash/Bash tutorial` into current directory `/d/workspace/Bash/Bash tutorial`
--------------------- end of 1th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial
current directory: /d/workspace/Bash/Bash tutorial
current directory stack:
 0  /d/workspace/Bash/Bash tutorial
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 2th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial` into new directory `/d/workspace/Bash/Bash tutorial/examples` and push direcoty `/d/workspace/Bash/Bash tutorial/examples` into directory stack
/d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully change from old directory `/d/workspace/Bash/Bash tutorial` into current directory `/d/workspace/Bash/Bash tutorial/examples` and push direcoty `/d/workspace/Bash/Bash tutorial/examples` into directory stack
--------------------- end of 2th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial
current directory: /d/workspace/Bash/Bash tutorial/examples
current directory stack:
 0  /d/workspace/Bash/Bash tutorial/examples
 1  /d/workspace/Bash/Bash tutorial
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 3th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/examples` into new directory `/d/workspace/Bash/Bash tutorial/examples/declare` and push direcoty `/d/workspace/Bash/Bash tutorial/examples/declare` into directory stack
/d/workspace/Bash/Bash tutorial/examples/declare /d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully change from old directory `/d/workspace/Bash/Bash tutorial/examples` into current directory `/d/workspace/Bash/Bash tutorial/examples/declare` and push direcoty `/d/workspace/Bash/Bash tutorial/examples/declare` into directory stack
--------------------- end of 3th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial/examples
current directory: /d/workspace/Bash/Bash tutorial/examples/declare
current directory stack:
 0  /d/workspace/Bash/Bash tutorial/examples/declare
 1  /d/workspace/Bash/Bash tutorial/examples
 2  /d/workspace/Bash/Bash tutorial
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 4th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/examples/declare` into new directory `/d/workspace/Bash/Bash tutorial/examples/getopts` and push direcoty `/d/workspace/Bash/Bash tutorial/examples/getopts` into directory stack
/d/workspace/Bash/Bash tutorial/examples/getopts /d/workspace/Bash/Bash tutorial/examples/declare /d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully change from old directory `/d/workspace/Bash/Bash tutorial/examples/declare` into current directory `/d/workspace/Bash/Bash tutorial/examples/getopts` and push direcoty `/d/workspace/Bash/Bash tutorial/examples/getopts` into directory stack
--------------------- end of 4th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial/examples/declare
current directory: /d/workspace/Bash/Bash tutorial/examples/getopts
current directory stack:
 0  /d/workspace/Bash/Bash tutorial/examples/getopts
 1  /d/workspace/Bash/Bash tutorial/examples/declare
 2  /d/workspace/Bash/Bash tutorial/examples
 3  /d/workspace/Bash/Bash tutorial
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 5th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/examples/getopts` into new directory `/d/workspace/Bash/Bash tutorial/examples/joining` and push direcoty `/d/workspace/Bash/Bash tutorial/examples/joining` into directory stack
/d/workspace/Bash/Bash tutorial/examples/joining /d/workspace/Bash/Bash tutorial/examples/getopts /d/workspace/Bash/Bash tutorial/examples/declare /d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully change from old directory `/d/workspace/Bash/Bash tutorial/examples/getopts` into current directory `/d/workspace/Bash/Bash tutorial/examples/joining` and push direcoty `/d/workspace/Bash/Bash tutorial/examples/joining` into directory stack
--------------------- end of 5th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial/examples/getopts
current directory: /d/workspace/Bash/Bash tutorial/examples/joining
current directory stack:
 0  /d/workspace/Bash/Bash tutorial/examples/joining
 1  /d/workspace/Bash/Bash tutorial/examples/getopts
 2  /d/workspace/Bash/Bash tutorial/examples/declare
 3  /d/workspace/Bash/Bash tutorial/examples
 4  /d/workspace/Bash/Bash tutorial
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 6th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/examples/joining` into new directory `/d/workspace/Bash/Bash tutorial/utility modules` and push direcoty `/d/workspace/Bash/Bash tutorial/utility modules` into directory stack
/d/workspace/Bash/Bash tutorial/utility modules /d/workspace/Bash/Bash tutorial/examples/joining /d/workspace/Bash/Bash tutorial/examples/getopts /d/workspace/Bash/Bash tutorial/examples/declare /d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully change from old directory `/d/workspace/Bash/Bash tutorial/examples/joining` into current directory `/d/workspace/Bash/Bash tutorial/utility modules` and push direcoty `/d/workspace/Bash/Bash tutorial/utility modules` into directory stack
--------------------- end of 6th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial/examples/joining
current directory: /d/workspace/Bash/Bash tutorial/utility modules
current directory stack:
 0  /d/workspace/Bash/Bash tutorial/utility modules
 1  /d/workspace/Bash/Bash tutorial/examples/joining
 2  /d/workspace/Bash/Bash tutorial/examples/getopts
 3  /d/workspace/Bash/Bash tutorial/examples/declare
 4  /d/workspace/Bash/Bash tutorial/examples
 5  /d/workspace/Bash/Bash tutorial
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 7th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/utility modules` into new directory `/d/workspace/Bash/Bash tutorial/utility modules/unset` and push direcoty `/d/workspace/Bash/Bash tutorial/utility modules/unset` into directory stack
/d/workspace/Bash/Bash tutorial/utility modules/unset /d/workspace/Bash/Bash tutorial/utility modules /d/workspace/Bash/Bash tutorial/examples/joining /d/workspace/Bash/Bash tutorial/examples/getopts /d/workspace/Bash/Bash tutorial/examples/declare /d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully change from old directory `/d/workspace/Bash/Bash tutorial/utility modules` into current directory `/d/workspace/Bash/Bash tutorial/utility modules/unset` and push direcoty `/d/workspace/Bash/Bash tutorial/utility modules/unset` into directory stack
--------------------- end of 7th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial/utility modules
current directory: /d/workspace/Bash/Bash tutorial/utility modules/unset
current directory stack:
 0  /d/workspace/Bash/Bash tutorial/utility modules/unset
 1  /d/workspace/Bash/Bash tutorial/utility modules
 2  /d/workspace/Bash/Bash tutorial/examples/joining
 3  /d/workspace/Bash/Bash tutorial/examples/getopts
 4  /d/workspace/Bash/Bash tutorial/examples/declare
 5  /d/workspace/Bash/Bash tutorial/examples
 6  /d/workspace/Bash/Bash tutorial
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 8th operation ---------------------
Ready to pop up from directory stack
/d/workspace/Bash/Bash tutorial/utility modules /d/workspace/Bash/Bash tutorial/examples/joining /d/workspace/Bash/Bash tutorial/examples/getopts /d/workspace/Bash/Bash tutorial/examples/declare /d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully popping up directory stack
--------------------- end of 8th operation ---------------------
current directory stack:
 0  /d/workspace/Bash/Bash tutorial/utility modules
 1  /d/workspace/Bash/Bash tutorial/examples/joining
 2  /d/workspace/Bash/Bash tutorial/examples/getopts
 3  /d/workspace/Bash/Bash tutorial/examples/declare
 4  /d/workspace/Bash/Bash tutorial/examples
 5  /d/workspace/Bash/Bash tutorial
successfully echoing directory stack
--------------------- 9th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/utility modules` into new directory -- the top 2th of directory stack
successfully change from old directory `/d/workspace/Bash/Bash tutorial/utility modules` into current directory `/d/workspace/Bash/Bash tutorial/examples/declare`
--------------------- end of 9th operation ---------------------
--------------------- 10th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/examples/declare` into new directory -- the last 2th of directory stack
successfully change from old directory `/d/workspace/Bash/Bash tutorial/examples/declare` into current directory `/d/workspace/Bash/Bash tutorial/examples/getopts`
--------------------- end of 10th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial/examples/declare
current directory: /d/workspace/Bash/Bash tutorial/examples/getopts
current directory stack:
 0  /d/workspace/Bash/Bash tutorial/examples/getopts
 1  /d/workspace/Bash/Bash tutorial/examples/joining
 2  /d/workspace/Bash/Bash tutorial/examples/getopts
 3  /d/workspace/Bash/Bash tutorial/examples/declare
 4  /d/workspace/Bash/Bash tutorial/examples
 5  /d/workspace/Bash/Bash tutorial
successfully echoing directory stack
--------- end of echo info ---------

```

### Example 2

`look-at-directory-stack-example-2.bash`

```
main(){
    local base_directory="/d/workspace/Bash/Bash tutorial"
    local examples_directory="/d/workspace/Bash/Bash tutorial/examples"
    local examples_declare_directory="/d/workspace/Bash/Bash tutorial/examples/declare"
    local examples_getopts_directory="/d/workspace/Bash/Bash tutorial/examples/getopts"
    local examples_joining_directory="/d/workspace/Bash/Bash tutorial/examples/joining"
    local utility_modules_directory="/d/workspace/Bash/Bash tutorial/utility modules"
    local utility_modules_unset_directory="/d/workspace/Bash/Bash tutorial/utility modules/unset"

    declare -i index=1
    local temp_working_directory="$PWD"
    
    echo "--------- echo info ---------"
    echo "current directory: $PWD" 

    echo "current directory stack:"
    echo "$DIRSTACK"
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$base_directory\`"
    cd "$base_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\`"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    echo "$DIRSTACK"
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$examples_directory\` and push direcoty \`$examples_directory\` into directory stack"
    pushd "$examples_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\` and push direcoty \`$examples_directory\` into directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    echo "$DIRSTACK"
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$examples_declare_directory\` and push direcoty \`$examples_declare_directory\` into directory stack"
    pushd "$examples_declare_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\` and push direcoty \`$examples_declare_directory\` into directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    echo "$DIRSTACK"
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$examples_getopts_directory\` and push direcoty \`$examples_getopts_directory\` into directory stack"
    pushd "$examples_getopts_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\` and push direcoty \`$examples_getopts_directory\` into directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    echo "$DIRSTACK"
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$examples_joining_directory\` and push direcoty \`$examples_joining_directory\` into directory stack"
    pushd "$examples_joining_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\` and push direcoty \`$examples_joining_directory\` into directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    echo "$DIRSTACK"
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$utility_modules_directory\` and push direcoty \`$utility_modules_directory\` into directory stack"
    pushd "$utility_modules_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\` and push direcoty \`$utility_modules_directory\` into directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    echo "$DIRSTACK"
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to change from current directory \`$PWD\` into new directory \`$utility_modules_unset_directory\` and push direcoty \`$utility_modules_unset_directory\` into directory stack"
    pushd "$utility_modules_unset_directory"
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\` and push direcoty \`$utility_modules_unset_directory\` into directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    echo "$DIRSTACK"
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"

    echo "--------------------- ${index}th operation ---------------------"
    echo "Ready to pop up from directory stack"
    popd
    echo "successfully popping up directory stack"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "current directory stack:"
    echo "$DIRSTACK"
    echo "successfully echoing directory stack"

    echo "--------------------- ${index}th operation ---------------------"
    temp_working_directory=~-2
    echo "Ready to change from current directory \`$PWD\` into new directory -- the top 2th of directory stack"
    cd ~-2
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\`"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------------------- ${index}th operation ---------------------"
    temp_working_directory=~+2
    echo "Ready to change from current directory \`$PWD\` into new directory -- the last 2th of directory stack"
    cd ~+2
    echo "successfully change from old directory \`$OLDPWD\` into current directory \`$PWD\`"
    echo "--------------------- end of ${index}th operation ---------------------"

    ((index++))

    echo "--------- echo info ---------"
    echo "old directory: $OLDPWD" 
    echo "current directory: $PWD" 

    echo "current directory stack:"
    echo "$DIRSTACK"
    echo "successfully echoing directory stack"
    echo "--------- end of echo info ---------"
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\directory stack\look-at-directory-stack-example-2.bash"
--------- echo info ---------
current directory: /d/workspace/Bash/Bash tutorial
current directory stack:
/d/workspace/Bash/Bash tutorial
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 1th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial` into new directory `/d/workspace/Bash/Bash tutorial`
successfully change from old directory `/d/workspace/Bash/Bash tutorial` into current directory `/d/workspace/Bash/Bash tutorial`
--------------------- end of 1th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial
current directory: /d/workspace/Bash/Bash tutorial
current directory stack:
/d/workspace/Bash/Bash tutorial
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 2th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial` into new directory `/d/workspace/Bash/Bash tutorial/examples` and push direcoty `/d/workspace/Bash/Bash tutorial/examples` into directory stack
/d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully change from old directory `/d/workspace/Bash/Bash tutorial` into current directory `/d/workspace/Bash/Bash tutorial/examples` and push direcoty `/d/workspace/Bash/Bash tutorial/examples` into directory stack
--------------------- end of 2th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial
current directory: /d/workspace/Bash/Bash tutorial/examples
current directory stack:
/d/workspace/Bash/Bash tutorial/examples
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 3th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/examples` into new directory `/d/workspace/Bash/Bash tutorial/examples/declare` and push direcoty `/d/workspace/Bash/Bash tutorial/examples/declare` into directory stack
/d/workspace/Bash/Bash tutorial/examples/declare /d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully change from old directory `/d/workspace/Bash/Bash tutorial/examples` into current directory `/d/workspace/Bash/Bash tutorial/examples/declare` and push direcoty `/d/workspace/Bash/Bash tutorial/examples/declare` into directory stack
--------------------- end of 3th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial/examples
current directory: /d/workspace/Bash/Bash tutorial/examples/declare
current directory stack:
/d/workspace/Bash/Bash tutorial/examples/declare
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 4th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/examples/declare` into new directory `/d/workspace/Bash/Bash tutorial/examples/getopts` and push direcoty `/d/workspace/Bash/Bash tutorial/examples/getopts` into directory stack
/d/workspace/Bash/Bash tutorial/examples/getopts /d/workspace/Bash/Bash tutorial/examples/declare /d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully change from old directory `/d/workspace/Bash/Bash tutorial/examples/declare` into current directory `/d/workspace/Bash/Bash tutorial/examples/getopts` and push direcoty `/d/workspace/Bash/Bash tutorial/examples/getopts` into directory stack
--------------------- end of 4th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial/examples/declare
current directory: /d/workspace/Bash/Bash tutorial/examples/getopts
current directory stack:
/d/workspace/Bash/Bash tutorial/examples/getopts
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 5th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/examples/getopts` into new directory `/d/workspace/Bash/Bash tutorial/examples/joining` and push direcoty `/d/workspace/Bash/Bash tutorial/examples/joining` into directory stack
/d/workspace/Bash/Bash tutorial/examples/joining /d/workspace/Bash/Bash tutorial/examples/getopts /d/workspace/Bash/Bash tutorial/examples/declare /d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully change from old directory `/d/workspace/Bash/Bash tutorial/examples/getopts` into current directory `/d/workspace/Bash/Bash tutorial/examples/joining` and push direcoty `/d/workspace/Bash/Bash tutorial/examples/joining` into directory stack
--------------------- end of 5th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial/examples/getopts
current directory: /d/workspace/Bash/Bash tutorial/examples/joining
current directory stack:
/d/workspace/Bash/Bash tutorial/examples/joining
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 6th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/examples/joining` into new directory `/d/workspace/Bash/Bash tutorial/utility modules` and push direcoty `/d/workspace/Bash/Bash tutorial/utility modules` into directory stack
/d/workspace/Bash/Bash tutorial/utility modules /d/workspace/Bash/Bash tutorial/examples/joining /d/workspace/Bash/Bash tutorial/examples/getopts /d/workspace/Bash/Bash tutorial/examples/declare /d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully change from old directory `/d/workspace/Bash/Bash tutorial/examples/joining` into current directory `/d/workspace/Bash/Bash tutorial/utility modules` and push direcoty `/d/workspace/Bash/Bash tutorial/utility modules` into directory stack
--------------------- end of 6th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial/examples/joining
current directory: /d/workspace/Bash/Bash tutorial/utility modules
current directory stack:
/d/workspace/Bash/Bash tutorial/utility modules
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 7th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/utility modules` into new directory `/d/workspace/Bash/Bash tutorial/utility modules/unset` and push direcoty `/d/workspace/Bash/Bash tutorial/utility modules/unset` into directory stack
/d/workspace/Bash/Bash tutorial/utility modules/unset /d/workspace/Bash/Bash tutorial/utility modules /d/workspace/Bash/Bash tutorial/examples/joining /d/workspace/Bash/Bash tutorial/examples/getopts /d/workspace/Bash/Bash tutorial/examples/declare /d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully change from old directory `/d/workspace/Bash/Bash tutorial/utility modules` into current directory `/d/workspace/Bash/Bash tutorial/utility modules/unset` and push direcoty `/d/workspace/Bash/Bash tutorial/utility modules/unset` into directory stack
--------------------- end of 7th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial/utility modules
current directory: /d/workspace/Bash/Bash tutorial/utility modules/unset
current directory stack:
/d/workspace/Bash/Bash tutorial/utility modules/unset
successfully echoing directory stack
--------- end of echo info ---------
--------------------- 8th operation ---------------------
Ready to pop up from directory stack
/d/workspace/Bash/Bash tutorial/utility modules /d/workspace/Bash/Bash tutorial/examples/joining /d/workspace/Bash/Bash tutorial/examples/getopts /d/workspace/Bash/Bash tutorial/examples/declare /d/workspace/Bash/Bash tutorial/examples /d/workspace/Bash/Bash tutorial
successfully popping up directory stack
--------------------- end of 8th operation ---------------------
current directory stack:
/d/workspace/Bash/Bash tutorial/utility modules
successfully echoing directory stack
--------------------- 9th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/utility modules` into new directory -- the top 2th of directory stack
successfully change from old directory `/d/workspace/Bash/Bash tutorial/utility modules` into current directory `/d/workspace/Bash/Bash tutorial/examples/declare`
--------------------- end of 9th operation ---------------------
--------------------- 10th operation ---------------------
Ready to change from current directory `/d/workspace/Bash/Bash tutorial/examples/declare` into new directory -- the last 2th of directory stack
successfully change from old directory `/d/workspace/Bash/Bash tutorial/examples/declare` into current directory `/d/workspace/Bash/Bash tutorial/examples/getopts`
--------------------- end of 10th operation ---------------------
--------- echo info ---------
old directory: /d/workspace/Bash/Bash tutorial/examples/declare
current directory: /d/workspace/Bash/Bash tutorial/examples/getopts
current directory stack:
/d/workspace/Bash/Bash tutorial/examples/getopts
successfully echoing directory stack
--------- end of echo info ---------

```
