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
`!/bin/bash/matching_version.bash`

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

# 匹配版本的函式 (version number format:`v<主要版本>.<次要版本>.<修訂版本>`)
matching_version() {

    if [[ "$#" -ne 1 ]]; then
            echo "Error: $func_name requires exactly 1 argument (version number)." >&2
            return 1
    fi
        
    # 匹配 v<主要版本>.<次要版本>.<修訂版本>
    local version_regex="^v([0-9]+)\.([0-9]+)\.([0-9]+)"
    
    if [[ $VERSION_TAG =~ $version_regex ]]; then
        echo "整個匹配: ${BASH_REMATCH[0]}"  # 輸出: v1.2.3
        echo "主要版本: ${BASH_REMATCH[1]}"  # 輸出: 1
        echo "次要版本: ${BASH_REMATCH[2]}"  # 輸出: 2
        echo "修訂版本: ${BASH_REMATCH[3]}"  # 輸出: 3
    else
        echo "this is NOT a version number"
    fi
    
    return 0
}

parse_args() {
    # 修正賦值語法
    local version_tag="" 

    # 重設 OPTIND，確保函數可以被多次呼叫 (雖然此腳本中不需要)
    OPTIND=1 

    while getopts "v:h" opt; do
        case "${opt}" in
            v)
                # 這裡不需要 option_code，直接設置 version_tag
                version_tag="${OPTARG}"
                ;;
            h)
                usage # usage 函數會退出
                ;;
            *)
                echo "Invalid option: -${OPTARG}" >&2
                usage
                ;;
        esac
    done
    
    # 清理已處理的選項，但這次不需要
    # shift $((OPTIND-1)) 
    
    # 執行必要的參數驗證和主要功能呼叫
    if [[ -z "$version_tag" ]]; then
        echo "Error: The required option -v <version_number> is missing." >&2
        usage # 缺少參數時顯示 usage 並退出
    fi

    # 呼叫主要功能，並傳遞 version_tag 區域變數的值
    matching_version "$version_tag"
}
```

Then, in Bash terminal, the output will be in these interactions.

```
# grant it for permission
chmod +x matching_version.bash
```

enter

```
./matching_version.bash -v v1.2.3
```

output

```
整個匹配: v1.2.3
主要版本: 1
次要版本: 2
修訂版本: 3
```

enter

```
./matching_version.bash -v 1.2.3
```

ouput

```
this is NOT a version number
```
