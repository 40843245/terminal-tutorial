# CH34 -- parsing alias names
## preface
This is a technical note: Mastery of Bash Alias and `eval`

This note summarizes the behavioral mechanics of `alias` and the `eval` command, derived from a series of 28 comprehensive experiments. 

It is designed for developers coming from a structured background (like C#) to understand the `dynamic parsing` nature of Shell.

## objectives
You will first know the core mechanism of parsing an alias name 
  
  + `alias` performs literal substitution

Then you will know some important principles about parsing an aliased name and the behavior of `alias` and `eval`

Additionally, I will highlight the notice that you need to pay lots of attention to

Lastly, I will discuss these issues. 

  + what's the pros and cons of using aliased name compared to using functions
  + why use functions for logic and parameters rather than aliased names
  + which is better to use aliased name rather than functions.
    
## CH34-1 -- notice
> [!IMPORTANT]
> In Bash, aliases are **NOT** variables or functions.
> 
> They are **literal text substitutions** with a mapping table about parse whose entry is inserted into during the parsing phase, before the command is executed.

> [!WARNING]
> To make Bash engine consider it is a aliased name that needs to be expanded, you have to enable the functionality `expand_aliases`
>
> About how to enable a functionality, see CH3.

For example,

To use aliased names `say_hello` and `say_dirty_word`, you have to tell Bash engine that they are aliased names, you have to expand it. 
```
alias say_hello='echo Hello from Alias!'
alias say_dirty_word='echo dirty word from Alias!' 

main(){
    shopt -s expand_aliases

    # use alias
    say_hello
    echo ""
    say_dirty_word

    shopt -u expand_aliases
}

main
```

> [!WARNING]
> To make other scripts be unaffected by **literal text substitutions** with incident,
>
> always disable `alias_expanded` functionality after main script executes. 

## CH34-2 -- principles about parsing an aliased name
### 1\. Literal Substitution
#### A. The Trailing Space Rule (Chaining)

If an alias definition ends with a **space**, Bash will check the _next_ word for alias expansion.

```
    alias sudo='sudo '
    alias ll='ls -l'
    # Result: 'sudo ll' becomes 'sudo ls -l'
```

#### B. The Recursive Principle

Bash keeps expanding aliases until the resulting first word is no longer an alias.

```
alias a='b '
alias b='c '

function main(){
  eval "echo 'a'" # will result in echo `c`
}

main
``` 

### 2\. The "Parsing Time" Trap
Pay attention to parsing phrase (such as when this statement is parsed)

The Bash engine reads and parse block-by block (NOT parse them by per statement) at compile time.

For example,

Before executing this Bash script,

```
alias say_hello='echo Hello from Alias!'
alias say_dirty_word='echo dirty word from Alias!' 

main(){
    shopt -s expand_aliases

    # use alias
    say_hello
    echo ""
    say_dirty_word

    shopt -u expand_aliases
}

main
```

the Bash engine will read and parse `main` function block when seek `main(){` this line.

rather than parsing it by one statement. 

Thus, it is needed to tell Bash engine inserts two aliases name `say_hello` and `say_dirty_word` into the mapping table 

before the `main` function is defined  (and of course, NOT in the `main` function)

if you want to use these aliased name in `main` function 

since Bash engine searches the mapping table and expands aliases names just immediately when whole `main` function is parsed (at compile time) 

Otherwise, it is needed to use `eval` built-in command to tell the Bash engine dynamically re-parse at the executionn of the statement (and thus dynamically re-evaluates it), 

resulting in, forcing do follows at execution of `eval` statement, 

Bash engine will insert the alias name into mapping table 

then searching the mapping table and expands aliases names.

> [!WARNING]
> `eval` is dangerous.
>
> At the execution of `eval`, Bash engine is forced to be re-parsed, and thus will affect expanded results globally.
>
> see section 4 `Precedence and Overriding` for more details.

### 3\. The Role of `eval`

`eval` forces the shell to re-parse a string as raw code, triggering the entire expansion pipeline 

> [!NOTE]
> The expansion precedence is
>
> alias expansion-> variable expansion -> arithmetic expansion -> command expansion,
>
> see CH11 for more details.

| Feature | Direct Alias Call | `eval "alias_name"` |
| --- | --- | --- |
| **Parsing Context** | Static (at definition time) | **Dynamic (at runtime)** |
| **Inside Functions** | Usually fails | **Works consistently** |
| **Late Binding** | No  | **Yes (Reflects current alias table)** |


### 4\. Precedence and Overriding

When a command is issued, Bash resolves the name in this order:

1.  **Aliased name** (uses `alias`)
    
2.  **Keyword** (e.g., `if`, `function`)
    
3.  **Function**
    
4.  **Built-in function** (e.g., `cd`, `unset`)
    
5.  **Executable** (found in `$PATH`)
    

#### Using `command` to bypass
The `command` keyword tells Bash to **ignore aliases and functions**, going directly to builtins or files.

See CH7 for more details.

> [!NOTE]
> `command` built-in cannot see aliases.

`command my_alias` will return `command not found`.
    
### 5\. Advanced Magic: Function Injection
Aliases can be used as "templates" to define or override functions at runtime.

For example,

In a Bash script

    alias inject_logic='function func1(){ echo "New Logic"; }'
    eval "inject_logic"
    # Now func1() is updated in the global scope. For the explanation of the reason, see section 2


### 6\. Developer Cheat Sheet (Bash vs. C#)
These have been discussed in previous chapters. 

I will take notes that is used in this chapter.

| Bash Concept | C# Analogy | Key Takeaway |
| --- | --- | --- |
| **Alias** | `using Alias = ...` or Macros | Pure text replacement. |
| **`eval`** | `CSharpScript.Evaluate()` | Forces runtime compilation. |
| **`local -n`** | `ref` parameters | Enables Pass-by-Reference. |
| **Trailing Space** | Fluent Interface / Chaining | Allows aliases to "stack" on each other. |
| **`shopt -s expand_aliases`** | Compiler Flag | Must be enabled for aliases to work in scripts. |

### CH34-3 -- Pro-Tip or better usage
#### 1\. Pro-Tip: Conditional Alias (The "C# #if" Equivalent)
Use conditional logic to define aliases based on diffent environment:

```
if [[ "$OSTYPE" == "darwin"* ]]; then
    alias ls='ls -G' # macOS
else
    alias ls='ls --color=auto' # Linux
fi

main(){
  ls 

  ls -l

  ls -l
}

main
```

In the above script,

it gives an aliased name `ls` and overwrites the orginal behavior of `ls` built-in command

When using an aliased name `ls`, it will list items with background color green.

In different environments, listing item with background color green takes different options and arguments,

using alias can easier to resolve it.

You don't need to 

define a function (that needs to handle the other options of `ls` built-in command for different environment which is much more complex)

see following example.

```
function list_items(){
  local command_str=""
  declare -i i=0
  if [[ "$OSTYPE" == "darwin"* ]]; then
      command_str='ls -G' # macOS
  else
      command_str='ls --color=auto' # Linux
  fi
  for (( i=0; i<$# ; i+=2 )); do
    ## TODO: check the corresponding argument of option name is available for the option and other check (which are the most complex parts)
    ## ... lots of code

    # concatenate the `command_str` variable
    # with 1th passed argument (as option name), 2th passed argument (as corresponding argument of option name) (using zero-based index) on 0th iteration
    # with 3th passed argument (as option name), 4th passed argument (as corresponding argument of option name) (using zero-based index) on 1th iteration
    # and so on
    # However, for best practice, you need to first check the corresponding argument of option name is available for the option and other check (which are the most complex parts)
    # Otherwise, you will build an invalid command related to `ls`, causing error or unexpected behavior (which is extremely hard to debug it) when re-parse it with `eval`.
    command_str="$command_str '${$i+1}' '${$i+2}'"
  done

  eval "$command_str"
}

main(){
  list_item 

  list_item "-l"

  list_item "-l"
}

main
```

### CH34-4 -- issues about aliased name
#### Q1: what's the pros and cons of using aliased name compared to using functions
1. For aliased name in Bash,

* Pros:

  + Simplicity for simple command:

    Extremely easy to define for simple command shortcuts
  
  + Easier to implement cross-platform:

    In different environemts, the command arguments may be different.

    It is easier to handle the command arguments in diffent environment by aliasing the command arguments as a new name.

    see the example in `Pro-Tip: Conditional Alias` of CH34-3 as its usage.

* Cons (Fatal cons): 

  + No arguments support:

    Can't pass an argument in aliased name

  + Limited logic:

    Since you can't easily use `if`, repetitive loops, or local variable within standard alias.

  + Parsing issues:
 
    Aliases inside functions often fail unless wrapped in eval because they are resolved during parsing phrase at compiled time, rather than execution time.

    see `Parsing Traps` of CH34-2 for more details.

2. For functions in Bash

* Pros (which are more concerned relatively to its cons):

  + Argument support:
    
    Functions fully support positional parameters (`$1`, `$2`, `$@`).
    
  + Local scope:

    You can use the `local` keyword inside a function to prevent variable leakage, variable pollution.
    
  + Complex logic:

    Supports including loops, conditionals, and redirections.
    
  + Reliable resolution:

    Functions are resolved at execution time, making them much more stable inside scripts and other functions without needing `eval`

    since `eval` is NOT safe (might be injected for malicious use) and might cause unexpected behavior.
    
  + Overriding Capability:

    You can use functions to wrap and extend existing commands (using the `command` keyword to call the original).
    
* Cons
    
  + Lookup priority:

    While high, they sit below aliases in the command lookup order (see CH11).

    Thus if an alias and a function share the same name, the alias wins.

#### Q2: why use functions for logic and parameters rather than aliased names
it is discussed in Q1, see Q1.

#### Q3: which is better to use aliased name rather than functions
For command shortcuts especially for commands whose available options are different in different platform.

see the example in `Pro-Tip: Conditional Alias` of CH34-3 as its usage.

### CH34-5 -- conclusion
For robust script development:

1.  Use **Functions** for logic and parameters.
    
2.  Use **Aliases** for interactive shortcuts or simple command wrapping.
    
3.  Use **`eval`** whenever an alias must be resolved dynamically inside a function scope.
