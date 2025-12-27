# CH41-1 -- jobs
## objectives
You will learn how to

  + look at jobs of current shell

## CH41-1 -- look at jobs of current shell
`D:\workspace\Bash\Bash tutorial\example scripts\jobs\long-sleep-job.bash`

```
delayed_function() {
    # 模擬 10 秒的等待（這是在背景跑的，不影響你操作）
    sleep 10
    
    echo -e "\n[通知] 10秒已到！delayed_function 開始執行..."
    echo "目前的日期時間是: $(date)"
    
    # 這裡可以放你實際想執行的指令，例如 dotnet build 或通知
    # dotnet build
}

main(){
    # --- 主程式 ---

    echo "腳本已啟動。我們將在背景啟動一個等待 10 秒的任務。"
    echo "這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。"

    # 關鍵點：在呼叫函數後加上 '&' 符號，讓它去背景跑
    delayed_function &

    # 取得剛才丟到背景的 Job ID (這行非必要，僅作示範)
    PID=$!
    echo "背景任務已啟動 (PID: $PID)，你可以試著輸入 'jobs' 查看。"
    echo "--------------------------------------------------------"
}

main
```

using `source "D:\workspace\Bash\Bash tutorial\example scripts\jobs\long-sleep-job.bash"` or `. "D:\workspace\Bash\Bash tutorial\example scripts\jobs\long-sleep-job.bash"` will execute this script in current shell

In this script, it will stimulate to schedule a job that is executed after 10 seconds (in fact, it sleeps 10 seconds but it is executed in background). 

Interaction:

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ . "D:\workspace\Bash\Bash tutorial\example scripts\jobs\long-sleep-job.bash"
腳本已啟動。我們將在背景啟動一個等待 10 秒的任務。
這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。
背景任務已啟動 (PID: 1144)，你可以試著輸入 'jobs' 查看。
--------------------------------------------------------

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ jobs
[1]+  Running                 delayed_function &

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$
[通知] 10秒已到！delayed_function 開始執行...
目前的日期時間是: Sat Dec 27 20:08:21 TST 2025

```

<img width="949" height="220" alt="image" src="https://github.com/user-attachments/assets/65bb375a-bba0-42d2-886b-a44a07e20f93" />
