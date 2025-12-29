# CH22 -- pipeline
## objectives
You will learn how to

  + use pipeline

## CH22-1 -- syntax and description
syntax:

Represented with RE.

```
(time (\-p)?)? (!)? {connection}
```

where

```
{connection}:= {commands}
{commands}:= {command} ({pipeline-operators} {command})*
{pipeline-operators}:= {standard-pipeline} | {pipeline-with-return-value}
{standard-pipeline}:= "|"
{pipeline-with-return-value}:= "|&"
```

> [!NOTE]
> `|&` is shorthand of `2>&1 |` which means stream out from stderr, stdout into next command among the consecutive commands then pass it to next command

- pipeline operators:

+ `|`: After finishing executing this command, executes next command without arguments
+ `|&`: After finishing executing this command, executes next command with arguments passed from this command with value -- returned value (exit code) of this command

- return value

Case 1: If `pipefail` functionality is NOT enabled (it is the default),

  The connection will return the exit code of last executed command regardless the exit code that is returned by other commands 

Case 2: Otherwise

  The connection will return the exit code of last executed command with non-zero exit code (indicating failure)
  
`!` negates the value that will be returned of the connection.

## CH22-2 -- rationales
### independency
When starting to execute one among the consecutive commands, it is executed in different subshell.

So, they are independent. Thus, 

+ when value of one local, global variable without exporting (using `export` built-in command) is changed, typically, it will NOT affect in other and main shell.

> [!IMPORTANT]
> Exceptions of use case:
>
> + `lastpipe` functionality is disabled and job control is closed.
>
> In above example, last command will be executed in main shell. So that you can set the value of variable in last command and make it affect. 
## CH22-3 -- precedence
The following has the highest to lowest precedence, respectively

1. connection using pipeline operators
2. redirection
