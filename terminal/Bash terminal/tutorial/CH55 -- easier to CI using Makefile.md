# CH55 -- easier to CI using Makefile
## objectives
You will learn how to 

    + do something easier by writting a Makefile.

## CH55-1 -- introduction of Makefile
Makefile is a file consists of rulesets whose ruleset can be executed by `make` command (with arguments)

`make` command makes it easier to auto-generate the file to more easily compile ONLY for the changed files (like Git will commit for changed but uncommitted file)

## CH55-5 -- syntax
In Makefile,

A file consists of rulesets.

Each ruleset consists of

A key-value pair and/or its dependencies.

The key is the tag name of the ruleset. It is treated as the argument of `make` command.

The value is the commands that are executed when executing the ruleset.

The dependencies in the ruleset can be used in the ruleset.

### define a ruleset
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

### variable
#### define a variable
To define a variable,

like it in Ubuntu kernel

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

#### naming rule of valid identifier
Like it in Ubuntu kernel,

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

#### accessing a variable
To access a variable, like in Ubuntu kernel,

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
