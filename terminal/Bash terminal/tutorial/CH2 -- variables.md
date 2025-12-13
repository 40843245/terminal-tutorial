# CH2 -- variables
## objectives 
You will know how to

  + define a variable
  + access a variable
  + assign a value to a variable

## CH2-1 -- define a variable
To define a global variable, you can simply assign a value into a variable.

```
COUNT=1
```

To define a local variable, you have to use `local` variable and assign a value into a variable.

```
parse_args(){
  local integer1=1
}
```

## CH2-2 -- access a variable
To access a variable, you have to simply add `$` followed by a variable

```
COUNT=1
$COUNT # access the variable `count`
```

## CH2-3 -- assign a value to a variable
To assign a value into a variable through `=`,

```
COUNT=1 # define COUNT global variable and initialize COUNT as 1  
$COUNT
COUNT=2 # assign 2 to COUNT variable.
```
