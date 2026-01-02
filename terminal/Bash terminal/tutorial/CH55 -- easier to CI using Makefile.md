# CH55 -- easier to CI using Makefile
## objectives
You will learn how to 

    + do something easier by writting a Makefile.

## CH55-1 -- introduction of Makefile
Makefile is a file consists of rulesets whose ruleset wraps one or more commands that will be executed using `make` command with arguments.

`make` command makes it easier to auto-generate the file to more easily compile ONLY for the changed files (like Git will commit for changed but uncommitted file)

Makefile is based on POSIX standard.

## CH55-2 -- rationales
1. `Make` tool creates a subshell when executes one command, and thus it does NOT take effect in different commands even in the same target, to solve this issue, combines conmmands to a clause (see CH55-6)
2. `Make` tool will terminate to execute the target when an error is thrown unless it is ignored (see CH55-9) but ONLY applied for a non-fatal error.
3. `Make` tool will print a command itself immediately after the command is successfully executed, by default (see CH55-8), or print the error message when executing one command with failure, and consequently `Make` tool terminate to execute the target, by default.

## CH55-3 -- rulesets
### define a ruleset
In Makefile, a file consists of rulesets.

Each ruleset wraps one or more commands with given dependencies (see 1th form), or without anything (see 2th form)

Iff the dependency does NOT exist or it exist but one of its related files is changed., 

then `Makefile` tool will auto-generate this file through its related files.

The syntax:

1th form:

```
{Target}: {Dependencies}
    {Commands}
```

or

2th form

```
{Target}:
    {Commands}
```

or

3th form

```
{Target}:{Another-Target}
```

where

`{Target}` is the tag name of the ruleset. It is treated as an argument of `make` command.

`{Dependencies}` are depencies consists one or more file names seperated with whitespace ` `

```
{Dependencies}:={Dependency}
{Dependency}:= {FileName}
```

and 

`{Commands}` consists one or more commands with Ubuntu kernel (such as Unix/Linux environment).

Additionally, you can specify that executing a target will execute another target in the 3th form. 

> [!IMPORTANTT]
> It is needed to indent four spaces with one tab `\t` rather than four whitespace ` `.

> [!NOTE]
> If you don't specify the argument with `make` command (`make`),
> 
> then it will execute the first target by default.

> [!NOTE]
> The `make` uses the create time and modified time to
>
> check whether a file is changed or not,
>
> and thus to check whether needed to auto-generate or auto-update the dependencies that uses this file.

## CH55-4 -- variable
### define a variable
To define a variable,

like it in POSIX

just directly simply assign a value to a variable name

syntax:

```
# assignment a value (can be either a constant or a value after evaluation or a predifine value (whose concepts is similar to macro in `C#`)
{VariableName} = {Value}
```

where

`{VariableName}` is a valid variable name that consists of a valid identifier.

```
{VariableName}:= {Identifier}
```

> [!IMPORTANT]
> In assignment, it is needed to leave exactly one whitespace ` ` before and after assignment operator `=`.

### naming rule of valid identifier
Like it in POSIX,

An Identifier must 

    + consist if digits, alphabets, or underscores.
    + starts with an alphabet or a underscore

```
{Identifier}:= ({Alphabet}|{Underscore}) ({Alphabet}|{Digit}|{Underscore})*
```

where

```
# an alphabet is a character that starts from `a` to `z` or from `A` to `Z`
{Alphabet}:= ([a-z]|[A-Z])
# an underscore is a character `_`.
{Underscore}:= "_"
# a digit is a characters that stars from `0` to `9`.
{Digit}:= [0-9]
```

### accessing a variable
To access a variable, like in POSIX,

use a variable expansion -- `$(` enclosed with `)`.

syntax:

```
$({VariableName})
```

> [!IMPORTANT]
> In variable expansion, it is NOT to leave any whitespaces between `(` and a variable name, and so for between a variable name and `)`.
>
> It is the correct way to access a variable like following examples
> 
>> [!DO]
>>
>> ```
>> $(DOTNET)
>> ```
>
> While it is not correct, don't do so like following examples
> 
>> [!DON'T]
>>
>> ```
>> $( DONTET)
>> ```
>>
>> ```
>> $(DOTNET )
>> ```
>>
>> ```
>> $( DOTNET )
>> ```

## CH55-5 -- phony target
Marking a target as a phony target can ensure that 

    + when typing the tagert as an argument of `make` (such as `make clean`), always executes the target (even if there exists a file whose name equals the target name)

### marking a phony target
To marking a target as a phony target, use `.PHONY`

syntax:

```
.PHONY: {Target}
```

> [!IMPPORTANT]
> If a terget is NOT specified with `.PHONY` annotations, then the target will NOT executed iff there exist a file with same name.

> [!NOTE]
> All annotations (such as `.PHONY`) MUST be placed before the specific target `{Target}`.

## CH5-6 -- clause
One command ends at the end of the line.

A clause consists of many commands with seperator `;` but it is executed in same one subshell.

The rationale of executing a clause is NOT totally equivalent to executing many commands (It will create one subshell to execute one command), 

it can solve the issue about environment variable is NOT changed (see 1th point of CH55-2) 

To combine many commands to one clause, 

append `; ` as separator at the end of each commmand (NOT leave any whitespace before `;` and leave at least one whitespace ` ` after `;`)

then place many commands in same one line as a clause.

A clause ONLY can hold one line unless `\` (multi-line symbol) is appended at the end of line (and leaving at least one whitespaces before `\`).

## CH55-7 -- logical operator

| logical operator | meaning | description |
| :-- | :-- | :-- |
| `&&` | and | returns true iff all conditions BOTH return true |
| `||` | or | returns true if any conditions are true |

#### short-circuit features
Once one clause can be determined to return true or false, then the rest conditions will be NOT executed.

So you can combine these commands to one clause with `&&` separator,

so that when executing one command with failure (returns non-zero exit code), the next command will NOT be executed.

## CH55-8 -- control printing behavior
Since `Make` tool will print a command itself immediately after the command is successfully executed, by default,

to NOT print the command itself, it is needed to add `@` before the command (NOT leaving any whitespaces)

> [!NOTE]
> Although it will NOT print the command if `@` is added before the command, it will still execute the command.

## CH55-9 -- ignore a non-fatal error
To make it continue to execute the target when there is a non-fatal error, please ignore it.

Adding `-` before the command (NOT leaving any whitespace), when a non-fatal error is thrown during executing this command, it will ignore it (but it will print `(ignore)`, after printing the error message, to prompt the user)

## reference
+ [Makefile 基礎教學](https://liaoxuefeng.com/books/makefile/makefile-basic/)

