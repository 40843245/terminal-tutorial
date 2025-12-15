# CH10 -- Word Splitting
## objectives
You will know

  + behavior of word splitting in Bash
  + special variable about word splitting

## CH10-1 -- behavior of word splitting in Bash
**ONLY** applied for a string that is expanded from a list of string (the string after **parameter expansion**), it can be splitted by many word,

this behavior is called word splitting.

### Examples
#### Example 1

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

One command can behaves this thanks to word splitting and expansions

When one enters these commands in terminal

```
dir "D:\workspace" "D:\SrcCode"
```

In Bash, the shell will receive a list of string `{"D:\workspace","D:\SrcCode"}`

and special variables will be


| Recommended or not | expression of special variable | value | number of parameters that will be passed by shell | description |
| :-- | :-- | :-- | :-- | :-- |
| RECOMMENDED | `"$*"` | "D:\workspace D:\SrcCode" | always 1 | it will be combined into one longer string , quotating with a double quoation. (see `"$@"`) |
| RECOMMENDED | `"$@"` | "D:\workspace" "D:\SrcCode" | always 2 | it will be NOT combine them together, quotating with a doule quoation for each arguments. |
| NOT RECOMMENDED | `$@` | D:\workspace D:\SrcCode | at least 2 (if no any whitespace between these arguments) | it will be NOT combine them together (BUT no quoated with a double quotation) | 
| NOT RECOMMENDED | `$*` | D:\workspace D:\SrcCode | at least 2 (if no any whitespace between these arguments) | it will be combined into one longer string (BUT no quoated with a double quotation) | 

## CH10-2 -- special variable about word splitting
### IFS (Internal Field Seperator)

The shell will split **the string after parameter expansion** into a list of string according the value of `$IFS` special variable.

#### Behavior of `$IFS`
1. The shell treats each character of `$IFS` as a delimiter, and splits the results of the other expansions into fields using these characters as field terminators.

2. `{whitespace}{tab}{newline}` characters are treated as `IFS whitespace`

3. Iff `$IFS` ONLY contains these characters `{whitespace}{tab}{newline}`, then

   3.1. any consequence consists of any of combination of these characters `{whitespace}{tab}{newline}` will be treated as **one delimiter**

   3.2. the leading and trailing `IFS whitespace` will be trimmed (if exists) for the stting that is splitted. 

4. If `$IFS` is unset in terminal, then shell will behaves as the default value `{whitespace}{tab}{newline}` when splitting the word and these characters (`{whitespace}{tab}{newline}`) are treated as `IFS whitespace` 

   where

  `{whitespace}`: a whitespace
  
  `{tab}`: a tab
  
  `{newline}`: a break line

5. If `$IFS` is set to empty string, then shell will treat it as NULL value.

   Here, it will NOT perform word splitting.

   However, **implicit null arguments** will be reomved.

6. If `$IFS` contains non-whitespace character, then

   6.1. For these non-whitespace characters, the consequence of non-whitespace characters and its adjacent `IFS whitespace` will be the boundary of fields.

   6.2. Its adjacent of non-whitespace characters (used as delimiter) will generate a NULL field (i.e. empty string).

#### Examples
##### Example 1

`IFS-example-1.bash`

```
# 預設的 IFS: <space><tab><newline>
# 假設 IFS= ' \t\n'

my_string="apple  banana,cherry"
echo "--- 預設 IFS ---"
# 使用 for 迴圈來展示分詞結果 (for 迴圈預設會使用 IFS 分詞)
for item in $my_string; do
  echo "單詞: $item"
done
```

will echo

```
--- 預設 IFS ---
單詞: apple
單詞: banana,cherry

```

##### Example 2

`IFS-example-2.bash`

```
# 預設的 IFS: <space><tab><newline>
# 假設 IFS= ' \t\n'

my_csv="data1,data2,data3"

# 暫時改變 IFS，並在處理完後恢復
OLD_IFS=$IFS
IFS=','

echo "--- IFS=' ,' ---"
# 確保 $my_csv 展開時在非雙引號下，才能進行分詞
for item in $my_csv; do
  echo "欄位: $item"
done

# 恢復 IFS
IFS=$OLD_IFS
```

will echo

```
--- IFS=' ,' ---
欄位: data1
欄位: data2
欄位: data3

```
#### reference

See [Gemini's response](https://gemini.google.com/share/b17a5bd18807) for a brief explanation of [word splitting on GNU official docs](https://www.gnu.org/software/bash/manual/bash.html#Word-Splitting) in Traditional Chinese

See [word splitting on GNU official docs](https://www.gnu.org/software/bash/manual/bash.html#Word-Splitting) for detailed explanation of word splitting and `IFS` in English 
