# CH13 -- set and unset a variable
## objectivs
You will know how to 

  + set a value of an variable
  + unset a variable
  + check a variable is unset

## CH13-1 -- set a value of an variable
Once after declaring a variable, the value of the variable is determined, 

even if you don't intialize the variable.

> [!IMPORTANT]
> If you don't initalize value when declaring (i.e. just type an identifier),
>
> the variable is set to NULL value.

After declaring a variable, you can set a value by assign a value with assignment operator `=`.

See [CH2-1 -- define a variable](https://github.com/40843245/terminal-tutorial/blob/main/terminal/Bash%20terminal/tutorial/CH2%20--%20variables.md#ch2-1----define-a-variable) and

[CH2-3 -- assign a value to a variable](https://github.com/40843245/terminal-tutorial/blob/main/terminal/Bash%20terminal/tutorial/CH2%20--%20variables.md#ch2-3----assign-a-value-to-a-variable)

for more details.

## CH13-2 -- unset a variable
### unset
Once the variable is set, you can only unset it by `unset` built-in command.

syntax:

```
unset <variable-name>
```

> [!WARNING]
> NEVER `unset $<variable-name>`.
>
> Otherwise, it will expand the `<variable-name>`, substituting the value of variable name.
>
> Causing unset a value which is a strange behavior. 

> [!IMPORTANT]
> To unset a global variable using `unset` built-in command inside a function will affect the result after invoking it.
>
> But to unset a global variable using `unset` built-in command inside a function will NOT affect the result after invoking it.
>
> see CH2 for more details

#### Examples
##### Example 1

```
variable4=2
unset $variable4
```

After expansion, it is equivalent to

```
variable4=2
unset 2 # <-- nothing will be unset
```

## CH13-3 -- check a variable is unset
### expand it through `${<variable-name>+set}`
Expanding it through `${<variable-name>+set}` will check the variable name `<variable-name>` is unset.

If the result is an empty string, it MUST be in unset state (that is it does NOT exist).

Otherwise, it is in set state (BUT its value may be an empty string)

> [!IMPORTANT]
> It is highly recommended relative to the approach in next section.

#### Examples
##### Example 1

`unset-example-1.bash`

```
is_truly_unset(){
    local arg0_name="$1"
    if [ -z "${!arg0_name+set}" ]; then
        echo "$arg0_name is in unset state"
        return 0
    else
        echo "$arg0_name is NOT in unset state (its value may be an empty string)"
        return 1
    fi
}

variable2=""

variable3=2

variable4=2
unset variable4

variable5=2
unset variable5
variable5=3

is_truly_unset variable1
is_truly_unset variable2
is_truly_unset variable3
is_truly_unset variable4
is_truly_unset variable5

```

executing this script will echo

```
variable1 is in unset state
variable2 is NOT in unset state (its value may be an empty string)
variable3 is NOT in unset state (its value may be an empty string)
variable4 is in unset state
variable5 is NOT in unset state (its value may be an empty string)

```

### expand it through `${<variable-name>-}`
Expanding it through `${<variable-name>-}` will check it the variable name `<variable-name>` is unset **or it is an empty string**.

If the result is an empty string, it may be in unset state (that is it does NOT exist) or it is in set state (BUT its value is an empty string).

Otherwise, it is in set state (AND its value is NOT an empty string)

> [!IMPORTANT]
> It is NOT highly recommended relative to the approach in next section.

#### Examples
##### Example 1

`unset-example-2.bash`

```
is_unset_or_empty_string(){
    local arg0_name="$1"
    if [ -z "${!arg0_name-}" ]; then
        echo "Either $arg0_name is in unset state or its in set state but it value is NOT an empty string"
        return 0
    else
        echo "$arg0_name is NOT in unset state and its value is NOT an empty string"
        return 1
    fi
}

variable2=""

variable3=2

variable4=2
unset variable4

variable5=2
unset variable5
variable5=3

is_unset_or_empty_string variable1
is_unset_or_empty_string variable2
is_unset_or_empty_string variable3
is_unset_or_empty_string variable4
is_unset_or_empty_string variable5

```

executing this script will echo

```
Either variable1 is in unset state or its in set state but it value is NOT an empty string
Either variable2 is in unset state or its in set state but it value is NOT an empty string
variable3 is NOT in unset state and its value is NOT an empty string
Either variable4 is in unset state or its in set state but it value is NOT an empty string
variable5 is NOT in unset state and its value is NOT an empty string

```

## CH13-4 -- extra bonus
### Q1 -- check a variable is in unset state and it has been unset by `unset` command
Q1: 

How check a variable is in unset state and it has been unset by `unset` command, returning 0 (boolean true) if the conditions are BOTH satisfied?

A1:

You have to define a global variable (dictionary type) to trace that the variable has been unset by `unset` command.

To do that, first you have to override the `unset` command by defining function named `unset`

In `unset` function, you have to set the argument name as its key and set its value as any value you want.

Then invoke `unset` built-in command.

Secondly, when checking, you have to check the argument is in unset state and it is in the dictory.

#### Examples
##### Example 1

a utility module:

`tracing_unset_state_module.bash`

```
# !/bin/bash
# 模組名稱:tracing_unset_state_module.bash

### 模組目的
### 記錄該變數曾因`unset`指令而被unset過且正在處於unset狀態 (i.e.是否不存在)

## 函式目的
## 使用全域陣列，用於記錄被 'unset' 過的變數名稱
## 變數名稱將作為陣列的鍵 (key)，值 (value) 不重要，只要存在即可。
declare -gA UNSET_HISTORY

# 確保這個函式名為 unset，以便覆蓋內建命令
unset() {
    # 遍歷所有傳遞給這個自定義 unset 函式的參數 (即變數名稱)
    for var_name in "$@"; do
        # 1. 記錄變數名稱到追蹤陣列
        UNSET_HISTORY["$var_name"]=1
        # 2. 執行內建的 unset 命令
        command unset "$var_name" # 執行內建的 unset 命令
    done
}

## ------------- 主要函式 ------------- ##
## 函式目的
## 判斷該變數曾因`unset`指令而被unset過且正在處於unset狀態 (i.e.是否不存在)
## 
## 回傳值
## 若該變數曾因`unset`指令而被unset過且正在處於unset狀態，則回傳0(boolean true)。
## 否則，則回傳1(boolean false)。
##
## 詳細情況分析
##
## Case 1:正在處於unset狀態
## Case 1.1: 若該變數曾因`unset`指令而被unset過
## => 回傳0
## Case 1.2: 若該變數未曾因`unset`指令(example use case:直接嘗試存取未宣告過的變數)
## => 回傳1
## Case 2: 正在處於set狀態
## > [!NOTE]
## > 若該變數沒有被宣告而不存在的話，則也屬於Case 1.2，而回傳1(boolean false)。

is_unset_preserved_word() {
    local var_name="$1"

    # 條件 1: 檢查變數是否處於 UNSET 狀態
    if [ -z "${!var_name+set}" ]; then
        # Case 1:正在處於unset狀態
        # 注意: 使用 ${!var_name+set} 進行間接參數展開
        # 條件 2: 檢查變數名稱是否在 UNSET_HISTORY 中
        if [[ -v UNSET_HISTORY["$var_name"] ]]; then
            # Case 1.1: 若該變數曾因`unset`指令而被unset過 => 回傳0
            echo "$var_name: 因\`unset\` preserved word而被unset過且目前是unset狀態"
            return 0
        else
            # Case 1.2 未曾設定，也未曾呼叫 unset => 回傳1
            echo "$var_name: 目前是unset狀態，但沒有因呼叫內建的\`unset\`指令而被追蹤為'被unset過' (例如: 從未設定過)"
            return 1
        fi
    else
        # Case 2: 變數目前處於 SET 狀態 => 回傳1
        echo "$var_name: 目前是 set 狀態"
        return 1
    fi 
}
```

main script:

`unset-example-3.bash`

```
source "../../utility modules/unset/tracing_unset_state_module.bash"

variable1=""
declare variable2 # 顯式宣告 variable2 處於 unset 狀態
variable3=2
unset variable3 # 觸發自定義 unset，記錄 variable3

variable4=2
unset variable4 # 觸發自定義 unset，記錄 variable4
variable4=3     # 重新設定 variable4

echo "--- 執行判斷 ---"

is_unset_preserved_word variable1 # expected returns 1 (set)
is_unset_preserved_word variable2 # expected returns 1 (unset, but not tracked)
is_unset_preserved_word variable3 # expected returns 0 (tracked, and is unset)
is_unset_preserved_word variable4 # expected returns 1 (set)

echo "--- UNSET_HISTORY 追蹤紀錄 ---"
# variable3 和 variable4 會被記錄
declare -p UNSET_HISTORY
```

executing the main script will echo

```
--- 執行判斷 ---
variable1: 目前是 set 狀態
variable2: 目前是unset狀態，但沒有因呼叫內建的`unset`指令而被追蹤為'被unset過' (例如: 從未設定過)
variable3: 因`unset` preserved word而被unset過且目前是unset狀態
variable4: 目前是 set 狀態
--- UNSET_HISTORY 追蹤紀錄 ---
declare -A UNSET_HISTORY=([variable3]="1" [variable4]="1" )

```

see CH15 for overriding a built-in command.
