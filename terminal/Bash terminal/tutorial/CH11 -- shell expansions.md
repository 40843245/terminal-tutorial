# CH11 -- shell expansions
## objectives
You will know

    + brace expansion
    + tilde expansion
    + parameter and variable expansion
    + command substitution
    + arithmetic expansion
    + process substitution
    + word splitting
    + filename expansion
    + quote removal
    + priority of these expansions

## CH11-1 -- priority of these expansions

The priority from highest to lowest are as follows

    + brace expansion
    + tilde expansion, parameter and variable expansion, arithmetic expansion (done in inside-to-outside fashion), command substitution (done in left-to-right fashion), process substitution (if system supports it)
    + word splitting
    + filename expansion
    + quote removal

## CH11-2 -- brace expansion
Given a sequence that is wrapped by `{}` and seperated by `,`,  it will perform brace expansions on the sequence,

generating a sequence of string by replacing each element in the string that contains the sequence that is used for brace expansions (i.e. contains `{}`).

### Examples
#### Example 1

`brace-expansion-example-1.bash`

```
echo a{d,c,b}e
```

In above example,

| sequence used for brace expansions | `n`th iteration | element | generated string  in `n`th iteration |
| :-- | :-- | :-- | :-- |
| `{d,c,b}` | 0 | `d` | `ade` |
| `{d,c,b}` | 1 | `c` | `ace` |
| `{d,c,b}` | 2 | `b` | `abe` |

combining them together will get a sequence of string

```
ade ace abe
```

Therefore,

```
a{d,c,b}e
```

will be expanded into

```
ade ace abe
```

And thus, executing this script will echo

```
ade ace abe
```

#### Example 2

```
mkdir -p project/{src,bin,doc}
```

In above example,

| sequence used for brace expansions | `n`th iteration | element | generated string  in `n`th iteration |
| :-- | :-- | :-- | :-- |
| `{src,bin,doc}` | 0 | `src` | `project/src` |
| `{src,bin,doc}` | 1 | `bin` | `project/bin` |
| `{src,bin,doc}` | 2 | `doc` | `project/doc` |

combining them together will get a sequence of string

```
project/src project/bin project/doc
```

Therefore,

```
mkdir -p project/{src,bin,doc}
```

is equivalent to

```
mkdir -p project/src project/bin project/doc
```

executing this script will create these folders respectively

    + `src` folder under `project` folder relative to current path
    + `bin` folder under `project` folder relative to current path
    + `doc` folder under `project` folder relative to current path
