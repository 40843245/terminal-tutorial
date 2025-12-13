# CH9 -- Regex 
##  objectives
You will learn how to 

  + defines a Regex.

## CH9-1 -- Regex

| regex symbol | meaing |
| :-- | :-- |
| `.` | any one characters (exclusive to `\n`)|
| `+` | zero or more times (of previous one character or group) |
| `*` | one or more times (of previous one character or group) |
| `?` | zero or one time (of previous one character or group) |

| regex symbol | meaing |
| :-- | :-- |
| `^` | starting from specified characters |
| `$` | end from specified characters |
| `/<` | the end point |
| `>/` | the start point |

| regex symbol | meaing |
| :-- | :-- |
| `[]` | matchs one of these characters inside `[]`|
| `-` (inside `[]`) | matchs one of these characters that ranges from left-hand side of `-` to right-hand side of `-` inside `[]` |
| `[^ ]` | NOT match one of these characters inside `[]` |
