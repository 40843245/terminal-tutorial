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
> If there is a `:` in short option (in `<short-options-string>`), then it is expected that there is one argument passed with the short option.
>
> Otherwise, it will NOT require. 

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

Case 1.2: In this case, if the corresponding argument is NOT passed (if it is expected)

then it will throw an error like

```
D:\workspace\Bash\Bash tutorial\examples\getopts\getopts-example-1.bash: option requires an argument -- a
```

    + set `<current-option-name>` variable as `?`

    + set special variable `OPTARG` as NULL value.

    + advance (increase it by 1) special variable `OPTIND`.
      
Case 1.2: In non-slient mode, if it can successfully find a short option (i.e. `<short-options-string>` does NOT contain the value of currently short option for targeting in current round) 

then it will 

    + set `<current-option-name>` variable as `?`

    + set special variable `OPTARG` as NULL value

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
