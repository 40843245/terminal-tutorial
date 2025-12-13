# CH9 -- Regex 
##  objectives
You will learn how to 

  + defines a Regex.

## CH9-1 -- symbol of Regex

| regex symbol | meaing |
| :-- | :-- |
| `.` | any one characters (exclusive to `\n`)|
| `+` | zero or more times (of previous one character or group) |
| `*` | one or more times (of previous one character or group) |
| `?` | zero or one time (of previous one character or group) |

| regex symbol | meaing |
| :-- | :-- |
| `^` | starting from specified characters |
| `$` | end from specified characters |
| `/<` | the end point |
| `>/` | the start point |

| regex symbol | meaing |
| :-- | :-- |
| `[]` | matches one of these characters inside `[]`|
| `-` (inside `[]`) | matches one of these characters that ranges from left-hand side of `-` to right-hand side of `-` inside `[]` |
| `[^ ]` | NOT match one of these characters inside `[]` |

| regex symbol | meaing |
| :-- | :-- |
| `{n}` | matches exactly n times (of previous one character or group) |
| `{n,}` | matches at least n times (of previous one character or group) |
| `{n,m}` | matches at least n times and at most m times (of previous one character or group) |

| regex symbol | meaing |
| :-- | :-- |
| `()` | groups one or more characters or groups into one group |

| regex symbol | meaing |
| :-- | :-- |
| `|` | `OR` logical operator |

## CH9-2 -- matching patterns with Regex
To match patterns with Regex, 

use `=~`.

syntax:

```
<target-patterns> =~ <regular-expression-to-match-patterns>
```

## CH9-3 -- accessing the matching patterns
When matching patterns with Regex successfully, they will be stored into `BASH_REMATCH` special variable

`${BASH_REMATCH[0]}` returns a string of all matching patterns.

`${BASH_REMATCH[1]}` returns the first group of all matching patterns.

`${BASH_REMATCH[2]}` returns the second group of all matching patterns.

`${BASH_REMATCH[3]}` returns the third group of all matching patterns.

etc

## Examples 
### Example 1

```
#!/bin/bash
# 腳本名稱: matching_version.bash

# 讓腳本更健壯
set -euo pipefail

# --- 函數定義 ---

# 顯示使用說明
usage() {
    echo "Usage: $0 [-h] -v `<version_number>`"
    echo "A reusable script for matching a version Regex."
    echo "Options:"
    echo "  -v `<version_number>`  Specify the required version for matching with Regex."
    echo "  -h               Show this help message."
    exit 0
}

# 檢查依賴
check_dependencies() {
    if ! command -v "grep" &> /dev/null; then
        echo "Error: Required command 'grep' is not installed." >&2
        exit 1
    fi
}

# 執行主要動作
perform_action() {
    local input_file="$1"
    
    echo "--- Processing: $input_file ---"
    # 範例：計算檔案中的行數
    local line_count=$(wc -l < "$input_file")
    echo "Total lines found: $line_count"
    
    # 範例：尋找關鍵字
    echo "Lines containing 'TODO':"
    grep -i "TODO" "$input_file" || true # 使用 || true 避免 grep 找不到時觸發 set -e 退出
    
    return 0
}

# --- 參數處理 ---

VERSION_TAG=""

while getopts "v:h" opt; do
    case "${opt}" in
        i)
            VERSION_TAG="${OPTARG}"
            ;;
        h)
            usage
            ;;
        *)
            echo "Invalid option: -${OPTARG}" >&2
            usage
            ;;
    esac
done
shift $((OPTIND-1))

# 驗證輸入
if [[ -z "$VERSION_TAG" ]]; then
    echo "Error: Missing required option -v <input_file>." >&2
    usage
fi

if [[ ! -f "$INPUT_FILE" ]]; then
    echo "Error: Input file '$INPUT_FILE' not found or is not a regular file." >&2
    exit 2
fi


# --- 主程序執行區塊 ---
main() {
VERSION_TAG="v1.2.3-beta"

# 匹配 v<主要版本>.<次要版本>.<修訂版本>
REGEX="^v([0-9]+)\.([0-9]+)\.([0-9]+)"

if [[ $VERSION_TAG =~ $REGEX ]]; then
    echo "整個匹配: ${BASH_REMATCH[0]}"  # 輸出: v1.2.3
    echo "主要版本: ${BASH_REMATCH[1]}"  # 輸出: 1
    echo "次要版本: ${BASH_REMATCH[2]}"  # 輸出: 2
    echo "修訂版本: ${BASH_REMATCH[3]}"  # 輸出: 3
else
    echo "no"
fi
```




