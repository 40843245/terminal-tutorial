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

#### Example 2

`continue-example-3.bash`

```
function roll_dice(){
    local name_in_current_turn="$1"
    declare -i rolling_times_in_this_round=0;
    declare -i max_rolling_times_in_this_round=5;
    declare -i point_1=0;
    declare -i point_2=0;
    declare -i sum=0;
    declare -i current_status_code=2;

    while [ $rolling_times_in_this_round -lt $max_rolling_times_in_this_round ]
    do 
        point_1=$(( ( RANDOM % 6 )  + 1 ));
        point_2=$(( ( RANDOM % 6 )  + 1 ));
        sum=$((point_1 + point_2))
            echo "The \`$name_in_current_turn\` throws two dices and gets \`$sum\` point in total."
        if [ $sum  -eq 7 ]; then
            # win this round
            current_status_code=0
            break
        fi
        if [ $sum -eq 11 ]; then
            # lose this round
            current_status_code=1;
            break
        fi
        ((rolling_times_in_this_round++))
    done

    if [ $rolling_times_in_this_round -eq $max_rolling_times_in_this_round ]; then
        # lose this round
        # due to rolling the dice too many times (>=5) in this round.
        current_status_code=1;
    fi

    return $current_status_code;
}

function dice_rolling_game(){
    declare number_of_turns_of_championship=$1;
    local max_win_needed_for_championship=$((number_of_turns_of_championship/2+1))
    local number_of_completed_turns_in_this_overtime=0;
    local number_of_completed_turns=0;
    declare max_turn_limit=10;
    declare max_overtime_limit=4;
    declare -i player_win_times=0;
    declare -i computer_win_times=0;
    local is_player_win=0;
    local is_computer_win=0;
    local is_win_in_this_round=0;
    local is_end_of_the_game=0;
    local current_turn=1; # 0: player's turn 1: computer's turn
    local name_in_current_turn=player;
    local status_text_in_current_round=win;
    local message_in_current_round="";
    local congratulation_message="";
    local needs_to_execute_this_turn=1;

    echo "------------- the rolling game starts -------------"

    while [ $is_end_of_the_game -eq 0 ]
    do
        status_text_in_current_round=""
        message_in_current_round=""

        # 0->1, 1->0
        current_turn=$(( ( current_turn + 1 ) % 2 ))

        if [[ $current_turn -eq 0 ]]; then
            name_in_current_turn=player
        else
            name_in_current_turn=computer
        fi 

        roll_dice "$name_in_current_turn"
        
        is_win_in_this_round=$?
        
        if [[ $is_win_in_this_round -eq 1 ]]; then
            status_text_in_current_round="win"
            message_in_current_round="The $name_in_current_turn ${status_text_in_current_round}s in ${number_of_completed_turns}th ${name_in_current_turn}'s turn";
            if [[ $current_turn -eq 0 ]]; then
                player_win_times=$((player_win_times + 1))
            else
                computer_win_times=$((computer_win_times + 1))
            fi
        elif [ $is_win_in_this_round -eq 0 ]; then
            status_text_in_current_round="lose"
            message_in_current_round="The $name_in_current_turn ${status_text_in_current_round}s in ${number_of_completed_turns}th ${name_in_current_turn}'s turn";
        else
            message_in_current_round="Unknown error in ${number_of_completed_turns}th ${name_in_current_turn}'s turn";
        fi

        number_of_completed_turns=$((number_of_completed_turns + 1))

        echo "$message_in_current_round"
        
        if [[ $player_win_times -gt $max_win_needed_for_championship && $((player_win_times - computer_win_times)) -gt 2 ]]; then
            # player wins
            is_player_win=1;
            # end games in this championship
            is_end_of_the_game=1
            continue 1
        fi

        if [[ $computer_win_times -gt $max_win_needed_for_championship && $((computer_win_times - player_win_times)) -gt 2 ]]; then
            # computer wins
            is_computer_win=1;
            # end games in this championship
            is_end_of_the_game=1
            continue
        fi
        
        if [[ $number_of_completed_turns -gt $number_of_turns_of_championship ]]; then
            number_of_completed_turns_in_this_overtime=0;
            while [ $number_of_completed_turns_in_this_overtime -le $max_overtime_limit ]
            do
                status_text_in_current_round=""
                message_in_current_round=""
                
                # 0->1, 1->0
                current_turn=$(( ( current_turn + 1 ) % 2 ))

                if [[ $current_turn -eq 0 ]]; then
                    name_in_current_turn=player
                else
                    name_in_current_turn=computer
                fi

                roll_dice "$name_in_current_turn"
                
                is_win_in_this_round=$?

                if [[ $is_win_in_this_round -eq 1 ]]; then
                    status_text_in_current_round="win"
                    message_in_current_round="The $name_in_current_turn ${status_text_in_current_round}s in ${number_of_completed_turns}th ${name_in_current_turn}'s turn";
                    if [[ $current_turn -eq 0 ]]; then
                        player_win_times=$((player_win_times + 1))
                    else
                        computer_win_times=$((computer_win_times + 1))
                    fi
                elif [ $is_win_in_this_round -eq 0 ]; then
                    status_text_in_current_round="lose"
                    message_in_current_round="The $name_in_current_turn ${status_text_in_current_round}s in ${number_of_completed_turns}th ${name_in_current_turn}'s turn";
                else
                    message_in_current_round="Unknown error in ${number_of_completed_turns}th ${name_in_current_turn}'s turn";
                fi

                number_of_completed_turns=$(( number_of_completed_turns + 1 ))
                number_of_completed_turns_in_this_overtime=$(( number_of_completed_turns_in_this_overtime + 1 ))

                echo "$message_in_current_round"

                if [[ $player_win_times -gt $max_win_needed_for_championship && $(( player_win_times - computer_win_times )) -gt 2 ]]; then
                    # player wins
                    is_player_win=1;
                    
                    # end games in this championship
                    is_end_of_the_game=1
                    continue 2
                fi

                if [[ $computer_win_times -gt $max_win_needed_for_championship && $(( computer_win_times - player_win_times )) -gt 2 ]]; then        
                    # computer wins
                    is_computer_win=1;
                    
                    # end games in this championship
                    is_end_of_the_game=1
                    continue 2
                fi
            done 
        fi
    done
    echo "game over"
    if [ $is_player_win -eq 1 ]; then
        echo "The player wins this championship"
    elif [ $is_computer_win -eq 1 ]; then
        echo "The player wins this championship"
    fi

    echo "------------- the rolling game over -------------"
}

main(){
    echo "------------- ready to play the rolling game -------------"

    dice_rolling_game 3

    echo "------------- bye bye -------------"
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\loop\repetitive loop\continue\continue-example-3.bash"
------------- ready to play the rolling game -------------
------------- the rolling game starts -------------
The `player` throws two dices and gets `4` point in total.
The `player` throws two dices and gets `4` point in total.
The `player` throws two dices and gets `6` point in total.
The `player` throws two dices and gets `3` point in total.
The `player` throws two dices and gets `7` point in total.
The player loses in 0th player's turn
The `computer` throws two dices and gets `6` point in total.
The `computer` throws two dices and gets `7` point in total.
The computer loses in 1th computer's turn
The `player` throws two dices and gets `9` point in total.
The `player` throws two dices and gets `9` point in total.
The `player` throws two dices and gets `7` point in total.
The player loses in 2th player's turn
The `computer` throws two dices and gets `8` point in total.
The `computer` throws two dices and gets `6` point in total.
The `computer` throws two dices and gets `11` point in total.
The computer wins in 3th computer's turn
The `player` throws two dices and gets `5` point in total.
The `player` throws two dices and gets `5` point in total.
The `player` throws two dices and gets `5` point in total.
The `player` throws two dices and gets `11` point in total.
The player wins in 4th player's turn
The `computer` throws two dices and gets `6` point in total.
The `computer` throws two dices and gets `9` point in total.
The `computer` throws two dices and gets `10` point in total.
The `computer` throws two dices and gets `5` point in total.
The `computer` throws two dices and gets `9` point in total.
The computer wins in 5th computer's turn
The `player` throws two dices and gets `2` point in total.
The `player` throws two dices and gets `7` point in total.
The player loses in 6th player's turn
The `computer` throws two dices and gets `5` point in total.
The `computer` throws two dices and gets `9` point in total.
The `computer` throws two dices and gets `8` point in total.
The `computer` throws two dices and gets `4` point in total.
The `computer` throws two dices and gets `7` point in total.
The computer loses in 7th computer's turn
The `player` throws two dices and gets `4` point in total.
The `player` throws two dices and gets `11` point in total.
The player wins in 8th player's turn
The `computer` throws two dices and gets `7` point in total.
The computer loses in 9th computer's turn
The `player` throws two dices and gets `11` point in total.
The player wins in 10th player's turn
The `computer` throws two dices and gets `7` point in total.
The computer loses in 11th computer's turn
The `player` throws two dices and gets `5` point in total.
The `player` throws two dices and gets `6` point in total.
The `player` throws two dices and gets `10` point in total.
The `player` throws two dices and gets `9` point in total.
The `player` throws two dices and gets `11` point in total.
The player wins in 12th player's turn
The `computer` throws two dices and gets `9` point in total.
The `computer` throws two dices and gets `8` point in total.
The `computer` throws two dices and gets `8` point in total.
The `computer` throws two dices and gets `3` point in total.
The `computer` throws two dices and gets `10` point in total.
The computer wins in 13th computer's turn
The `player` throws two dices and gets `5` point in total.
The `player` throws two dices and gets `4` point in total.
The `player` throws two dices and gets `6` point in total.
The `player` throws two dices and gets `7` point in total.
The player loses in 14th player's turn
The `computer` throws two dices and gets `5` point in total.
The `computer` throws two dices and gets `4` point in total.
The `computer` throws two dices and gets `7` point in total.
The computer loses in 15th computer's turn
The `player` throws two dices and gets `10` point in total.
The `player` throws two dices and gets `8` point in total.
The `player` throws two dices and gets `11` point in total.
The player wins in 16th player's turn
The `computer` throws two dices and gets `4` point in total.
The `computer` throws two dices and gets `9` point in total.
The `computer` throws two dices and gets `8` point in total.
The `computer` throws two dices and gets `4` point in total.
The `computer` throws two dices and gets `3` point in total.
The computer wins in 17th computer's turn
The `player` throws two dices and gets `2` point in total.
The `player` throws two dices and gets `8` point in total.
The `player` throws two dices and gets `5` point in total.
The `player` throws two dices and gets `4` point in total.
The `player` throws two dices and gets `8` point in total.
The player wins in 18th player's turn
The `computer` throws two dices and gets `9` point in total.
The `computer` throws two dices and gets `11` point in total.
The computer wins in 19th computer's turn
The `player` throws two dices and gets `7` point in total.
The player loses in 20th player's turn
The `computer` throws two dices and gets `10` point in total.
The `computer` throws two dices and gets `4` point in total.
The `computer` throws two dices and gets `10` point in total.
The `computer` throws two dices and gets `7` point in total.
The computer loses in 21th computer's turn
The `player` throws two dices and gets `9` point in total.
The `player` throws two dices and gets `9` point in total.
The `player` throws two dices and gets `6` point in total.
The `player` throws two dices and gets `7` point in total.
The player loses in 22th player's turn
The `computer` throws two dices and gets `8` point in total.
The `computer` throws two dices and gets `3` point in total.
The `computer` throws two dices and gets `9` point in total.
The `computer` throws two dices and gets `2` point in total.
The `computer` throws two dices and gets `11` point in total.
The computer wins in 23th computer's turn
The `player` throws two dices and gets `7` point in total.
The player loses in 24th player's turn
The `computer` throws two dices and gets `5` point in total.
The `computer` throws two dices and gets `4` point in total.
The `computer` throws two dices and gets `7` point in total.
The computer loses in 25th computer's turn
The `player` throws two dices and gets `5` point in total.
The `player` throws two dices and gets `5` point in total.
The `player` throws two dices and gets `7` point in total.
The player loses in 26th player's turn
The `computer` throws two dices and gets `8` point in total.
The `computer` throws two dices and gets `8` point in total.
The `computer` throws two dices and gets `5` point in total.
The `computer` throws two dices and gets `7` point in total.
The computer loses in 27th computer's turn
The `player` throws two dices and gets `6` point in total.
The `player` throws two dices and gets `8` point in total.
The `player` throws two dices and gets `6` point in total.
The `player` throws two dices and gets `3` point in total.
The `player` throws two dices and gets `7` point in total.
The player loses in 28th player's turn
The `computer` throws two dices and gets `2` point in total.
The `computer` throws two dices and gets `11` point in total.
The computer wins in 29th computer's turn
The `player` throws two dices and gets `6` point in total.
The `player` throws two dices and gets `8` point in total.
The `player` throws two dices and gets `5` point in total.
The `player` throws two dices and gets `4` point in total.
The `player` throws two dices and gets `7` point in total.
The player loses in 30th player's turn
The `computer` throws two dices and gets `11` point in total.
The computer wins in 31th computer's turn
The `player` throws two dices and gets `8` point in total.
The `player` throws two dices and gets `12` point in total.
The `player` throws two dices and gets `9` point in total.
The `player` throws two dices and gets `8` point in total.
The `player` throws two dices and gets `4` point in total.
The player wins in 32th player's turn
The `computer` throws two dices and gets `12` point in total.
The `computer` throws two dices and gets `8` point in total.
The `computer` throws two dices and gets `6` point in total.
The `computer` throws two dices and gets `5` point in total.
The `computer` throws two dices and gets `8` point in total.
The computer wins in 33th computer's turn
The `player` throws two dices and gets `7` point in total.
The player loses in 34th player's turn
The `computer` throws two dices and gets `6` point in total.
The `computer` throws two dices and gets `8` point in total.
The `computer` throws two dices and gets `8` point in total.
The `computer` throws two dices and gets `9` point in total.
The `computer` throws two dices and gets `6` point in total.
The computer wins in 35th computer's turn
game over
The player wins this championship
------------- the rolling game over -------------
------------- bye bye -------------

```
