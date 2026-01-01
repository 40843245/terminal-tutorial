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

If the dependency does NOT exist, 

then `Makefile` tool will auto-generate this file through its related file.

The syntax:

```
{Target}: {Dependencies}
    {Commands}
```

or

```
{Target}:
    {Commands}
```

where

`{Target}` is the tag name of the ruleset. It is treated as the argument of `make` command.

```

```

