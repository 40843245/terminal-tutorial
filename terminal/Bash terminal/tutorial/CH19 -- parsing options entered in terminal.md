# CH19 -- parsing options entered in terminal
## objectives
You will know how to

  + parse the short options user enters in a command
  + parse the long options user enters in a command

## CH19-1 -- parse the short options user enters in a command
### getopts
syntax:

```
getopts <short-options-string> <current-option-name>
```

where

`<short-options-string>`: consists of many short option (options that has ONLY one character). 

> [!NOTE]
> If there is a `:` append at a short option (in `<short-options-string>`), then it is expected that there is one argument passed with the short option.
>
> Otherwise, it will NOT be required. 

> [!NOTE]
> To make `getopts` use silent mode, use `:` at the beginning of `<short-options-string>` (such as `<short-options-string>` is `":a:b"`)

> [!IMPORTANT]
> When `getopts` uses silent mode, you have to handle these cases
>
> + case `\?` to handle an invalid option (as case 1.2) 
>
> + case `:` to handle NO argument passed when it expects an argument (as case 1.1.2)

The `getopts` is a built-in command.

The string quoated with double quotations followed by getopts define which short options can be recongized by Bash engine and the option should take one argument or not.

When it is invoked, it will try to find current option with current index (stored in special variable `OPTIND`).


Case 1: In non-slient mode

Case 1.1: In non-slient mode, if it can successfully find a short option (i.e. `<short-options-string>` contains the value of currently short option for targeting in current round) 

Case 1.1.1: In this case, also, if either the corresponding argument is passed (if it is expected) or it is not expected.

then it will 

    + set `<current-option-name>` variable as currently found short option

    + set special variable `OPTARG` as currently corresponding argument (only applied when it is expected that there is one argument passed with the short option (i.e. `:` is followed after currently found short option))

    + advance (increase it by 1) special variable `OPTIND`.

Case 1.1.2: In this case, if the corresponding argument is NOT passed (if it is expected)

then it will echo a diagnostic error message like

```
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: option requires an argument -- a
```

    + set `<current-option-name>` variable as `?`

    + set special variable `OPTARG` as NULL value.

    + advance (increase it by 1) special variable `OPTIND`.

Case 1.2: In non-slient mode, if it can't successfully find a short option (i.e. `<short-options-string>` does NOT contains the value of currently short option for targeting in current round) 

then it will echo a diagnostic error message like

```
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option --c
```

Next, 

    + set `<current-option-name>` variable as `?`

    + set special variable `OPTARG` as NULL value.

    + advance (increase it by 1) special variable `OPTIND`.

Case 2: In slient mode

Case 2.1: In slient mode, if it can successfully find a short option (i.e. `<short-options-string>` does NOT contain the value of currently short option for targeting in current round) 

Case 2.1.1: In this case, also, if either the corresponding argument is passed (if it is expected) or it is not expected.

then behaves same as Case 1.1.1

Case 2.1.2: In this case, if the corresponding argument is NOT passed (if it is expected)

then it will catch a diagnostic error message (and thus, not echo it) 

Next

    + set `<current-option-name>` variable as `:`

    + set special variable `OPTARG` as NULL value.

    + advance (increase it by 1) special variable `OPTIND`.

Case 2.2: In slient mode, if it can't successfully find a short option (i.e. `<short-options-string>` does NOT contains the value of currently short option for targeting in current round) 

then it will catch a diagnostic error message (and thus, not echo it)

Next

    + set `<current-option-name>` variable as `:`

    + set special variable `OPTARG` as NULL value.

    + advance (increase it by 1) special variable `OPTIND`.

#### Examples
##### Example 1

`getopts-example-1.bash`

```
#!/bin/bash

echo "--- 腳本開始 ---"

# 初始狀態
echo "初始 OPTIND: $OPTIND"
echo "---"

# optstring: "a:" 表示 -a 必須有參數；"b" 表示 -b 是個開關
while getopts "a:b" opt; do
  echo "--- 迴圈開始 ---"
  echo "當前選項 (opt): $opt"
  
  # 檢查 OPTARG 是否有值
  if [ -n "$OPTARG" ]; then
    echo "當前參數 (OPTARG): $OPTARG"
  else
    echo "當前參數 (OPTARG): (空)"
  fi
  
  echo "下一個要處理的索引 (OPTIND): $OPTIND"
  
  case $opt in
    a)
      echo "處理邏輯: 處理選項 -a，值為 $OPTARG"
      ;;
    b)
      echo "處理邏輯: 處理選項 -b"
      ;;
    \?)
      echo "處理邏輯: 遇到無效選項"
      ;;
  esac
done

echo "--- 迴圈結束 ---"
echo "最終選項 (opt): $opt"
echo "最終 OPTIND: $OPTIND (這指向第一個非選項參數的索引)"

# shift 處理，移除已解析的參數
shift $((OPTIND - 1))
echo "剩餘的非選項參數 (\$@): $@"
```

In non-slient mode, user interactions:

```
<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash"
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 1 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -a -b "switch"
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): -b
下一個要處理的索引 (OPTIND): 3
處理邏輯: 處理選項 -a，值為 -b
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 3 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@): switch

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -a "haha" -b
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): haha
下一個要處理的索引 (OPTIND): 3
處理邏輯: 處理選項 -a，值為 haha
--- 迴圈開始 ---
當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 4
處理邏輯: 處理選項 -b
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 4 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -a "haha" -b "win"
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): haha
下一個要處理的索引 (OPTIND): 3
處理邏輯: 處理選項 -a，值為 haha
--- 迴圈開始 ---
當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 4
處理邏輯: 處理選項 -b
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 4 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@): win

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -a "haha" "hoho" -b "win"
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): haha
下一個要處理的索引 (OPTIND): 3
處理邏輯: 處理選項 -a，值為 haha
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 3 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@): hoho -b win

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -b "win" -a "haha"
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
處理邏輯: 處理選項 -b
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 2 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@): win -a haha

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -b -a "haha"
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
處理邏輯: 處理選項 -b
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): haha
下一個要處理的索引 (OPTIND): 4
處理邏輯: 處理選項 -a，值為 haha
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 4 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -b -a "haha" -c
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
處理邏輯: 處理選項 -b
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): haha
下一個要處理的索引 (OPTIND): 4
處理邏輯: 處理選項 -a，值為 haha
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option -- c
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 5
處理邏輯: 遇到無效選項
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 5 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -c -b -a "haha"
--- 腳本開始 ---
初始 OPTIND: 1
---
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option -- c
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
處理邏輯: 遇到無效選項
--- 迴圈開始 ---
當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 3
處理邏輯: 處理選項 -b
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): haha
下一個要處理的索引 (OPTIND): 5
處理邏輯: 處理選項 -a，值為 haha
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 5 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -c "carry" -b -a "haha"
--- 腳本開始 ---
初始 OPTIND: 1
---
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option -- c
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
處理邏輯: 遇到無效選項
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 2 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@): carry -b -a haha

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -c -carry -b -a "haha"
--- 腳本開始 ---
初始 OPTIND: 1
---
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option -- c
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
處理邏輯: 遇到無效選項
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option -- c
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
處理邏輯: 遇到無效選項
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): rry
下一個要處理的索引 (OPTIND): 3
處理邏輯: 處理選項 -a，值為 rry
--- 迴圈開始 ---
當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 4
處理邏輯: 處理選項 -b
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): haha
下一個要處理的索引 (OPTIND): 6
處理邏輯: 處理選項 -a，值為 haha
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 6 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -c "-carry" -b -a "haha"
--- 腳本開始 ---
初始 OPTIND: 1
---
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option -- c
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
處理邏輯: 遇到無效選項
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option -- c
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
處理邏輯: 遇到無效選項
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): rry
下一個要處理的索引 (OPTIND): 3
處理邏輯: 處理選項 -a，值為 rry
--- 迴圈開始 ---
當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 4
處理邏輯: 處理選項 -b
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): haha
下一個要處理的索引 (OPTIND): 6
處理邏輯: 處理選項 -a，值為 haha
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 6 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -c-x "-carry" -b -a "haha"
--- 腳本開始 ---
初始 OPTIND: 1
---
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option -- c
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 1
處理邏輯: 遇到無效選項
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option -- -
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 1
處理邏輯: 遇到無效選項
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option -- x
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
處理邏輯: 遇到無效選項
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option -- c
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
處理邏輯: 遇到無效選項
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): rry
下一個要處理的索引 (OPTIND): 3
處理邏輯: 處理選項 -a，值為 rry
--- 迴圈開始 ---
當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 4
處理邏輯: 處理選項 -b
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): haha
下一個要處理的索引 (OPTIND): 6
處理邏輯: 處理選項 -a，值為 haha
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 6 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -c
--- 腳本開始 ---
初始 OPTIND: 1
---
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option -- c
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
處理邏輯: 遇到無效選項
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 2 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -c -a
--- 腳本開始 ---
初始 OPTIND: 1
---
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: illegal option -- c
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
處理邏輯: 遇到無效選項
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: option requires an argument -- a
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 3
處理邏輯: 遇到無效選項
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 3 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -a -a -a
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): -a
下一個要處理的索引 (OPTIND): 3
處理邏輯: 處理選項 -a，值為 -a
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: option requires an argument -- a
--- 迴圈開始 ---
當前選項 (opt): ?
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 4
處理邏輯: 遇到無效選項
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 4 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -a -a -b
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): -a
下一個要處理的索引 (OPTIND): 3
處理邏輯: 處理選項 -a，值為 -a
--- 迴圈開始 ---
當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 4
處理邏輯: 處理選項 -b
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 4 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash" -a -a -b 1
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
當前選項 (opt): a
當前參數 (OPTARG): -a
下一個要處理的索引 (OPTIND): 3
處理邏輯: 處理選項 -a，值為 -a
--- 迴圈開始 ---
當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 4
處理邏輯: 處理選項 -b
--- 迴圈結束 ---
最終選項 (opt): ?
最終 OPTIND: 4 (這指向第一個非選項參數的索引)
剩餘的非選項參數 ($@): 1

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$

```

##### Examples 2

`getopts-example-2..bash`

```
#!/bin/bash

echo "--- 腳本開始 ---"

# 初始狀態
echo "初始 OPTIND: $OPTIND"
echo "---"

# 使用靜默模式
# optstring: "a:" 表示 -a 必須有參數；"b" 表示 -b 是個開關
while getopts ":a:b" opt; do
  echo "--- 迴圈開始 ---"
  echo "在靜默模式下，當前選項 (opt): $opt"
  
  # 檢查 OPTARG 是否有值
  if [ -n "$OPTARG" ]; then
    echo "當前參數 (OPTARG): $OPTARG"
  else
    echo "當前參數 (OPTARG): (空)"
  fi
  
  echo "下一個要處理的索引 (OPTIND): $OPTIND"
  
  case $opt in
    a)
      echo "選項 -a 已啟用，參數值是：$OPTARG"      
      ;;
    b)
      echo "選項 -b 已啟用，參數值是：$OPTARG"  
      ;;
    \?)
      # 處理無效選項
      echo "錯誤 (靜默模式): 遇到無效選項 -$OPTARG" >&2
      ;;
    :)
      # 處理缺少必需參數
      echo "錯誤 (靜默模式): 選項 -$OPTARG 缺少必需的參數。" >&2
  esac
done

echo "--- 迴圈結束 ---"
echo "在靜默模式下，最終選項 (opt): $opt"
echo "在靜默模式下，最終 OPTIND: $OPTIND (這指向第一個非選項參數的索引)"

# shift 處理，移除已解析的參數
shift $((OPTIND - 1))
echo "在靜默模式下，剩餘的非選項參數 (\$@): $@"
```

In silent mode, users interactions:

```

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -a -b
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): -b
下一個要處理的索引 (OPTIND): 3
選項 -a 已啟用，參數值是：-b
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 3 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -a
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): :
當前參數 (OPTARG): a
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 選項 -a 缺少必需的參數。
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 2 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): :
當前參數 (OPTARG): a
下一個要處理的索引 (OPTIND): 3
錯誤 (靜默模式): 選項 -a 缺少必需的參數。
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 3 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -ax
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): x
下一個要處理的索引 (OPTIND): 3
選項 -a 已啟用，參數值是：x
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 3 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a-a-a
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): -a-a
下一個要處理的索引 (OPTIND): 3
選項 -a 已啟用，參數值是：-a-a
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 3 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a"-a -a"
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): -a -a
下一個要處理的索引 (OPTIND): 3
選項 -a 已啟用，參數值是：-a -a
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 3 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a""
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): :
當前參數 (OPTARG): a
下一個要處理的索引 (OPTIND): 3
錯誤 (靜默模式): 選項 -a 缺少必需的參數。
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 3 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a ""
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 4
選項 -a 已啟用，參數值是：
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 4 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a" "
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG):
下一個要處理的索引 (OPTIND): 3
選項 -a 已啟用，參數值是：
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 3 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a " "
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG):
下一個要處理的索引 (OPTIND): 4
選項 -a 已啟用，參數值是：
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 4 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a '" "'
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): " "
下一個要處理的索引 (OPTIND): 4
選項 -a 已啟用，參數值是：" "
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 4 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a'" "'
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): " "
下一個要處理的索引 (OPTIND): 3
選項 -a 已啟用，參數值是：" "
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 3 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a'""'
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): ""
下一個要處理的索引 (OPTIND): 3
選項 -a 已啟用，參數值是：""
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 3 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -carry
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): c
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 -c
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): rry
下一個要處理的索引 (OPTIND): 3
選項 -a 已啟用，參數值是：rry
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 3 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -c-arry
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): c
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 -c
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): rry
下一個要處理的索引 (OPTIND): 3
選項 -a 已啟用，參數值是：rry
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 3 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b --arry
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): rry
下一個要處理的索引 (OPTIND): 3
選項 -a 已啟用，參數值是：rry
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 3 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -------------------------------------------arry
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 2
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): rry
下一個要處理的索引 (OPTIND): 3
選項 -a 已啟用，參數值是：rry
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 3 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b--a-r-----ry
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 1
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 1
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): ?
當前參數 (OPTARG): -
下一個要處理的索引 (OPTIND): 1
錯誤 (靜默模式): 遇到無效選項 --
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): -r-----ry
下一個要處理的索引 (OPTIND): 2
選項 -a 已啟用，參數值是：-r-----ry
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 2 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a "win" "victory"
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): win
下一個要處理的索引 (OPTIND): 4
選項 -a 已啟用，參數值是：win
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 4 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@): victory

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b " " -a "win" "victory"
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 2 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@):   -a win victory

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a "win" "victory" -b
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): win
下一個要處理的索引 (OPTIND): 4
選項 -a 已啟用，參數值是：win
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 4 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@): victory -b

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a "win" -b "victory" -b
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): win
下一個要處理的索引 (OPTIND): 4
選項 -a 已啟用，參數值是：win
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 5
選項 -b 已啟用，參數值是：
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 5 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@): victory -b

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-2.bash" -b -a "win" -b "victory" -b -a
--- 腳本開始 ---
初始 OPTIND: 1
---
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 2
選項 -b 已啟用，參數值是：
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): a
當前參數 (OPTARG): win
下一個要處理的索引 (OPTIND): 4
選項 -a 已啟用，參數值是：win
--- 迴圈開始 ---
在靜默模式下，當前選項 (opt): b
當前參數 (OPTARG): (空)
下一個要處理的索引 (OPTIND): 5
選項 -b 已啟用，參數值是：
--- 迴圈結束 ---
在靜默模式下，最終選項 (opt): ?
在靜默模式下，最終 OPTIND: 5 (這指向第一個非選項參數的索引)
在靜默模式下，剩餘的非選項參數 ($@): victory -b -a

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$

```

## CH19-2 -- parse the long options user enters in a command
> [!IMPORTANT]
> `getopts` built-in command can NOT be used for parsing long options.
>
> To parse the long options, you have to parse it one-by-one (by iterating all passed arguments)

### Examples
#### Example 1
This example illustrate how to parse a long option `--recursive` and a short option `-r`

`parsing-long-options-example-1.bash`

```
#!/bin/bash

# 定義一個函數來解析參數
parse_long_options() {
  # 初始化變數
  RECURSIVE_FLAG=false
  OUTPUT_FILE=""

  # 遍歷所有傳入的參數 ($@)
  while [[ "$#" -gt 0 ]]; do
    case "$1" in
      # 處理長選項: --recursive
      --recursive)
        RECURSIVE_FLAG=true
        shift # 移除已處理的參數 (--recursive)
        ;;
      
      # 處理長選項: --output <file>
      --output)
        # 檢查是否有下一個參數作為值
        if [ -n "$2" ]; then
          OUTPUT_FILE="$2"
          shift 2 # 移除兩個參數 (--output 和 <file>)
        else
          echo "錯誤: 選項 '$1' 缺少必需的參數。" >&2
          return 1
        fi
        ;;

      # 處理短選項（可選，但為了完整性）
      -r)
        RECURSIVE_FLAG=true
        shift
        ;;

      # 處理選項結束符號：--
      --)
        shift # 移除選項結束符號
        break # 停止解析選項，後面的都是非選項參數
        ;;

      # 處理所有其他非選項參數
      -*)
        echo "錯誤: 無效選項 '$1'" >&2
        return 1
        ;;
      
      # 處理非選項參數（例如檔案路徑）
      *)
        # 這裡 $1 是第一個非選項參數
        break # 遇到第一個非選項參數就跳出選項解析迴圈
        ;;
    esac
  done

  echo "--- 選項結果 ---"
  echo "是否啟用遞迴 (-r / --recursive): $RECURSIVE_FLAG"
  echo "輸出檔案 (--output): ${OUTPUT_FILE:-未指定}"
  
  echo "--- 剩餘參數 ---"
  # 在迴圈結束後，所有剩餘的參數 (非選項參數) 仍保留在 $@ 中
  if [ "$#" -gt 0 ]; then
    echo "非選項參數列表: $@"
  else
    echo "沒有剩餘的非選項參數。"
  fi
}

# --- 呼叫範例 ---
echo "== 範例 1: 僅長選項 =="
parse_long_options --recursive --output my_log.txt file1.txt

echo -e "\n== 範例 2: 混合選項和非選項參數 =="
parse_long_options -r file2.txt --output "another file.csv"

echo -e "\n== 範例 3: 錯誤處理 (缺少參數) =="
parse_long_options --output
```

executing this script will echo

```
== 範例 1: 僅長選項 ==
--- 選項結果 ---
是否啟用遞迴 (-r / --recursive): true
輸出檔案 (--output): my_log.txt
--- 剩餘參數 ---
非選項參數列表: file1.txt

== 範例 2: 混合選項和非選項參數 ==
--- 選項結果 ---
是否啟用遞迴 (-r / --recursive): true
輸出檔案 (--output): 未指定
--- 剩餘參數 ---
非選項參數列表: file2.txt --output another file.csv

== 範例 3: 錯誤處理 (缺少參數) ==
錯誤: 選項 '--output' 缺少必需的參數。
```
