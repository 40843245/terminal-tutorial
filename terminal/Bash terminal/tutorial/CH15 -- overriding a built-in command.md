# CH15 -- overriding a built-in command
## objectives
You will know how to

  + override a built-in command
  + invoke the built-in command when overriding it

## CH15-1 -- override a built-in command
To override a built-in command (such as `unset`), 

just simply define a function with same name of the built-in command in a module.

Then when you want to use the overridden built-in command, just import the module.

## CH15-2 -- invoke the built-in command when overriding it
To invoke the built-in command (its behavior is same as original built-in command which is NOT overriden) when overriding it (such as `unset`),

just invoke the built-in command with preserved word `command` (such as `command unset`)

### Examples
#### Example 1
See example 1 in CH13-4. 
