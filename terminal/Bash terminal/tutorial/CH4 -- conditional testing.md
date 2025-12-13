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
