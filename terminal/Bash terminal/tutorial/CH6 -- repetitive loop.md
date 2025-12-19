# CH6 -- repetitive loop
## objectives
You can know how to

  + use a repetitive loop
  + break in a repetive loop
  + continue in a repetive loop

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

see CH6-6 Example 1.

## CH6-4 -- while

syntax:

```
while <condition_for_true>; do
    # statements
    # executes these statements iff the condition `<condition_for_true>` is evaluated to true
done
```

see CH6-7 Example 1.

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
```

where

{cases} is shown in case section in CH4-1. 

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

## CH6-7 -- break in a repetitive loop
`break` built-in comman with an arguments `{n}` where `n` is a positive integer can exit the `n`th loop to (`m`-`n`)th loop in `m`th loop (`m>=n`) 

When `{n}` is NOT specified, it uses one as default value. 

syntax:

```
break {n}?
```

returns:

When `{n}` is a positive integer, it always succeeds, so returns exit code 0

Otherwise, it always fails, so returns exit code 1

### Examples
#### Example 1

`break-example-1.bash`

```
main(){
    echo "----------- 九九乘法表 -----------"
    for((i=1;i<10;i++)); do
        for((j=1;j<10;j++)); do
            printf "%d*%d=%d\t" $i $j $(($i*$j)) 
        done
        printf "\n"
    done
    echo "----------- end of 九九乘法表 -----------"

    echo "----------- 一九乘法表 -----------"
    for((i=1;i<10;i++)); do
        for((j=1;j<10;j++)); do
            printf "%d*%d=%d\t" $i $j $(($i*$j)) 
        done
        printf "\n"
        break 1
    done
    echo "----------- end of 一九乘法表 -----------"

    echo "----------- 九一乘法表 -----------"
    for((i=1;i<10;i++)); do
        for((j=1;j<10;j++)); do
            printf "%d*%d=%d\t" $i $j $(($i*$j))
            break 1 
        done
        printf "\n"
    done
    echo "----------- end of 九一乘法表 -----------"

    echo "----------- 一一乘法表 -----------"
    for((i=1;i<10;i++)); do
        for((j=1;j<10;j++)); do
            printf "%d*%d=%d\t" $i $j $(($i*$j))
            break 2 
        done
        printf "\n"
    done
    echo "----------- end of 一一乘法表 -----------"
}

main
```

executing this script will echo

```
----------- 九九乘法表 -----------
1*1=1   1*2=2   1*3=3   1*4=4   1*5=5   1*6=6   1*7=7   1*8=8   1*9=9
2*1=2   2*2=4   2*3=6   2*4=8   2*5=10  2*6=12  2*7=14  2*8=16  2*9=18
3*1=3   3*2=6   3*3=9   3*4=12  3*5=15  3*6=18  3*7=21  3*8=24  3*9=27
4*1=4   4*2=8   4*3=12  4*4=16  4*5=20  4*6=24  4*7=28  4*8=32  4*9=36
5*1=5   5*2=10  5*3=15  5*4=20  5*5=25  5*6=30  5*7=35  5*8=40  5*9=45
6*1=6   6*2=12  6*3=18  6*4=24  6*5=30  6*6=36  6*7=42  6*8=48  6*9=54
7*1=7   7*2=14  7*3=21  7*4=28  7*5=35  7*6=42  7*7=49  7*8=56  7*9=63
8*1=8   8*2=16  8*3=24  8*4=32  8*5=40  8*6=48  8*7=56  8*8=64  8*9=72
9*1=9   9*2=18  9*3=27  9*4=36  9*5=45  9*6=54  9*7=63  9*8=72  9*9=81
----------- end of 九九乘法表 -----------
----------- 一九乘法表 -----------
1*1=1   1*2=2   1*3=3   1*4=4   1*5=5   1*6=6   1*7=7   1*8=8   1*9=9
----------- end of 一九乘法表 -----------
----------- 九一乘法表 -----------
1*1=1
2*1=2
3*1=3
4*1=4
5*1=5
6*1=6
7*1=7
8*1=8
9*1=9
----------- end of 九一乘法表 -----------
----------- 一一乘法表 -----------
1*1=1   ----------- end of 一一乘法表 -----------

```

## CH6-8 -- continue in a repetitive loop
`continue` built-in comman with an arguments `{n}` where `n` is a positive integer can jump to the beginning of the `n`th loop to (`m`-`n`)th loop in `m`th loop (`m>=n`) 

When `{n}` is NOT specified, it uses one as default value. 

syntax:

```
continue {n}?
```

returns:

When `{n}` is a positive integer, it always succeeds, so returns exit code 0

Otherwise, it always fails, so returns exit code 1

### Examples
#### Example 1

`continue-example-1.bash`

```
function main(){
    declare -i is_end_of_the_game=0;
    declare -i completed_runs=0;
    declare -i completed_sub_runs_in_this_run=0;

    while [ $completed_runs -le 10 ]
    do
        completed_runs=$((completed_runs + 1))
        if [ $completed_runs -eq 2 ]; then
            echo "will continue to the outer loop in outer loop"
            continue 1
        fi
        if [ $completed_runs -eq 3 ]; then
            completed_sub_runs_in_this_run=0
            while [ $completed_sub_runs_in_this_run -le 6 ]
            do
                completed_sub_runs_in_this_run=$((completed_sub_runs_in_this_run + 1))
                if [ $completed_sub_runs_in_this_run -eq 2 ]; then
                    echo "will continue to the inner loop in inner loop"
                    continue 1
                fi
                if [ $completed_sub_runs_in_this_run -eq 3 ]; then
                    echo "will continue to the outer loop in inner loop"
                    continue 2
                fi
                
                echo "In the inner loop, completed_runs=\`$completed_runs\`,completed_sub_runs_in_this_run=\`$completed_sub_runs_in_this_run\`"
            done
        fi
        if [ $completed_runs -eq 6 ]; then
            is_end_of_the_game=1
            echo "is_end_of_the_game is set to integer 1 and then will continue to the outer loop in outer loop"
        fi
        echo "In the outer loop, completed_runs=\`$completed_runs\`,completed_sub_runs_in_this_run=\`$completed_sub_runs_in_this_run\`"
    done
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\loop\repetitive loop\continue\continue-example-1.bash"
In the outer loop, completed_runs=`1`,completed_sub_runs_in_this_run=`0`
will continue to the outer loop in outer loop
In the inner loop, completed_runs=`3`,completed_sub_runs_in_this_run=`1`
will continue to the inner loop in inner loop
will continue to the outer loop in inner loop
In the outer loop, completed_runs=`4`,completed_sub_runs_in_this_run=`3`
In the outer loop, completed_runs=`5`,completed_sub_runs_in_this_run=`3`
is_end_of_the_game is set to integer 1 and then will continue to the outer loop in outer loop
In the outer loop, completed_runs=`6`,completed_sub_runs_in_this_run=`3`
In the outer loop, completed_runs=`7`,completed_sub_runs_in_this_run=`3`
In the outer loop, completed_runs=`8`,completed_sub_runs_in_this_run=`3`
In the outer loop, completed_runs=`9`,completed_sub_runs_in_this_run=`3`
In the outer loop, completed_runs=`10`,completed_sub_runs_in_this_run=`3`
In the outer loop, completed_runs=`11`,completed_sub_runs_in_this_run=`3`

```
