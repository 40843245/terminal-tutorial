# CH30 -- built-in commands used for executing command
## objectives
You will know 

  + built-in commands used for executing command

## CH30-1 -- built-in commands used for executing command
### `:`
`:`: just expanding arguments (see CH11) and then performing redirection (see CH24)

syntax:

```
: {arguments}
```

### `.`

syntax:

```
. (-p {path})? {file-name} {arguments}?
```

`.` command reads and then execute {file-name} with `{arguments}` in the current shell context.

> [!NOTE]
> If `-p` is supplied, `.` treats `{path}` as a colon-separated list of directories in which to find {file-name}
>
> Otherwise, `.` uses the directories in `$PATH` to find {file-name}. 

> [!NOTE]
> `{file-name}` does not need to be executable
