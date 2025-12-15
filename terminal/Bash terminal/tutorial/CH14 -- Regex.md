# CH14 -- Regex 
##  objectives
You will learn how to 

  + defines a Regex.

## CH14-1 -- symbol of Regex

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

## CH14-2 -- matching patterns with Regex
To match patterns with Regex, 

use `=~`.

syntax:

```
<target-patterns> =~ <regular-expression-to-match-patterns>
```

## CH14-3 -- accessing the matching patterns
When matching patterns with Regex successfully, they will be stored into `BASH_REMATCH` special variable

`${BASH_REMATCH[0]}` returns a string of all matching patterns.

`${BASH_REMATCH[1]}` returns the first group of all matching patterns.

`${BASH_REMATCH[2]}` returns the second group of all matching patterns.

`${BASH_REMATCH[3]}` returns the third group of all matching patterns.

etc

## Examples 
### Example 1
The following example illustrates how to matching a version

utility module:

`matching_version_module_nameref.bash`

```
#!/bin/bash
# 模組名稱: matching_version_module_nameref.bash
# ...

# 匹配版本的函式 (使用命名引用回傳多個值)
# 參數：
#   $1 - 待檢查的版本字串
#   $2 - 變數名稱 (用於回傳整個匹配)
#   $3 - 變數名稱 (用於回傳主要版本)
#   $4 - 變數名稱 (用於回傳次要版本)
#   $5 - 變數名稱 (用於回傳修訂版本)
matching_version_nameref() {
    local version_tag="$1"
    local version_regex="^v([0-9]+)\.([0-9]+)\.([0-9]+)"

    # 設置命名引用，讓這些局部變數指向呼叫者提供的外部變數
    declare -n ref_full_match="$2"
    declare -n ref_major="$3"
    declare -n ref_minor="$4"
    declare -n ref_patch="$5"
    declare -n ref_error_message="$6"

    if [[ "$#" -ne 6 ]]; then
        ref_error_message="Error: requires 6 arguments (tag + 4 namerefs + error_message)."
        return 1
    fi
    
    if [[ $version_tag =~ $version_regex ]]; then
        # 透過引用直接將值賦予到呼叫者的變數
        ref_full_match="${BASH_REMATCH[0]}"
        ref_major="${BASH_REMATCH[1]}"
        ref_minor="${BASH_REMATCH[2]}"
        ref_patch="${BASH_REMATCH[3]}"
        ref_error_message=""
        return 0
    else
        # 匹配失敗時，清空變數
        ref_full_match=""
        ref_major=""
        ref_minor=""
        ref_patch=""
        ref_error_message="this is NOT a version number: $version_tag"
        return 1
    fi
}
```

main script:

`regex-example-1.bash`

```
#!/bin/bash
# 腳本名稱: regex-example-1.bash

source "../../utility modules/Regex/matching_version_module_nameref.bash"

# 1. 在呼叫者腳本中定義變數


parse_and_display(){
    local full_version_tag=""
    local major_version_number=""
    local minor_version_number=""
    local patch_version_number=""
    local error_message=""
    matching_version_nameref "$@" full_version_tag major_version_number minor_version_number patch_version_number error_message

    if [ "$?" -eq 0 ]; then
        echo "--- 命名引用成功解析 ---"
        # 3. 直接使用已賦值的變數
        echo "整個匹配: $full_version_tag"
        echo "主要版本: $major_version_number"
        echo "次要版本: $minor_version_number"
        echo "修訂版本: $patch_version_number"
    elif [[ "$error_message" != "" ]]; then
        echo $error_message
    else
        echo "命名引用解析失敗。"
    fi
}

parse_and_display "v1.2.3"
parse_and_display "v1.2.3.4"
parse_and_display "v.1.2.3"
parse_and_display "1.2."
parse_and_display "1.2"
parse_and_display "1"
```

it will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-1.bash"
--- 命名引用成功解析 ---
整個匹配: v1.2.3
主要版本: 1
次要版本: 2
修訂版本: 3
--- 命名引用成功解析 ---
整個匹配: v1.2.3
主要版本: 1
次要版本: 2
修訂版本: 3
this is NOT a version number: v.1.2.3
this is NOT a version number: 1.2.
this is NOT a version number: 1.2
this is NOT a version number: 1

```
