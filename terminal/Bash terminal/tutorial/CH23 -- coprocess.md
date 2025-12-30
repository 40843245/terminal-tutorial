# CH23 -- coprocess.md
## objectives
You will learn how to

    + use coprocess

## CH23-1 -- rationales
a coprocess (using `coproc` built-in command) uses IPC (stands for Inter-Process Communication)

At the startup of Bash terminal, two pipes will be created by kernel:

one for reading from main shell, writting from its subshell, that's called `pipe-A`.

the another one for writing from main shell, reading from its subshell, that's called `pipe-B`.

In each pipe, there are two nodes: one for write (like server-side), the another one for reading (like client-side).

When `coproc` command is executed,

Bash will invoke `fork()` function.

When `fork()` function is invoked,

one subshell is created.

Here, the shell and its subshell BOTH have fd (stands for file descriptor)

Its subshell will connect its stdin (stands for standards input stream) which uses `fd[0]` to the reading-side (like client-side) of `pipe-B`.

Also it will connect its stdout (stands for standards output stream) which uses `fd[1]` to the writting-side (like server-side) of `pipe-A`.

After that, it will immediately close unncessary nodes.

Lastly, Bash engine binds these nodes 

    + writting-side (like server-side) of `pipe-A` and 
    + reading-side (like client-side) of `pipe-B`) 
    
to the coprocess name (by default, it is `COPROC`) as a variable name (with indexed-array type). The indexed array can be accessed in the parent shell:

| nodes | binds to | accessed by |
| :-- | :-- | :-- |
| writting-side (like server-side) of `pipe-A` | `${COPROC[0]}` | can be accessed by parent shell |
| reading-side (like client-side) of `pipe-B` | `${COPROC[1]}` | can be accessed by parent shell |

> [!CRITICAL]
> Deadlock might occur.
> 
> So, Developer **MUST** prevent this situation.
> 
> NEVER let a parent shell waits for its subshell and its subshell waits for the parent shell simultaneously.

## CH23-2 -- usage
To create a coprocess, use `coproc`

syntax:

There are these forms. Represented with RE

```
coproc {NAME}? {command} {redirections}?
coproc {NAME} {left-curl-bracket}{whitespaces}{commands}{colon}{whitespaces}{right-curl-bracket}
coproc {NAME} {compound-command}
coproc {compound-command}
coproc {simple-command}
```

where

```
{left-curl-bracket}: left curl bracket `{`
{right-curl-bracket}: right curl bracket `}`
{whitespaces}:= {whitespace}+
{whitespace}: a whitespace ` `
```

```
{NAME}: name of coprocess
{redirections}: one or more command about redirection, see [redirection in GNU offical docs](https://www.gnu.org/software/bash/manual/bash.html#Redirections) 
{commands}: one or more commands
{compound-command}: a compound command discussed at [compound command in GNU offical docs](https://www.gnu.org/software/bash/manual/bash.html#Compound-Commands)
{simple-command}: a simple command discussed at [simple command in GNU offical docs](https://www.gnu.org/software/bash/manual/bash.html#Simple-Commands)
```

The recommend form is 2th form

```
coproc {NAME} {left-curl-bracket}{whitespaces}{command}{colon}{whitespaces}{right-curl-bracket}
```

> [!NOTE]
> In 2th form, it is needed to specify the coprocess name.

> [!NOTE]
> In 2th form, it is needed to append `;` at the end of the command without leaving any whitespace, and be wrapped with `{}` with leaving whitespaces.

> [!NOTE]
> If you specify the name of coproccess `{NAME}`, you also need to specify compound command or `{ ...; }`. Otherwise, it is treated as command that will be executed.
>
> according 2th and 3th form.

## Examples
### Example 1
`coprocess-example-1.bash`

```
# 1. 啟動一個名為 "CALC" 的協程執行 bc
# 注意：一定要用 { ...; } 且內部要有分號
coproc CALC { bc --quiet; }

# 2. 向協程發送一個數學運算式
# 使用 >&"${CALC[1]}" 將資料導向協程的輸入端
echo "10 + 20 * 2" >&"${CALC[1]}"

# 3. 從協程的輸出端讀取結果
# 使用 <&"${CALC[0]}" 從協程的輸出端讀取
read result <&"${CALC[0]}"

echo "計算結果是: $result"

# 4. 再次發送不同運算
echo "scale=2; 10 / 3" >&"${CALC[1]}"
read result <&"${CALC[0]}"

echo "精確計算結果是: $result"

# 5. 結束協程（發送 quit 給 bc）
echo "quit" >&"${CALC[1]}"
```

executing this script will echo

```
$ source "D:\workspace\Bash\Bash tutorial\examples\coprocess\coprocess-example-1.bash"
計算結果是: 50
精確計算結果是: 3.33
[1]+  Done                    coproc CALC { bc --quiet; }

```

### Example 2
`coprocess-example-2.bash`

```
# 啟動一個模擬資料庫的協程
# 它會無限循環等待輸入，並根據輸入回傳資料
coproc DB_ENGINE {
    while read line; do
        if [ "$line" == "GET_USER_1" ]; then
            echo "User: Alice, Role: Admin"
        elif [ "$line" == "GET_USER_2" ]; then
            echo "User: Bob, Role: Editor"
        else
            echo "Error: Unknown Command"
        fi
    done
}

## utility function
## 主要目的
## 包裝協程的讀寫邏輯
query_db() {
    local cmd=$1
    echo "$cmd" >&"${DB_ENGINE[1]}"  # 寫入指令
    read response <&"${DB_ENGINE[0]}" # 讀取回應
    echo "DB回應: $response"
}

# 在腳本各處隨時呼叫
query_db "GET_USER_1"
query_db "GET_USER_2"
query_db "INVALID_CMD"

# 關閉協程的寫入端，這會讓協程內的 while 迴圈結束
exec {DB_ENGINE[1]}>&-
```

executing this script will echo

```
$ source "D:\workspace\Bash\Bash tutorial\examples\coprocess\coprocess-example-2.bash"
DB回應: User: Alice, Role: Admin
DB回應: User: Bob, Role: Editor
DB回應: Error: Unknown Command
[1]+  Done                    coproc DB_ENGINE { while read line; do
    if [ "$line" == "GET_USER_1" ]; then
        echo "User: Alice, Role: Admin";
    else
        if [ "$line" == "GET_USER_2" ]; then
            echo "User: Bob, Role: Editor";
        else
            echo "Error: Unknown Command";
        fi;
    fi;
done; }

```