# CH44 -- Here-string
## objectives
You will know the Here-string techinuqe.

Also, you will learn how to

  + use Here-string techinuqe

## CH44-1 -- rationales and performance
Here-string techinuqe stream value of an variable in same shell.

Using `>>>` (for Here-string) always has same behavior than passing the value into next command using pipe.

But their implementations are totally different.

For `>>>`, it behaves like the implementation of a `ToString` method of `StringBuilder` in `C#`, you can think of creating a virtual pipe in the shell then stream the value using the virtual pipe.

While pipe behaves like implementation of `Stream` in `C#`.

In each `|` pipe, it will create a subshell, then stream the returned value out from stderror and stdout to stdin of subshell that it will be used in next command (see CH22 for more details)

Thus, for string that is NOT too long or a file that is NOT to large, 

using `>>>` (Here-string) has much better performance than using `|` (pipe) 

It takes approxiatemately than simply accessing a variable it takes to stream the value of a variable out using `>>>` 

since it will stream out the value from std stdout and stderror to stdin in same shell.

While it takes a mutliple times of accessing a variable it takes to stream the value of a variable out using `|` (pipe)

since it is needed to create a subshell which takes much longer than simply accessing a variable.

But, for string that is too long or a very large file,

using `>>>` (Here-string) has much better performance than using `|` (pipe) 

If you use Here-string, it is needed to read the whole file content which holds lots of spaces in cached memory.

And consequently taking it much longer than using pipe due to lots of context switches.

While, if you use pipe (`|`), it is NOT needed to read the whole file content, it reads and handles file content block-by-block, which holds less spaces .

And thus, taking it shorter than using Here-string, even though creating a subshell spends lots time (but it takes much shorter than time using pipe)

## CH44-2 -- Here-string
syntax:

```
{command-or-pipe} <<< {expression-that-returns-value}
```

where

```
{expression-that-returns-value}: {expression} that will return value, it may be a value of a variable, or a user-define function call that returns value, or more.
```

## Examples
### Example 1
> [!NOTE]
> Check `bc.exe` and `jq.exe` are installed
>
> In Windows environment, `bc.exe` (for `bc` command) is NOT installed.
>
> Thus, you have to install the bc package (zipped in an archive file), unzip it with 7-zip manager
>
> then move it to where the Bash engine parser will be used.
>
> For me, I use Git Bash terminal, I have to move `bc.exe` to `C:\Program Files\Git\usr\bin`.
>
> `jq`.exe is NOT installed by default.
>
> you have to install it according to environment that is used on [GitHub release page](https://github.com/jqlang/jq/releases)
>
> Then you have to rename it as `jq.exe` and again move it to where the Bash engine parser will be used.
>
> For me, I use Git Bash terminal, I have to rename it to `jq.exe` then move it to `C:\Program Files\Git\usr\bin`.

utility modules:

`encryption-handler-module.bash`

```
### utility module
### 主要目的:
### 進行加密(encryption)和解密(decryption)運算

## utility function
## 主要目的:
## 用Base64演算法加密
## <params>
## <param name='$1'>
## <type>
## string
## </type>
## <description>
## plain text
## </description>
## </param>
## </params>
## <namerefs>
## <nameref name='$2'>
## <type>
## a string
## </type>
## <description>
## a string after encoded with base64 algorithm 
## </description>
## </nameref>
## </namerefs>
function to_base64(){
    local -n _ref=$2
    _ref=$(base64 <<< "$1")
}

## utility function
## 主要目的:
## 用Base64演算法解密
## <params>
## <param name='$1'>
## <type>
## string
## </type>
## <description>
## encrypted text
## </description>
## </param>
## </params>
## <namerefs>
## <nameref name='$2'>
## <type>
## a string
## </type>
## <description>
## a string after decoded with base64 algorithm 
## </description>
## </nameref>
## </namerefs>
function from_base64(){
    local -n _ref=$2
    _ref=$(base64 -d <<< "$1")
}

## utility function
## 主要目的:
## 產生SHA256密鑰
## <params>
## <param name='$1'>
## <type>
## string
## </type>
## <description>
## encrypted text
## </description>
## </param>
## </params>
## <namerefs>
## <nameref name='$2'>
## <type>
## a string
## </type>
## <description>
## a string after decoded with base64 algorithm 
## </description>
## </nameref>
## </namerefs>
function get_sha256(){
    local -n _ref=$2
    _ref=$(openssl dgst -sha256 <<< "$1")
}
```

`path-handler-module.bash`

```
### utility module
### 主要目的:
### 針對以字串方式呈現的檔案路徑或目錄進行處理

## utility function
## 主要目的:
## 將POSIX路徑轉換成Windows路徑 (類似 C# 的 Path.Replace('/', '\\'))
## <params>
## <param name='$1'>
## <type>
## string
## </type>
## <description>
## plain text
## </description>
## </param>
## </params>
## <namerefs>
## <nameref name='$2'>
## <type>
## a string
## </type>
## <description>
## a string after encoded with base64 algorithm 
## </description>
## </nameref>
## </namerefs>
function simply_to_windows_path(){
    local -n _ref=$2
    if [[ -z "$1" ]]; then
        _ref=""
        return 1
    fi
    local windows_path=$(sed -E 's/^\/([a-zA-Z])\//\1:\//; s/\//\\/g' <<< "$1")
    if [[ -z "$windows_path" ]]; then
        _ref=""
        return 1
    fi
    _ref="$windows_path"
    return 0
}

## utility function
## 詳見上一個utility function
function to_windows_path_with_full_range_supported(){
    local -n _ref=$2
    _ref=$(cygpath -w "$1")
}

```

`file-content-handler-module.bash`

```
### utility module
### 主要目的:
### 針對檔案內容進行處理

## utility function
## 主要目的:
## 針對多行字串中，取得行數
## <params>
## <param name='$1'>
## <type>
## string
## </type>
## <description>
## one-line string or multiple-line string
## </description>
## </param>
## </params>
## <namerefs>
## <nameref name='$2'>
## <type>
## a string
## </type>
## <description>
## The string after adding line no
## </description>
## </nameref>
## </namerefs>
function get_line_count(){
    local -n _ref=$2
    _ref=$(wc -l <<< "$1")
}

## utility function
## 主要目的:
## 針對多行字串中，在每行前面加上行號
## 格式:
## 1 xxx
## <params>
## <param name='$1'>
## <type>
## string
## </type>
## <description>
## one-line string or multiple-line string
## </description>
## </param>
## </params>
## <namerefs>
## <nameref name='$2'>
## <type>
## a string
## </type>
## <description>
## The string after adding line no
## </description>
## </nameref>
## </namerefs>
function add_line_no(){
    local -n _ref=$2
    # 注意：$(...) 會移除字串末尾所有的換行符號
    _ref=$(cat -n <<< "$1")
}

## utility function
## 主要目的:
## 針對多行字串中，在每行前面加上行號
## 格式:
## 1. xxx
## <params>
## <param name='$1'>
## <type>
## string
## </type>
## <description>
## one-line string or multiple-line string
## </description>
## </param>
## </params>
## <namerefs>
## <nameref name='$2'>
## <type>
## a string
## </type>
## <description>
## The string after adding line no
## </description>
## </nameref>
## </namerefs>
function add_dotted_line_no(){
    local -n _ref=$2
    # 注意：$(...) 會移除字串末尾所有的換行符號
    _ref=$(nl -s ". " -w 1 <<< "$1")
}

## utility function
## 主要目的:
## 針對多行字串中，在每行前面加上行號
## 格式:
## {$2} 1. {$2} xxx
## <params>
## <param name='$1'>
## <type>
## string
## </type>
## <description>
## one-line string or multiple-line string
## </description>
## </param>
## <param name='$2'>
## <type>
## string
## </type>
## <description>
## character on left-hand side that wraps the line no 
## </description>
## </param>
## <param name='$3'>
## <type>
## string
## </type>
## <description>
## character on right-hand side that wraps the line no 
## </description>
## </param>
## <param name='$4'>
## <type>
## integer
## </type>
## <description>
## determine to use zero-based index or one-based index
## 0 (indicating boolean true) => zero-based index
## 1 (indicating boolean false) => one-based index
## </description>
## </param>
## </params>
## <namerefs>
## <nameref name='$5'>
## <type>
## a string
## </type>
## <description>
## The string after adding line no
## </description>
## </nameref>
## </namerefs>
function add_bracket_wrapped_line_no(){
    local input_str="$1"
    local wrap_left_char="$2"
    local wrap_right_char="$3"
    local is_zero_based=$4
    local -n _ref=$5

    # 驗證 $3 (Boolean 邏輯)
    local offset=0
    if [[ "$is_zero_based" -eq 0 ]]; then
        offset=-1
    elif [[ "$is_zero_based" -eq 1 ]]; then
        offset=0
    else
        return 1 # 參數錯誤
    fi

    # 使用 -v 將變數傳入 awk 內部，避免轉義引號的混亂
    # wrap_left_char,wrap_right_char: 包裹字元, off: 偏移量
    _ref=$(awk -v wrap_left_char="$wrap_left_char" -v wrap_right_char="$wrap_right_char" -v off="$offset" '
        {
            current_idx = NR + off
            printf "%s%d%s %s\n", wrap_left_char, current_idx, wrap_right_char, $0
        }
    ' <<< "$input_str")
}

## utility function
## 主要目的:
## 針對多行字串中，在每行前面加上行號
## 格式:
## 1. xxx
## <params>
## <param name='$1'>
## <type>
## string
## </type>
## <description>
## one-line string or multiple-line string
## </description>
## </param>
## <param name='$2'>
## <type>
## string
## </type>
## <description>
## character on left-hand side that wraps the line no 
## </description>
## </param>
## <param name='$3'>
## <type>
## string
## </type>
## <description>
## character on right-hand side that wraps the line no 
## </description>
## </param>
## <param name='$4'>
## <type>
## integer
## </type>
## <description>
## determine to use zero-based index or one-based index
## 0 (indicating boolean true) => zero-based index
## 1 (indicating boolean false) => one-based index
## </description>
## </param>
## </params>
## <namerefs>
## <nameref name='$5'>
## <type>
## a string
## </type>
## <description>
## The string after adding line no
## </description>
## </nameref>
## </namerefs>
function add_hex_no() {
    local input_str="$1"
    local wrap_left_char="$2"
    local wrap_right_char="$3"
    local is_zero_based="$4"
    local -n _ref=$5

    # 驗證 $3 是否為 0 或 1
    if [[ "$is_zero_based" -ne 0 && "$is_zero_based" -ne 1 ]]; then
        return 1
    fi

    # 決定減法偏移量 (awk NR 預設從 1 開始)
    local offset
    if [[ "$is_zero_based" -eq 0 ]]; then
        offset="-1"
    else
        offset="+0"
    fi

    # 使用 -v 將變數傳入 awk 內部，避免轉義引號的混亂
    # wrap_left_char,wrap_right_char: 包裹字元, off: 偏移量
    _ref=$(awk -v wrap_left_char="$wrap_left_char" -v wrap_right_char="$wrap_right_char" -v off="$offset" '{
        printf "%s0x%02x%s %s\n", wrap_left_char, NR+off, wrap_right_char, $0
    }' <<< "$input_str")
}
```

`float-number-calculator-module.bash`

```
### utility module
### 主要目的:
### 進行數學計算並回傳小數(floating number) 
### 透過 BC (arbitrary precision calculator language)
### 
### 例如:
### 2/3 => 0.666

## utility function
## 主要目的:
## 進行除法運算並四捨五入至小數點第n位
## <params>
## <param name='$1'>
## <type>
## integer
## </type>
## </param>
## <param name='$2'>
## <type>
## integer
## </type>
## </param>
## <param name='$3'>
## <type>
## integer
## </type>
## </param>
## </params>
## <namerefs>
## <nameref name='$4'>
## <type>
## a string of float number
## </type>
## </nameref>
## </namerefs>
function float_division(){
    local -n _ref=$4
    _ref=$(bc <<< "scale=$3; $1 / $2")
}
```

`alphabet-handler-module.bash`

```
### utility module
### 主要目的:
### 定義一些處理英文字母的utility function

## utility function
## 主要目的:
## 轉成大寫
function to_upper(){
    local -n _ref=$2
    _ref=$(tr '[:lower:]' '[:upper:]' <<< "$1")
}

## utility function
## 主要目的:
## 轉成小寫
function to_lower(){
    local -n _ref=$2
    _ref=$(tr '[:upper:]' '[:lower:]' <<< "$1")
}

## utility function
## 主要目的:
## 從一個json格式的字串存取特定的property
function get_value_by_property_name(){
    local -n _ref=$3
    local property_name=".$2"
    _ref=$(jq -r "$property_name" <<< "$1")
}
```

main script:

`Here-string-example-1.bash`

```
# Get the directory where the current script is located 
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

source "$SCRIPT_DIR/../../utility modules/Here-string/alphabet-handler-module.bash"
source "$SCRIPT_DIR/../../utility modules/Here-string/path-handler-module.bash"
source "$SCRIPT_DIR/../../utility modules/Here-string/encryption-handler-module.bash"
source "$SCRIPT_DIR/../../utility modules/Here-string/float-number-calculator-module.bash"
source "$SCRIPT_DIR/../../utility modules/Here-string/file-content-handler-module.bash"

INPUTS_ROOT_DIRECTORY="$SCRIPT_DIR/../../inputs"
MULTIPLE_LINE_SCRIPT_BASE_DIRECTORY="$INPUTS_ROOT_DIRECTORY/scripts"
MULTIPLE_LINE_SCRIPT_EXAMPLE_1="$MULTIPLE_LINE_SCRIPT_BASE_DIRECTORY/multiple-line-example-1.txt"

MULTIPLE_LINE_SCRIPT_EXAMPLE_1_FILE_CONTENT=$(<"$MULTIPLE_LINE_SCRIPT_EXAMPLE_1")

if [[ -z "$MULTIPLE_LINE_SCRIPT_EXAMPLE_1_FILE_CONTENT" ]]; then
    "\`$MULTIPLE_LINE_SCRIPT_EXAMPLE_1\` file is empty or can't load it successfully.">2
    return 1
fi

function multiple_line_demo_function(){
    local input_file_content="$MULTIPLE_LINE_SCRIPT_EXAMPLE_1_FILE_CONTENT"
    local output_file_content=""
    
    echo "---------------- Part 1 -- Handles multiple-line string ----------------"

    echo "1.0)----------------"
    echo "Input file content:"
    cat "$MULTIPLE_LINE_SCRIPT_EXAMPLE_1"
    echo ""

    echo "1.1)----------------"
    add_line_no "$input_file_content" output_file_content
    echo "After adding line no, output file content:"
    echo "$output_file_content"

    echo "1.2)----------------"
    add_dotted_line_no "$input_file_content" output_file_content
    echo "After adding line no with dot, output file content:"
    echo "$output_file_content"

    echo "1.3)----------------"
    add_bracket_wrapped_line_no "$input_file_content" "[" "]" 0 output_file_content
    echo "After adding line no with wrapped squared brackets (using zero-based index), output file content:"
    echo "$output_file_content"

    echo "1.4)----------------"
    add_hex_no "$input_file_content" "[" "]" 1 output_file_content
    echo "After adding line number in hex with wrapped squared brackets (using one-based index), output file content:"
    echo "$output_file_content"
}

function encryption_demo_function(){
    local token="1234567890abcdefghijklmnopqrstuwxyz"
    local encoded_str=""
    local decoded_str=""
    local sha256=""
    local yes_no_answer=""

    echo "---------------- Part 2 -- Encrypt and decrypt the token ----------------"

    echo "2.0)----------------"
    echo "Token:"
    echo "$token"

    echo "2.1)----------------"
    to_base64 "$token" encoded_str
    echo "After encrypting the token with base64 algorithm, the encryption token will be:"
    echo "$encoded_str"

    echo "2.2)----------------"
    from_base64 "$encoded_str" decoded_str
    echo "After decrypting the token with base64 algorithm, the decryption token will be:"
    echo "$decoded_str"

    echo "2.3)----------------"
    if [[ "$token" == "$decoded_str" ]]; then
        yes_no_answer="yes"
    else
        yes_no_answer="no"
    fi
    echo "Verification!!! Is the decrypted token \`$decoded_str\` same as the original token \`$token\`:\`$yes_no_answer\`" 

    echo "2.4)----------------"
    get_sha256 "$token" sha256
    echo "The sha256 of \`$token\` is \`$sha256\`"
}

function alphabet_demo_function(){
    local text="I love to play basketball. Also, I am good at it."
    local upper_cased_text=""
    local lower_cased_text=""

    echo "---------------- Part 3 -- Handles the alphabet letter ----------------"

    echo "3.0)----------------"
    echo "text:"
    echo "$text"

    echo "3.1)----------------"
    to_upper "$text" upper_cased_text
    echo "After performing upper-case text, the text is:"
    echo "$upper_cased_text"

    echo "3.2)----------------"
    to_lower "$text" lower_cased_text
    echo "After performing lower-case text, the text is:"
    echo "$lower_cased_text"
}

function json_demo_function(){
    local json_str='{"interesting":"basketball","excellent":"basketball"}'
    local property_name="interesting"
    local property_value=""
    
    echo "---------------- Part 4 -- Serialize and deserialize and json string operations ----------------"
    echo "4.1)----------------"
    get_value_by_property_name "$json_str" "$property_name" property_value
    if [[ -n "$property_value" ]]; then
        echo "The json \`$json_str\` has the property named \`$property_name\` with value \`$property_value\`"
    else
        echo "The json \`$json_str\` has NO property named \`$property_name\`"
    fi
}

function bc_demo_function(){
    declare -i precision=0
    declare -i numerator=0
    declare -i denominator=0
    local returned_value=""

    local property_name="interesting"
    local property_value=""
    
    echo "---------------- Part 5 -- Using BC to perform arithmetic operation that may get the floating number ----------------"
    echo "5.1)----------------"
    precision=2
    numerator=3
    denominator=5
    float_division $precision $numerator $denominator returned_value
    echo "\`$numerator / $denominator\` with precision \`precision\` equals to \`$returned_value\`"

    echo "5.2)----------------"
    precision=100
    numerator=3
    denominator=6
    float_division $precision $numerator $denominator returned_value
    echo "\`$numerator / $denominator\` with precision \`precision\` equals to \`$returned_value\`"
}

function path_demo_function(){
    local path_with_windows_standard=""
    local path_with_posix_standard="$SCRIPT_DIR/../../utility modules/Here-string/file-content-handler-module.bash" 
    echo "---------------- Part 6 -- Handles file path ----------------"
    echo "6.1)----------------" 
    simply_to_windows_path "$path_with_posix_standard" path_with_windows_standard
    echo "The \`$path_with_posix_standard\` (used in Unix/Linux environment) is converted to \`$path_with_windows_standard\` (used on Windows environment)"
    
    echo "6.2)----------------" 
    to_windows_path_with_full_range_supported "$path_with_posix_standard" path_with_windows_standard
    echo "The \`$path_with_posix_standard\` (used in Unix/Linux environment) is converted to \`$path_with_windows_standard\` (used on Windows environment) using \`cygpath\` command which has full-range support."
}

main(){
    multiple_line_demo_function
    encryption_demo_function
    alphabet_demo_function
    json_demo_function
    bc_demo_function
    path_demo_function
}

main
```

```

input text file:

`multiple-line-example-1.txt`

```
This is a multiple line
Hello C
Hello C++
Hello C#
Hello Bash
Hello Java
Hello GoLang
Hello Kotlin
```

Assume that this main script is located at `D:\workspace\Bash\Bash tutorial\examples\Here-string\Here-string-example-1.bash` and the input text file is located at `D:\workspace\Bash\Bash tutorial\inputs\scripts\multiple-line-example-1.txt`

then executing this main script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\Here-string\Here-string-example-1.bash"
---------------- Part 1 -- Handles multiple-line string ----------------
1.0)----------------
Input file content:
This is a multiple line
Hello C
Hello C++
Hello C#
Hello Bash
Hello Java
Hello GoLang
Hello Kotlin
1.1)----------------
After adding line no, output file content:
     1  This is a multiple line
     2  Hello C
     3  Hello C++
     4  Hello C#
     5  Hello Bash
     6  Hello Java
     7  Hello GoLang
     8  Hello Kotlin
1.2)----------------
After adding line no with dot, output file content:
1. This is a multiple line
2. Hello C
3. Hello C++
4. Hello C#
5. Hello Bash
6. Hello Java
7. Hello GoLang
8. Hello Kotlin
1.3)----------------
After adding line no with wrapped squared brackets (using zero-based index), output file content:
[0] This is a multiple line
[1] Hello C
[2] Hello C++
[3] Hello C#
[4] Hello Bash
[5] Hello Java
[6] Hello GoLang
[7] Hello Kotlin
1.4)----------------
After adding line number in hex with wrapped squared brackets (using one-based index), output file content:
[0x01] This is a multiple line
[0x02] Hello C
[0x03] Hello C++
[0x04] Hello C#
[0x05] Hello Bash
[0x06] Hello Java
[0x07] Hello GoLang
[0x08] Hello Kotlin
---------------- Part 2 -- Encrypt and decrypt the token ----------------
2.0)----------------
Token:
1234567890abcdefghijklmnopqrstuwxyz
2.1)----------------
After encrypting the token with base64 algorithm, the encryption token will be:
MTIzNDU2Nzg5MGFiY2RlZmdoaWprbG1ub3BxcnN0dXd4eXoK
2.2)----------------
After decrypting the token with base64 algorithm, the decryption token will be:
1234567890abcdefghijklmnopqrstuwxyz
2.3)----------------
Verification!!! Is the decrypted token `1234567890abcdefghijklmnopqrstuwxyz` same as the original token `1234567890abcdefghijklmnopqrstuwxyz`:`yes`
2.4)----------------
The sha256 of `1234567890abcdefghijklmnopqrstuwxyz` is `SHA2-256(stdin)= 6506fbee164c0111fd54cc7bca5f8aadb3d99bb4f7c63ef7be3caf4800d8d061`
---------------- Part 3 -- Handles the alphabet letter ----------------
3.0)----------------
text:
I love to play basketball. Also, I am good at it.
3.1)----------------
After performing upper-case text, the text is:
I LOVE TO PLAY BASKETBALL. ALSO, I AM GOOD AT IT.
3.2)----------------
After performing lower-case text, the text is:
i love to play basketball. also, i am good at it.
---------------- Part 4 -- Serialize and deserialize and json string operations ----------------
4.1)----------------
The json `{"interesting":"basketball","excellent":"basketball"}` has the property named `interesting` with value `basketball`
---------------- Part 5 -- Using BC to perform arithmetic operation that may get the floating number ----------------
5.1)----------------
`3 / 5` with precision `precision` equals to `.66666`
5.2)----------------
`3 / 6` with precision `precision` equals to `33.333333`
---------------- Part 6 -- Handles file path ----------------
6.1)----------------
The `/d/workspace/Bash/Bash tutorial/examples/Here-string/../../utility modules/Here-string/file-content-handler-module.bash` (used in Unix/Linux environment) is converted to `d:\workspace\Bash\Bash tutorial\examples\Here-string\..\..\utility modules\Here-string\file-content-handler-module.bash` (used on Windows environment)
6.2)----------------
The `/d/workspace/Bash/Bash tutorial/examples/Here-string/../../utility modules/Here-string/file-content-handler-module.bash` (used in Unix/Linux environment) is converted to `D:\workspace\Bash\Bash tutorial\utility modules\Here-string\file-content-handler-module.bash` (used on Windows environment) using `cygpath` command which has full-range support.

```
