# CH44 -- history
## objectives
You will learn how to 

  + look at specific record in history

## CH44-1 -- History expansion
+ `!!`: exapnd the previous one command then executes it
+ `!{n}`:  exapnd the first `{n}`th command then executes it where `{n}` is an positive integer.
+ `!-{n}`:  exapnd the previous `{n}` command then executes it where `{n}` is an positive integer.
+ `!{string}`:  exapnd command that is the first occurrence that starts with the string `{string}`, where `{string}` is a string without quotation, then executes it.
+ `!?{string}?`:  exapnd command that is the first occurrence that contains the string `{string}`, where `{string}` is a string without quotation, then executes it.

### Examples
#### Example 1
Interactions:

In Git Bash terminal,

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ !!
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ !1
bash: !1: event not found

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ !-1
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ cd "C:"

userJay30@ASUS-B1400CBNGW MINGW64 /c
$ !-2
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 /c
$ !-1
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 /c
$ !e
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 /c
$ !?echo?
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 /c
$ !?echo
echo "$HOME"
/c/Users/userJay30

userJay30@ASUS-B1400CBNGW MINGW64 /c
$

```
