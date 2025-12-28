# CH43 -- history
## objectives
You will learn how to 

  + look at specific record in history

Additionally, you will learn how to 

  + set the number of saving history record

Lastly, you will know the rationale of history record.

## CH43-1 -- rationale of history record
The history records will be written to environment variable `HISTFILE` (defaults to `~` which is environment variable `HOME`)

The history records will be written when Bash terminal is successfully closed, or when `history -w` is executed successfully.

The history records will be read once Bash terminal opens successfully.

## CH43-2 -- History expansion
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

## CH43-3 -- operation of history record
+ `history`: will list all history records
+ `history -w`: will write all commands in this session to history file (the file where environment variable `HISTFILE` is)

