# CH7 -- comparison
## objectives
You will know how to 

  + compare two integers
  + compare two strings
  + check a file

## CH7-1 -- comparetwo integers
> [!NOTE]
> You can't compare two numbers using symbol (such as `!=`) inside `[[]]`.
>
> You have to use flag (like in IL) to do so.

> [!NOTE]
> You can compare two numbers using symbol (such as `!=`) inside `(())` C-style arithmetic spread operator.

<details>
<summary>
inside `[[]]`
</summary>
  
| flag | description |
| :-- | :-- |
| `-eq` | equal to |
| `-ne` | not equal to |
| `lt` | less than |
| `gt` | greater than |
| `le` | less than or equal to |
| `ge` | greater than or equal to |

</details>


<details>
<summary>
inside `(())` C-style arithmetic spread operator.
</summary>
  
| operator | description |
| :-- | :-- |
| `==` | equal to |
| `!=` | not equal to |
| `<` | less than |
| `>` | greater than |
| `<=` | less than or equal to |
| `>=` | greater than or equal to |

</details>

### Examples
#### Example 1
To check status code `STATUS_CODE` is equal to 0.

```
STATUS_CODE=0
```

You can write

```
if [[ $STATUS_CODE -eq 0 ]];
```

or equivalently

``
if (($STATUS_CODE == 0)); 
```

## CH7-2 -- compare two strings
`see [bash的string testing](https://docs.google.com/spreadsheets/d/1_lipmlEwKus0CVZgdS93Zda5gASe4g2bqrd4JsmQZM4/edit?gid=1895558629#gid=1895558629)

## CH7-3 -- check a file
see [bash的File Testing](https://docs.google.com/spreadsheets/d/1sSjh6nQffVeyuqQo4JFjFje6DZ7LzkrtOl_s688bUEM/edit?gid=1954033718#gid=1954033718)
