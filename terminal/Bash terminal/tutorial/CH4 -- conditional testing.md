# CH4 -- conditional testing
## objectives
You will know how to

  + execute which block by condition

## CH4-1 -- execute which block by condition
### if
On shell (such as `Bash` shell)

If the expresion in `if` is evaluated to true, 

it will execute `then` block.

Otherwise, it will execute `else` block (if exists)

Format:

```
if <condition> ; then
  <executed_when_condition_is_true_statments> # executed iff `<condition>` is evaluated to true
elif <condition_2> ; then
  <executed_when_condition_2_is_true_statments> # executed iff `<condition_2>` is evaluated to true
else
  <executed_when_all_conditions_is_false_statments> # executed iff `<condition>` and `<condition_2>` are BOTH evaluated to false
fi
```

the simple format is

```
if(<condition>); then
  <executed_when_condition_is_true_statments> # executed iff `<condition>` is evaluated to true
fi
```

### case

syntax:

```
case <variable> in
    <pattern>) <statements> {termination_operator}
    # more clause
esac
```

where

`{termination_operator}` MUST be one of following

+ `;;`: terminates the `case` block.
+ `;&`: executes the statements `<statements>` of next clause.
+ `;;&`: matches the pattern `<pattern>` starting from next clause until the pattern is matched successfully or no patterns can be matched.

  If one of patterns is matched successfully, then it executes the corresponding statements `<statements>`  


#### Examples
##### Example 1
This example illustrate read the input from terminal.

`case_example_1.bash`

```
#!/bin/bash
# case_example_1.bash
echo "請輸入 (y/Y/n/N):"
read answer

case "$answer" in
    y|Y)
        echo "您選擇了 Yes。"
        ;;
    n|N)
        echo "您選擇了 No。"
        ;;
    *) # 匹配所有其他輸入
        echo "無效的輸入。"
        ;;
esac
```

1th interaction

```
./case_example_1.bash
請輸入 (y/Y/n/N):y
您選擇了 Yes。
```

2th interaction

```
./case_example_1.bash
請輸入 (y/Y/n/N):Y
您選擇了 Yes。
```

3th interaction

```
./case_example_1.bash
請輸入 (y/Y/n/N):n
您選擇了 No。
```

4th interaction

```
./case_example_1.bash
請輸入 (y/Y/n/N):N
您選擇了 No。
```

5th interaction

```
./case_example_1.bash
請輸入 (y/Y/n/N):x
無效的輸入。
```

#### Example 2

`case_example_2.bash`

```
#!/bin/bash
# case_example_2.bash

read STATUS

echo "--- 開始執行 Case ---"

case $STATUS in
    "critical")
        echo "1. 發現嚴重錯誤！ (CRITICAL)"
        # 這裡使用 ;; 終止
        ;;

    "warning")
        echo "2. 發現一般警告，需要修復。 (WARNING)"
        # *** 關鍵點：使用 ;& 穿透到下一子句 ***
        ;&

    "error")
        echo "3. 發生任何錯誤或警告，記錄到日誌。 (ERROR/WARNING)"
        # *** 關鍵點：使用 ;;& 繼續測試下一子句 ***
        ;;&

    "ok" | "default")
        echo "4. 狀態 OK，執行預設清理。 (OK/DEFAULT)"
        ;;

    *)
        echo "5. 未知狀態。"
        ;;
esac

echo "--- Case 執行結束 ---"
```

1th interaction

```
./case_example_2.bash
warning
--- 開始執行 Case ---
2. 發現一般警告，需要修復。 (WARNING)
3. 發生任何錯誤或警告，記錄到日誌。 (ERROR/WARNING)
5. 未知狀態。
--- Case 執行結束 ---
```

2th interaction

```
./case_example_2.bash
critical
--- 開始執行 Case ---
1. 發現嚴重錯誤！ (CRITICAL)
--- Case 執行結束 ---
```

3th interaction

```
./case_example_2.bash
error
--- 開始執行 Case ---
3. 發生任何錯誤或警告，記錄到日誌。 (ERROR/WARNING)
5. 未知狀態。
--- Case 執行結束 ---
```

4th interaction

```
./case_example_2.bash
ok
--- 開始執行 Case ---
4. 狀態 OK，執行預設清理。 (OK/DEFAULT)
--- Case 執行結束 ---
```

5th interaction

```
./case_example_2.bash
default
--- 開始執行 Case ---
4. 狀態 OK，執行預設清理。 (OK/DEFAULT)
--- Case 執行結束 ---
```

5th interaction

```
./case_example_2.bash
w
--- 開始執行 Case ---
5. 未知狀態。
--- Case 執行結束 ---
```
