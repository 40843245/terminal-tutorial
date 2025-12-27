# CH36 -- Controlling the Prompt
## objectives
You will learn how to control the prompt via setting environment variable.

## CH36-1 -- introduction to prompt
prompt is a text shown in terminal that prompts you to enter the command.

<img width="1910" height="564" alt="image" src="https://github.com/user-attachments/assets/47e24577-0ea4-45a0-8b1d-5e640f364f12" />

In above figure,

`$` is the prompt text of this terminal.

## CH36-2 -- setting prompt text
> [!IMPORTANT]
> it will ONLY take effect when executing the commands that setting environment variable on terminal.
>
> it will NOT take effect when setting environment variable in script.

> [!IMPORTANT]
> it will ONLY take effect in this session.

setting `PS1` environment variable will set the prompt text.

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ PS1="\u@\W \$ "
userJay30@~ $ PS1="[\t] \u@\h:\w\$ "
[11:05:07] userJay30@ASUS-B1400CBNGW:~$ PS1="\u 在 \w 說：\n> "
userJay30 在 ~ 說：
>

```
