# CH6 -- repetitive loop
## objectives
You can know how to

  + use a repetitive loop

## CH6-1 -- commonly used repetitive loop
keywords about repetitive loops are

List Iteration
  + for

Pretest repetitive loop

  + while 
  + until

List Iteration with multiple selection (like `switch`-`case` in C)

  + select

## CH6-2 -- for (list iteration)

syntax:

```
# iterates a list
for <element> in <list>; do
   # statements
   # using <elements> in this round.
done
```

## CH6-3 -- for (C style)

syntax:

```
# iterates when the condition `<condition_for_true>` is evaluated to false.
for((<variable>=<initial_value>;<condition_for_true>;<step>)); do
    # statements
    # executes these statements iff the condition `<condition_for_true>` is evaluated to true
done
```

## CH6-4 -- while

syntax:

```
while <condition_for_true>; do
    # statements
    # executes these statements iff the condition `<condition_for_true>` is evaluated to true
done
```

## CH6-5 -- until

syntax:

```
until <condition_for_false>; do
    # statements
    # executes these statements iff the condition `<condition_for_false>` is evaluated to false
done
```

## CH6-6 -- select for list iteration loop with multiple selection

syntax:

```
select <element> in <list>; do
    {cases}
done

where

```
{case} := case {expression_for_true} in \n({available_expression} \) {expression}; ;; \n)+(\* \) {expression}; ;; \n) easc
{available_expression} := ({variable} | {constant})
```

### Examples
#### Example 1

```
select action in "Start Service" "Stop Service" "Exit"; do
    case $action in
        "Start Service") echo "正在啟動服務..."; ;;
        "Stop Service") echo "正在停止服務..."; ;;
        "Exit") break ;; # 退出 select 迴圈
        *) echo "無效選項"; ;;
    esac
done
```
