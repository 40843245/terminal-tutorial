# CH20 -- usage
## objectives
You will learn how to

  + override the helper function to display help message

## CH20-1 -- override the helper function to display help message
### usage
You can override the helper function by defining a function named `usage` at top level

structure:

```
usage(){
  # statements
  # echo help message to terminal
}
```


and invoke the `usage` function with`getopt` reserved word.

example structure

```
# 處理命令行參數的主函數
parse_args() {
    local version_input=""
    
    # 使用 getopts 解析選項： "v:" 表示 -v 接受一個參數
    while getopts "v:" opt; do
        case "${opt}" in
            v)
                version_input="${OPTARG}"
                ;;
            *)
                # 如果遇到未知選項，顯示 usage
                usage
                ;;
        esac
    done
    
    # 驗證 -v 參數是否提供
    if [[ -z "$version_input" ]]; then
        echo "Error: The required option -v <version_number> is missing." >&2
        usage
    fi
}
```


#### Examples
##### Example 1
see Example 2 in CH14-2 for full content.

In `regex-example-2.bash`,

```
# 處理命令行參數的主函數
parse_args() {
    local version_input=""
    
    # 使用 getopts 解析選項： "v:" 表示 -v 接受一個參數
    while getopts "v:" opt; do
        case "${opt}" in
            v)
                version_input="${OPTARG}"
                ;;
            *)
                # 如果遇到未知選項，顯示 usage
                usage
                ;;
        esac
    done
    
    # 驗證 -v 參數是否提供
    if [[ -z "$version_input" ]]; then
        echo "Error: The required option -v <version_number> is missing." >&2
        usage
    fi
    
    # 呼叫主要邏輯函數
    parse_and_display "$version_input"
}
```

`parse_args` function parses and handles the arguments first.

Case 1: If the end-user enters flag `-v`

Case 1.A: If the end-user enters flag `-v` followed by exactly one argument (separated by delimiter a whitespace)

it can be parsed correctly then invoke `parse_and_display` function with one argument after `-v` flag (separated by delimiter a whitespace)

Case 1.B: If the end-user enters flag `-v`, BUT NOT followed by one argument (or more)

it can't be parsed correctly, so it doesn't invoke `parse_and_display` function.

Due to 

```
getopts "v:"
```

requires one argument

and it violates the rule, 

Bash engine will echo an error message saying like

```
D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash: option requires an argument -- v
```

then automatically executes the helper function `usage`.

Case 1.C: If the end-user enters flag `-v`, followed by more than one argument

it can be parsed correctly then invoke `parse_and_display` function with one argument after `-v` flag (separated by delimiter a whitespace). 

The rest arguments will be ignored here.

Case 1.D: If the end-user enters flag -v`, BUT followed by an empty string

The Bash engine can find an option `-v`, thus executing

```
version_input="${OPTARG}"
```

in context of

```
            v)
                version_input="${OPTARG}"
                ;;
```

However, the passed argument is an empty string, thus

`version_input` is assigned to an empty string, causing it executes the block 

```
        echo "Error: The required option -v <version_number> is missing." >&2
        usage
```

in context of 

```
    # 驗證 -v 參數是否提供
    if [[ -z "$version_input" ]]; then
        echo "Error: The required option -v <version_number> is missing." >&2
        usage
    fi
```

echoing the `"Error: The required option -v <version_number> is missing."` to `stderr` (standard error output stream)

and invoking helper function `usage`.

Case 2: If the end-user enters **other flag than `-v`** (for example `-x`), BUT followed by an empty string

Then it can't recognize the option `x`

since it only can recognize the option `v` 

according to `"v:"`

in context of

```
getopts "v:"
```

Thus, echoing error message like

```
D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash: illegal option -- x
```

Then executes

```
usage
```

in context of

```
            *)
                # 如果遇到未知選項，顯示 usage
                usage
                ;;
```

invoking helper function `usage`.

User interactions:

```
<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash" -v "v1.2.3"
--- 命名引用成功解析 (vX.Y.Z) ---
整個匹配: v1.2.3
主要版本: 1
次要版本: 2
修訂版本: 3
-----------------------------------

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash" -v "v1.2.3.4"
--- 命名引用成功解析 (vX.Y.Z) ---
整個匹配: v1.2.3
主要版本: 1
次要版本: 2
修訂版本: 3
-----------------------------------

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash" -v "v.1.2.3"
--- 解析失敗 (錯誤訊息) ---
this is NOT a version number: v.1.2.3
-----------------------------------

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash" -v "v1.2"
--- 解析失敗 (錯誤訊息) ---
this is NOT a version number: v1.2
-----------------------------------

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash" -v "v1"
--- 解析失敗 (錯誤訊息) ---
this is NOT a version number: v1
-----------------------------------

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash" -v "1.2.3"
--- 解析失敗 (錯誤訊息) ---
this is NOT a version number: 1.2.3
-----------------------------------

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash" -v ""
Error: The required option -v <version_number> is missing.
Usage: D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash -v <version_number>
A script for matching and parsing vX.Y.Z version tags.
Options:
  -v <version_number>  Specify the version tag to parse (e.g., v1.2.3).

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash" -v
D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash: option requires an argument -- v
Usage: D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash -v <version_number>
A script for matching and parsing vX.Y.Z version tags.
Options:
  -v <version_number>  Specify the version tag to parse (e.g., v1.2.3).

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash" -v --help
--- 解析失敗 (錯誤訊息) ---
this is NOT a version number: --help
-----------------------------------

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash" -v "v1.2.3" "v.1.2.3"
--- 命名引用成功解析 (vX.Y.Z) ---
整個匹配: v1.2.3
主要版本: 1
次要版本: 2
修訂版本: 3
-----------------------------------

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash" -x "v1.2.3" "v.1.2.3"
D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash: illegal option -- x
Usage: D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash -v <version_number>
A script for matching and parsing vX.Y.Z version tags.
Options:
  -v <version_number>  Specify the version tag to parse (e.g., v1.2.3).

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash"
Error: The required option -v <version_number> is missing.
Usage: D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash -v <version_number>
A script for matching and parsing vX.Y.Z version tags.
Options:
  -v <version_number>  Specify the version tag to parse (e.g., v1.2.3).

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash" x
Error: The required option -v <version_number> is missing.
Usage: D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash -v <version_number>
A script for matching and parsing vX.Y.Z version tags.
Options:
  -v <version_number>  Specify the version tag to parse (e.g., v1.2.3).

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$ "D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash" -x
D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash: illegal option -- x
Usage: D:\workspace\Bash\Bash tutorial\examples\Regex\regex-example-2.bash -v <version_number>
A script for matching and parsing vX.Y.Z version tags.
Options:
  -v <version_number>  Specify the version tag to parse (e.g., v1.2.3).

<user-name>@<device-name> MINGW64 /d/workspace/Bash/Bash tutorial/examples/Regex
$

```
