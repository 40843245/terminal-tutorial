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
  + which is better to use aliased name than using functions.
    
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

1.  **Alias** (Highest priority)
    
2.  **Keyword** (e.g., `if`, `function`)
    
3.  **Function**
    
4.  **Builtin** (e.g., `cd`, `unset`)
    
5.  **Executable** (Found in `$PATH`)
    

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
    # Now func1() is updated in the global scope.


## 6\. Developer Cheat Sheet (Bash vs. C#)

| Bash Concept | C# Analogy | Key Takeaway |
| --- | --- | --- |
| **Alias** | `using Alias = ...` or Macros | Pure text replacement. |
| **`eval`** | `CSharpScript.Evaluate()` | Forces runtime compilation. |
| **`local -n`** | `ref` parameters | Enables Pass-by-Reference. |
| **Trailing Space** | Fluent Interface / Chaining | Allows aliases to "stack" on each other. |
| **`shopt -s expand_aliases`** | Compiler Flag | Must be enabled for aliases to work in scripts. |

Export to Sheets

* * *

## 7\. Conclusion

For robust script development:

1.  Use **Functions** for logic and parameters.
    
2.  Use **Aliases** for interactive shortcuts or simple command wrapping.
    
3.  Use **`eval`** whenever an alias must be resolved dynamically inside a function scope.



