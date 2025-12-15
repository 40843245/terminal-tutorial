# CH10 -- Word Splitting
## objectives
You will know

  + behavior of word splitting in Bash
  + special variable about word splitting

## CH10-1 -- behavior of word splitting in Bash
**ONLY** applied for a string that is expanded from a list of string, it can be splitted by many word,

this behavior is called word splitting.

For example,

when one enters these commands in terminal

```
dir "D:\workspace" "D:\SrcCode"
```

It will list all children of "D:\workspace" and "D:\SrcCode" respectively.

```
$ dir "D:\workspace" "D:\SrcCode"
D\:\\SrcCode:
OSV\ Scanner      osv-scanner_1.9.2_windows_amd64.exe  tutorial.md
README.md         osv-scanner_windows_amd64.exe
cdxgen11.2.7.exe  run_and_clean.bat

D\:\\workspace:
Git\ Control      Trash                                practice\ repo
HelperTools       developing\ project\ (git)           tutorial\ projects
SBOMTestProjects  developing\ project\ (git)\ publish  utility\ projects
Test              developing\ projects

```

One command can behaves this thanks to word splitting

When one enters these commands in terminal

```
dir "D:\workspace" "D:\SrcCode"
```

In Bash, the shell will receive a list of string `{"D:\workspace","D:\SrcCode"}`

and special variables will be

| expression of special variable | value | number of arguments (considered by shell) | description |
| :-- | :-- | :-- | :-- |
| `"$*"` | "D:\workspace D:\SrcCode" | always 1 | it will be combined into one longer string , quotating with a double quoation. (see `"$@"`) |
| `"$@"` | "D:\workspace" "D:\SrcCode" | always 2 | it will be NOT combine them together, quotating with a doule quoation for each arguments. |
| `$@` | D:\workspace D:\SrcCode | at least 2 (if no any whitespace between these arguments) | it will be NOT combine them together (BUT no quoated with a double quotation) | 
| `$*` | D:\workspace D:\SrcCode | at least 2 (if no any whitespace between these arguments) | it will be combined into one longer string (BUT no quoated with a double quotation) | 
