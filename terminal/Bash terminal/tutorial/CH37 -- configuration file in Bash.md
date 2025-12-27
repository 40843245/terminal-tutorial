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

#### Samples
Here's an sample via Bash command.

Open Git Bash terminal

Type this command to change the current working directory to `HOME` system environment variable.

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ cd ~
```

Then create or overwriting file `.bashrc` under `HOME` system environment variable

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ touch .bashrc
```

Then open NotePad it 

> [!IMPORTANT]
> You can use `vim .bashrc` in Unix/Linux environment to type plain text in edit mode)

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ notepad .bashrc
```

After finishing writing the Bash script, close it.

Then create or overwriting `.bash_profile` under `HOME` system environment variable.

Then open NotePad it.

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ notepad ~/.bash_profile
```

Then type these at top.

```
if [ -f ~/.bashrc ]; then
  . ~/.bashrc
fi
```

#### Examples
##### Example 1
Type these commands

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ cd ~
```

Then create or overwriting `.bashrc`.

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ touch .bashrc
```

Then open NotePad it.

```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ notepad .bashrc
```

Type these in `.bashrc` under `HOME` system environment variable.

```
PS1="\u 在 \w 說：\n> "
```

Similarly type these in `.bash_profile` under `HOME` system environment variable.

```
if [ -f ~/.bashrc ]; then
  . ~/.bashrc
fi
```

Then open Git Bash Terminal,

you will see the prompt will look like this
```
userJay30@ASUS-B1400CBNGW MINGW64 ~ (master)
$ notepad ~/.bash_profile


<img width="959" height="376" alt="image" src="https://github.com/user-attachments/assets/112db5bb-fdd2-4c0b-94fd-7c4ab40f78c8" />
