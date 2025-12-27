# CH41-1 -- jobs
## objectives
You will learn how to

  + look at jobs of current shell

## CH41-1 -- look at jobs of current shell
### `jobs`
use `jobs` lists current working, or scheduled jobs of current shell (in fact, it lists current state of jobs mapping table).

### Examples
#### Example 1

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
^C
[1]+  Done                    delayed_function

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$

```

<img width="959" height="241" alt="image" src="https://github.com/user-attachments/assets/4028fe4d-b9c9-4cea-bbe8-e8d9d77d2d91" />

#### Example 2
`D:\workspace\Bash\Bash tutorial\example scripts\jobs\long-sleep-job2.bash`

```
delayed_function() {
    declare -i n=$(("$1"))
    
    # 模擬 n 秒的等待（這是在背景跑的，不影響你操作）
    sleep $n
    
    echo -e "\n[通知] $n 秒已到！delayed_function 開始執行..."
    echo "目前的日期時間是: $(date)"
    
    # 這裡可以放你實際想執行的指令，例如 dotnet build 或通知
    # dotnet build
}

function subfunction(){
    declare -i n=$(("$1"))

    echo "腳本已啟動。我們將在背景啟動一個等待 $n 秒的任務。"
    echo "這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。"

    # 關鍵點：在呼叫函數後加上 '&' 符號，讓它去背景跑
    delayed_function $n &

    # 取得剛才丟到背景的 Job ID (這行非必要，僅作示範)
    PID=$!
    echo "背景任務已啟動 (PID: $PID)，你可以試著輸入 'jobs' 查看。"
    echo "--------------------------------------------------------"
}

main(){
    subfunction 60
    subfunction 50
    subfunction 55
}

main
```

Interactions:

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ . "D:\workspace\Bash\Bash tutorial\example scripts\jobs\long-sleep-job2.bash"
腳本已啟動。我們將在背景啟動一個等待 60 秒的任務。
這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。
背景任務已啟動 (PID: 1194)，你可以試著輸入 'jobs' 查看。
--------------------------------------------------------
腳本已啟動。我們將在背景啟動一個等待 50 秒的任務。
這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。
背景任務已啟動 (PID: 1195)，你可以試著輸入 'jobs' 查看。
--------------------------------------------------------
腳本已啟動。我們將在背景啟動一個等待 55 秒的任務。
這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。
背景任務已啟動 (PID: 1197)，你可以試著輸入 'jobs' 查看。
--------------------------------------------------------

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ jobs
[1]   Running                 delayed_function $n &
[2]-  Running                 delayed_function $n &
[3]+  Running                 delayed_function $n &

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$
[通知] 50 秒已到！delayed_function 開始執行...
目前的日期時間是: Sat Dec 27 20:27:46 TST 2025

[通知] 55 秒已到！delayed_function 開始執行...
目前的日期時間是: Sat Dec 27 20:27:51 TST 2025

[通知] 60 秒已到！delayed_function 開始執行...
目前的日期時間是: Sat Dec 27 20:27:56 TST 2025
^C
[1]   Done                    delayed_function $n
[2]-  Done                    delayed_function $n
[3]+  Done                    delayed_function $n

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$

```

<img width="958" height="422" alt="image" src="https://github.com/user-attachments/assets/e0def5d2-7542-46a4-9c0d-022d7eea654c" />

Interactions:

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ . "D:\workspace\Bash\Bash tutorial\example scripts\jobs\long-sleep-job2.bash"
腳本已啟動。我們將在背景啟動一個等待 60 秒的任務。
這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。
背景任務已啟動 (PID: 1287)，你可以試著輸入 'jobs' 查看。
--------------------------------------------------------
腳本已啟動。我們將在背景啟動一個等待 50 秒的任務。
這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。
背景任務已啟動 (PID: 1288)，你可以試著輸入 'jobs' 查看。
--------------------------------------------------------
腳本已啟動。我們將在背景啟動一個等待 55 秒的任務。
這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。
背景任務已啟動 (PID: 1290)，你可以試著輸入 'jobs' 查看。
--------------------------------------------------------

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ jobs
[1]   Running                 delayed_function $n &
[2]-  Running                 delayed_function $n &
[3]+  Running                 delayed_function $n &

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ fg %1
delayed_function $n

[通知] 50 秒已到！delayed_function 開始執行...
目前的日期時間是: Sat Dec 27 20:44:16 TST 2025

[通知] 55 秒已到！delayed_function 開始執行...
目前的日期時間是: Sat Dec 27 20:44:21 TST 2025

[通知] 60 秒已到！delayed_function 開始執行...
目前的日期時間是: Sat Dec 27 20:44:26 TST 2025
[2]   Done                    delayed_function $n
[3]   Done                    delayed_function $n

```

<img width="950" height="385" alt="image" src="https://github.com/user-attachments/assets/11d91111-6698-4305-91e4-8d4c260eaea7" />

Interactions

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ . "D:\workspace\Bash\Bash tutorial\example scripts\jobs\long-sleep-job2.bash"
腳本已啟動。我們將在背景啟動一個等待 60 秒的任務。
這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。
背景任務已啟動 (PID: 1326)，你可以試著輸入 'jobs' 查看。
--------------------------------------------------------
腳本已啟動。我們將在背景啟動一個等待 50 秒的任務。
這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。
背景任務已啟動 (PID: 1327)，你可以試著輸入 'jobs' 查看。
--------------------------------------------------------
腳本已啟動。我們將在背景啟動一個等待 55 秒的任務。
這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。
背景任務已啟動 (PID: 1329)，你可以試著輸入 'jobs' 查看。
--------------------------------------------------------

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ jobs
[1]   Running                 delayed_function $n &
[2]-  Running                 delayed_function $n &
[3]+  Running                 delayed_function $n &

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ bg %1
bash: bg: job 1 already in background

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ bg %2
bash: bg: job 2 already in background

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ bg %3
bash: bg: job 3 already in background

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ fg %1
delayed_function $n
bg %1
bg %1

[通知] 50 秒已到！delayed_function 開始執行...
目前的日期時間是: Sat Dec 27 20:48:45 TST 2025

[通知] 55 秒已到！delayed_function 開始執行...
目前的日期時間是: Sat Dec 27 20:48:50 TST 2025

[通知] 60 秒已到！delayed_function 開始執行...
目前的日期時間是: Sat Dec 27 20:48:55 TST 2025
[2]   Done                    delayed_function $n
[3]   Done                    delayed_function $n

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ bg %1
bash: bg: %1: no such job

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ bg %1
bash: bg: %1: no such job

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$

```

<img width="949" height="353" alt="image" src="https://github.com/user-attachments/assets/5b2ca741-fcdd-4e1c-b929-e0356e550094" />

<img width="939" height="272" alt="image" src="https://github.com/user-attachments/assets/016ff262-4f23-4d82-b741-98c611a12b6a" />

Interactions:

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ . "D:\workspace\Bash\Bash tutorial\example scripts\jobs\long-sleep-job2.bash"
腳本已啟動。我們將在背景啟動一個等待 60 秒的任務。
這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。
背景任務已啟動 (PID: 1372)，你可以試著輸入 'jobs' 查看。
--------------------------------------------------------
腳本已啟動。我們將在背景啟動一個等待 50 秒的任務。
這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。
背景任務已啟動 (PID: 1373)，你可以試著輸入 'jobs' 查看。
--------------------------------------------------------
腳本已啟動。我們將在背景啟動一個等待 55 秒的任務。
這期間你可以繼續在終端機輸入其他指令，或是查看 jobs。
背景任務已啟動 (PID: 1375)，你可以試著輸入 'jobs' 查看。
--------------------------------------------------------

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ jobs
[1]   Running                 delayed_function $n &
[2]-  Running                 delayed_function $n &
[3]+  Running                 delayed_function $n &

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ fg %1
delayed_function $n

[1]+  Stopped                 delayed_function $n

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ bg %1
[1]+ delayed_function $n &

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ jobs
[1]   Running                 delayed_function $n &
[2]-  Running                 delayed_function $n &
[3]+  Running                 delayed_function $n &

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ bg %1
bash: bg: job 1 already in background

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ jobs
[通知] 50 秒已到！delayed_function 開始執行...
目前的日期時間是: Sat Dec 27 20:57:56 TST 2025

[通知] 55 秒已到！delayed_function 開始執行...
目前的日期時間是: Sat Dec 27 20:58:01 TST 2025

[通知] 60 秒已到！delayed_function 開始執行...
目前的日期時間是: Sat Dec 27 20:58:17 TST 2025
^C
[1]   Done                    delayed_function $n
[2]-  Done                    delayed_function $n
[3]+  Done                    delayed_function $n

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ jobs

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$

```

<img width="944" height="433" alt="image" src="https://github.com/user-attachments/assets/b95e3880-1a35-45c1-8665-387cc2d001e6" />

<img width="951" height="359" alt="image" src="https://github.com/user-attachments/assets/c2a138b5-854a-4e2e-9585-ceb3b56660b5" />

## CH41-2 -- switch a job to foreground or background
```
fg %{job-number}
```

where 

`{job-number}` is the job number id, wrapped with `[]` when displaying jobs mapping table

will switch a background job with id `{job-number}` to a foreground job 

```
bg %{job-number}
```

where 

`{job-number}` is the job number id, wrapped with `[]` when displaying jobs mapping table

will continue to executed a stopped job with id `{job-number}` as a background job

Type this command then entering `Ctrl` + `z`

```
%{job-number}
```

where 

`{job-number}` is the job number id, wrapped with `[]` when displaying jobs mapping table

will stop this job with id `{job-number}`

See example 2 in CH41-1 for example
