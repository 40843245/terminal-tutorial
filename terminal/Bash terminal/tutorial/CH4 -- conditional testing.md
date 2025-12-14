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
# !/bin/bash
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
