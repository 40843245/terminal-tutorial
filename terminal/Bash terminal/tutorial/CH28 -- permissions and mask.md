# CH28 -- permissions and mask
## objectives
You will know how to 

  + represents a permission in Linux
  + change the permission
  + unmask the permission

## CH28-1 -- a permission in Linux
In Linux, a permission of a file or directory is represented as a three-digits octal number.

Each digit indicates who (specific role) applies to this permission.

Since a digit is in octal system, it can be interpreted as a three-digits binary number.

> [!TIP]
> `2^3 = 8`

For the three-digits binary number, in each digit, it indicates which permissions are granted in specific role. 

+ The three-digits octal number consist of `ugo`

where

`u`: user (the owner of file or directory)

`g`: group (members in the group of the file or directory)

`o`: others (other people than members listed in above two cases) 

+ In each digits of the three-digits octal number, which is interpreted as three-digit of binary number, it consists of `rwx`

where

`r`: read => 2^2=4 (in octal system)

`w`: write => 2^1=2 (in octal system)

`x`: execute => 2^0 (in octal system)

## CH28-2 -- change the permission
### `chmod`
Built-in command `chomod` (shorthand of *ch*ange *mod*e) changes the permission of a file or directory.

> [!NOTE]
> `chmod` can change permission for all roles.

There are two type of arguments you can passed.

1. with number

Directly assign new permission with a three-digit octal number.

for example,

```
chmod 777 "D:\workspace\Bash\Bash tutorial\examples\functions\shift\shift-example-1.bash"
```

2. symbol (plus sign `+`)

Use plus sign `+` to add permission to specific role.

for example,

```
chmod u+x "D:\workspace\Bash\Bash tutorial\examples\functions\shift\shift-example-1.bash"
```

### `chown`
Built-in command `chown` (shorthand of *ch*ange *own*er) changes the owner of a file or directory.

for example,

```
sudo chown user:john "D:\workspace\Bash\Bash tutorial\examples\functions\shift\shift-example-1.bash"
```

## CH28-3 -- unmask the permission
### `umask`
Built-in command `umask` (NOT spelled as `unmask`) sets the default permission when creating a direcory or a file.

for example,

```
umask 077
```

## Examples
### Example 1

`permission-example-1.bash`

```
## > [!NOTE]
## > To see the effect of `chmod` and `chown` etc commands,
## >
## > You must execute it on Linux environment
## >
## > such as terminal in Linux
## >
## > or powershell in WSL (Windows Subsystem for Linux) 
## >
## > for more details, see the last conversation in this chat [Gemini's response](https://gemini.google.com/share/7e5fcc355827)

# Get the directory where the current script is located
SCRIPT_DIR=$(cd "$(dirname "$0")" && pwd)

echo "Script Directory: $SCRIPT_DIR"

function delete_and_create_directory(){
    local directory_path="$1"

    rm -rf "$directory_path"
    mkdir -p "$directory_path"

    return 0
}

function initialize(){
    local outputs_permissions_base_directory="$SCRIPT_DIR/../../outputs/permissions"    
    local files1_name=""
    local files2_name=""
    local file1_name=""
    local file2_name=""
    local user1=""
    
    delete_and_create_directory "$outputs_permissions_base_directory"
    cd "$outputs_permissions_base_directory"

    # files1_name=file1.txt file2.txt file3.txt file4.txt file5.txt
    files1_name=$(echo file{1..5}.txt)

    # files2_name=file6.txt file7.txt file8.txt file9.txt file10.txt
    files2_name=$(echo file{6..10}.txt)

    file1_name=$(echo $files1_name | awk '{print $1}')
    file2_name=$(echo $files1_name | awk '{print $2}')

    echo "ready to create files file1.txt file2.txt file3.txt file4.txt file5.txt with default permission 777 in Linux environment"

    # create files file1.txt file2.txt file3.txt file4.txt file5.txt
    touch $files1_name

    echo "all files and directories in this directory \`$PWD\`"

    ls -l

    echo "ready to change the permission of file \`$file1_name\` to 700"
    
    # change permission of file1.txt to octal number `700`
    # which means that 
    # the owner can read, write, execute the file.
    # the members in the group can't read, write, execute the file.
    # the others can't read, write, execute the file.
    chmod 700 $file1_name

    echo "all files and directories in this directory \`$PWD\`"

    ls -l

    user1="user1"

    echo "ready to change the owner of file \`$file2_name\` to the user \`$user1\`"
    
    # change owner of file2.txt to the user user1
    sudo chown $user1 $file2_name

    echo "all files and directories in this directory \`$PWD\`"

    ls -l

    echo "ready to change the default permission as 007"

    # change the default permission as 007
    #    original default permission: 777
    # -) umask value                : 770
    # -----------------------------------
    #                                 007
    umask 770

    echo "ready to create files file6.txt file7.txt file8.txt file9.txt file10.txt with default permission 007 in Linux environment"

    # create files file6.txt file7.txt file8.txt file9.txt file10.txt
    touch $files2_name

    echo "all files and directories in this directory \`$PWD\`"

    ls -l
}

main(){
    # set -x
    initialize
    # set +x
}

main
```

### reference
[permission in Linux](https://gemini.google.com/share/3d35227dfbf2)

