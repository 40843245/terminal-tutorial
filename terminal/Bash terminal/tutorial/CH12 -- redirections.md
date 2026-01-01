# CH12 -- redirections
## objectives
You will learn how to 

    + stream out output to a file and stream the input in from a file using redirections

## CH12-1 -- syntax and description
+ `>`: stream the output (of `stdout`) out to a file (overwritting if it exists).
+ `>>`: stream the output (of `stdout`) out to a file (append at the file rather than overwritting if it exists).
+ `<`: stream the input (of `stdin`) in from a file (overwritting if it exists).
+ `2>`: stream the error (of `stderr`)  to a file (overwritting if it exists).
+ `|`: stream the output (of `stdout`) out in next command as input used in next command and stream the input (of `stdin`) in from `stdout` used in next command. 
+ `|&`: shorthand of `2>&1 | ` tream the output (of `stdout`,`stderr`) out in next command as input used in next command and stream the input (of `stdin`) in from `stdout` used in next command. 

### Examples
#### Example 1
`redirections-example-1.bash`

```
$ source "D:\workspace\Bash\Bash tutorial\examples\redirections\redirections-example-1.bash"
-rw-r--r-- 1 userJay30 197121 113 Dec 31 14:33 fake-demo-1.cs

```

Assume that

this script is located at `/d/workspace/Bash/Bash tutorial/examples/redirections/redirections-example-1.bash`
the file content of `/d/workspace/Bash/Bash tutorial/outputs/available-scripts/csharp.txt` is

```
-rw-r--r-- 1 userJay30 197121 113 Dec 31 14:33 fake-demo-1.cs

```

the directory `/d/workspace/Bash/Bash tutorial/outputs/scripts` exists

and `script1.cs` under the directory is empty string or does NOT exist

Then executing this script will echo

```
$ source "D:\workspace\Bash\Bash tutorial\examples\redirections\redirections-example-1.bash"
-rw-r--r-- 1 userJay30 197121 113 Dec 31 14:33 fake-demo-1.cs

```

and the file content of `/d/workspace/Bash/Bash tutorial/outputs/scripts/script1.cs` will be

```

    Hello World!!!
    C#
    

    Java
    C++

    
```

and the file content of `/d/workspace/Bash/Bash tutorial/outputs/scripts/script1.cs` will be

```

    JavaScript
    Perl
    
```