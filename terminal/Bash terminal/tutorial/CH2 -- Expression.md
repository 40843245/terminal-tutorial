# CH2 -- expression
## objectives
You will know how to

  + express an expreesion in Bash

## CH2 -- an expreesion in Bash

| expression | type | description | pros | cons | notes |
| :-- | :-- | :-- | :-- |
| `[]` | command | alias of `test` command | | NO features that `[[]]` has | MUST: Always quotate vaiable with double qoutation. |
| `[[]]` | composite command | | <ul><li>No expand participle and full path</li><li>supports logical operator inside `[[]]`</li><li>support Regex inside `[[]]`</li></ul> | BEST PRACTICE: Always quotate vaiable with double qoutation. |
