# CH37 -- configuration file in Bash
## objectives
You will learn how to configure settings in configuration file.

## CH37-1 -- configure settings in configuration file
### Git Bash
In Git Bash terminal,

In newer version of Git Bash, the startup file is `.bashrc` located at home directory.

In older version of Git Bash, the startup file is `.bash_profile` located at home directory.

So, for compatibility, it is highly recommend to

type these following code snippet at top in `~/.bash_profile` file

`~/.bash_profile`

```
if [ -f ~/.bashrc ]; then
  . ~/.bashrc
fi
```

above code snippet means that

loads `~/.bashrc` if it exists.

#### Examples
##### Example 1
###### code
Here's an example via Bash command.

Open Git Bash terminal

Type this command to change the current working directory to `HOME` system environment variable.

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ cd ~
```

create or overwriting file `.bashrc` under `HOME` system environment variable
```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ touch .bashrc
```

userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ notepad .bashrc


userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ notepad ~/.bash_profile

```
###### explanation
```
```
