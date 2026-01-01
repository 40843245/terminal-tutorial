# CH45 -- event handler
## objectives
You will learn how to

    + set and unset an event handler in Bash.

## CH45-1 -- set and unset an event handler
To set an event handler, use `trap` built-in command.

syntax:

```
trap {commands} ({PPID}|{signal-list})
```

where

```
{commands}: one or more commands.
{PPID}: Process ID
{signal-list}: a list of signal (each signal is considered as a event handler) that triggers the event.
```

To unset the event handler, use `trap` to set the event handler as empty (with `-`)

To list all event handlers, use `trap -p`.

### Examples
#### Example 1
`trap-example-1.bash`

```
main(){
    TEMP_FILE="/d/workspace/Bash/Bash tutorial/temp/temp-script.txt"

    # 設定 trap：當收到 SIGINT (Ctrl+C), SIGTERM 或腳本正常結束 (EXIT) 時，執行 rm    
    trap "echo '正在清理暫存檔...'; rm -f \"$TEMP_FILE\"; exit" SIGINT SIGTERM EXIT

    echo "正在處理資料並寫入 \"$TEMP_FILE\"..."
    touch "$TEMP_FILE"

    # 模擬長時間運作
    sleep 10

    echo "作業完成！"
}

main
```

executing this script will echo

```
正在處理資料並寫入 "/d/workspace/Bash/Bash tutorial/temp/temp-script.txt"...
```

and create `/d/workspace/Bash/Bash tutorial/temp/temp-script.txt` file

then echo

```
作業完成！
```

after 10 seconds 

then immediately trigger the event and thus executes

```
    echo '正在清理暫存檔...'; rm -f \"$TEMP_FILE\"; exit
```

which echos

```
正在清理暫存檔...
```

and remove `/d/workspace/Bash/Bash tutorial/temp/temp-script.txt`

### Example 2
`trap-example-2.bash`

```
TEMP_FILE="/d/workspace/Bash/Bash tutorial/temp/temp-script.txt"

function on_exit(){
    echo '正在清理暫存檔...'; 
    rm -f "$TEMP_FILE"; 
    exit
}

main(){
    # 設定 trap：當收到 SIGINT (Ctrl+C), SIGTERM 或腳本正常結束 (EXIT) 時，執行 on_exit function
    trap 'on_exit' SIGINT SIGTERM EXIT

    echo "正在處理資料並寫入 \"$TEMP_FILE\"..."
    touch "$TEMP_FILE"

    # 模擬長時間運作
    sleep 10

    echo "作業完成！"
}

main
```

executing this script will echo

```
正在處理資料並寫入 "/d/workspace/Bash/Bash tutorial/temp/temp-script.txt"...
```

and create `/d/workspace/Bash/Bash tutorial/temp/temp-script.txt` file

then echo

```
作業完成！
```

after 10 seconds 

then immediately trigger the event and thus executes `on_exit` function

which echos

```
正在清理暫存檔...
```

and remove `/d/workspace/Bash/Bash tutorial/temp/temp-script.txt`