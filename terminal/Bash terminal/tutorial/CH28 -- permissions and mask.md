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


### reference
[permission in Linux](https://gemini.google.com/share/3d35227dfbf2)

