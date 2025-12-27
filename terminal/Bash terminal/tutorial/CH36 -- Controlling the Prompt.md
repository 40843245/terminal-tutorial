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

### Examples
#### Example 1
In Bash Terminal

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ PS1="\u@\W \$ "
userJay30@~ $ PS1="[\t] \u@\h:\w\$ "
[11:05:07] userJay30@ASUS-B1400CBNGW:~$ PS1="\u 在 \w 說：\n> "
userJay30 在 ~ 說：
> PS1="\h"
ASUS-B1400CBNGW
ASUS-B1400CBNGW PS1="\u@\h \$ "
userJay30@ASUS-B1400CBNGW $  PS1="\u@\H \$ "
userJay30@ASUS-B1400CBNGW $  PS1="\u@\H \$ There are \j of jobs currently managed by the shell.>"
userJay30@ASUS-B1400CBNGW $ There are 0 of jobs currently managed by the shell.> PS1="\u@\H \$ The basename of shell's terminal name:\i. There are \j of jobs currently managed by the shell.>"
userJay30@ASUS-B1400CBNGW $ The basename of shell's terminal name:\i. There are 0 of jobs currently managed by the shell.> PS1="\u@\H \$ The basename of shell's terminal name:\l. There are \j of jobs currently managed by the shell.>"
userJay30@ASUS-B1400CBNGW $ The basename of shell's terminal name:pty0. There are 0 of jobs currently managed by the shell.>PS1="[\@] \u@\h:\w\$ "
[11:22 AM] userJay30@ASUS-B1400CBNGW:~$ PS1="[\A] \u@\h:\w\$ "
[11:22] userJay30@ASUS-B1400CBNGW:~$ PS1="[\u] \u@\h:\w\$ "
[userJay30] userJay30@ASUS-B1400CBNGW:~$ PS1="[\t] \u@\h:\w\$ "
[11:23:19] userJay30@ASUS-B1400CBNGW:~$ PS1="[\T] \u@\h:\w\$ "
[11:23:23] userJay30@ASUS-B1400CBNGW:~$ PS1="[\V] \u@\h:\w\$ "
[5.2.37] userJay30@ASUS-B1400CBNGW:~$ PS1="[\v] \u@\h:\w\$ "
[5.2] userJay30@ASUS-B1400CBNGW:~$ PS1="[\T] \u@\h:\w The history number of this command:`\!` \$ "
bash: !: command not found
[11:25:17] userJay30@ASUS-B1400CBNGW:~ The history number of this command: $ PS1="[\T] \u@\h:\w The history number of this command:\! \$ "
[11:25:26] userJay30@ASUS-B1400CBNGW:~ The history number of this command:519 $ PS1="[\T] \u@\h:\w The command number of this command:\# \$ "
[11:25:46] userJay30@ASUS-B1400CBNGW:~ The command number of this command:20 $

```
