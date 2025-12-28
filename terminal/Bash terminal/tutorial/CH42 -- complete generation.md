# CH42 -- complete generation
## objectives
You will learn how to know

  + when you type tab on keyboard, how to autofill in word

## CH42-1 -- complete generation
### Examples
#### Example 1
`D:\workspace\Bash\Bash tutorial\example scripts\completion generation\dotnet-wordlist-filter.bash`

```
# 確保補全函式有被定義
my-dotnet-tool-helper() { 
    echo "你執行了指令，參數為: \`$@\`"; 
}

_my_dotnet_tool_completion() {
    local cur="$2"
    local prev="$3"
    local opts="run build config"

    # 如果是指令後的第一個字，補全子指令 (build/run/config)
    if [[ "$prev" == "my-dotnet-tool-helper" ]]; then
        COMPREPLY=( $(compgen -W "${opts}" -- "$cur") )
        return 0
    fi

    case "${prev}" in
        "build")
            # 僅使用 compgen 處理 glob 模式
            COMPREPLY=( $(compgen -G "${cur}*.csproj") )
            # 保持空陣列，不要放入中文字串 
            return 0
            ;;
        "config")
            # 僅使用 compgen 處理 glob 模式
            COMPREPLY=( $(compgen -G "${cur}*.json") )
            # 保持空陣列，不要放入中文字串 
            return 0
            ;;
    esac
}

# 註冊補全函式到你的指令
complete -F _my_dotnet_tool_completion my-dotnet-tool-helper
```

Interactions:

To load the file, type this command

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ source "D:\workspace\Bash\Bash tutorial\example scripts\completion generation\dotnet-wordlist-filter.bash"
```

When you type `my-dotnet-tool-helper` then click tab on keyboard twice, it will show the available wordlist

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ my-dotnet-tool-helper
build   config  run
```

When you type `my-dotnet-tool-helper `(note there is a whitespace) then click tab on keyboard twice, it will show the available wordlist

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ my-dotnet-tool-helper
build   config  run
```

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ cd "D:\workspace\Bash\Bash tutorial"
```

```
userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ my-dotnet-tool-helper
你執行了指令，參數為: ``
```

When you type `my-dotnet-tool-helper bu`(note there is a whitespace) then click tab on keyboard once, it will autofill as `build`

```
userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ my-dotnet-tool-helper build
你執行了指令，參數為: `build`
```

When you type `my-dotnet-tool-helper r`(note there is a whitespace) then click tab on keyboard once, it will autofill as `run`

```
userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ my-dotnet-tool-helper run
你執行了指令，參數為: `run`
```

When you type `my-dotnet-tool-helper ru`(note there is a whitespace) then click tab on keyboard once, it will autofill as `run`

```
userJay30@ASUS-B1400CBNGW MINGW64 /d/workspace/Bash/Bash tutorial
$ my-dotnet-tool-helper run
你執行了指令，參數為: `run`
```
