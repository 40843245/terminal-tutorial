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

##### Example 3

`IFS-example-3.bash`

```
# 設定 IFS 為 冒號、空格、Tab
IFS_COMPLEX=': \t' # 實際值為 冒號 + 空格 + Tab

my_path="dir1: dir2  :dir3\t::dir4"

OLD_IFS=$IFS
IFS=$IFS_COMPLEX

echo "--- 複雜 IFS=': \t' ---"
# 讀取結果到陣列 (read -a 預設使用 IFS 分隔)
# 注意：read 指令在讀取時，不會像 for 迴圈那樣先移除前後的 IFS 空白
# 但對於 `for item in $var;` 這種形式的展開，會移除前後 IFS 空白。
# 我們使用 `read -ra` 來展示效果：
read -ra items <<< "$my_path" 

for item in "${items[@]}"; do
  # 輸出時加上引號，以觀察空字串
  echo "欄位: \"$item\""
done

# 恢復 IFS
IFS=$OLD_IFS
```

will echo

```
--- 複雜 IFS=': \t' ---
欄位: "dir1"
欄位: "dir2"
欄位: "dir3"
欄位: ""
欄位: ""
欄位: ""
欄位: "dir4"

```

##### Example 4

`IFS-example-4.bash`

```
empty_var=""
unset unset_var

# --- 修正後的條件判斷與輸出 ---

empty_or_null_empty="unknown"

# 判斷 $empty_var 是否為空或 null
if [ -z "$empty_var" ]; then
    empty_or_null_empty="Yes"
else
    empty_or_null_empty="No"
fi

# 判斷 $unset_var 是否為空或 null
if [ -z "$unset_var" ]; then
    empty_or_null_unset="Yes"
else
    empty_or_null_unset="No"
fi

# 使用正確的變數替換 ${var_name}
echo "empty_var is empty or null? ${empty_or_null_empty}"
echo "unset_var is empty or null? ${empty_or_null_unset}"

# --- 原始的分詞邏輯（這部分是正確的） ---

# 1. 隱式空參數 (未引號的 $empty_var 或 $unset_var) 會被移除
echo "--- 隱式空參數移除 ---"
set -- a $empty_var b $unset_var c
echo "參數數量: $#"
echo "參數 1: $1"
echo "參數 2: $2"
echo "參數 3: $3"

# 2. 顯式空參數 (用引號包住的 "" 或 '') 會被保留
echo "--- 顯式空參數保留 ---"
set -- a "" b '' c
echo "參數數量: $#"
echo "參數 1: $1"
echo "參數 2: $2"
echo "參數 3: $3"
echo "參數 4: $4"
echo "參數 5: $5"
```

will echo

```
empty_var is empty or null? Yes
unset_var is empty or null? Yes
--- 隱式空參數移除 ---
參數數量: 3
參數 1: a
參數 2: b
參數 3: c
--- 顯式空參數保留 ---
參數數量: 5
參數 1: a
參數 2:
參數 3: b
參數 4:
參數 5: c

```

##### Example 5

`IFS-example-5`

```
#!/bin/bash

# 1. 設置包含非空白字符和空白字符的 IFS
# IFS = 冒號 + 空格 + Tab
IFS_COMPLEX=': ' # 我們使用冒號和空格

# 待處理的字串
DATA="::Apple : Banana : :Orange: "

OLD_IFS=$IFS
IFS=$IFS_COMPLEX # 應用我們複雜的 IFS

echo "--- 原始字串: '$DATA' ---"
echo "--- 複雜 IFS: '$IFS_COMPLEX' ---"

# 使用 read -ra 將字串結果根據 IFS 讀入陣列
# <<< "$DATA" 是一種 here-string，將字串作為 read 的輸入
read -ra result_array <<< "$DATA"

# 恢復 IFS
IFS=$OLD_IFS

# 輸出結果 (注意：我們印出陣列長度和每個元素，包括空字串)
echo "--- 分割結果 ---"
echo "陣列元素數量: ${#result_array[@]}"

i=0
for item in "${result_array[@]}"; do
  echo "索引 $i: \"$item\""
  i=$((i+1))
done
```

will echo

```
--- 原始字串: '::Apple : Banana : :Orange: ' ---
--- 複雜 IFS: ': ' ---
--- 分割結果 ---
陣列元素數量: 6
索引 0: ""
索引 1: ""
索引 2: "Apple"
索引 3: "Banana"
索引 4: ""
索引 5: "Orange"

```

analysis of output

In `"::Apple : Banana : :Orange: "`,

+ you will see the leading character is `:` (there is no any characters before leading character),

thus there is a NULL value ("") in the 0th of the array `result_array`.

echo following in 1th iteration of the loop

```
索引 0: ""
```

+ also you will see there are no any characters between first occurence of `:` and second occurence of `:`,

thus there is a NULL value ("") in the 1th of the array `result_array`.

echo following in 2th iteration of the loop

```
索引 1: ""
```

+ also you will see there is one whitespaces (which is one of `IFS whitespace`) between fifth occurence of `:` and sixth occurence of `:`,

thus there is a NULL value ("") in the 3th of the array `result_array`.

echo following in 4th iteration of the loop

```
索引 3: ""
```

#### reference

See [Gemini's response](https://gemini.google.com/share/b17a5bd18807) for a brief explanation of [word splitting on GNU official docs](https://www.gnu.org/software/bash/manual/bash.html#Word-Splitting) in Traditional Chinese

See [word splitting on GNU official docs](https://www.gnu.org/software/bash/manual/bash.html#Word-Splitting) for detailed explanation of word splitting and `IFS` in English 
