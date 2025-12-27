# CH24 -- datetime format
## objectives
You will learn format of datetime.
## CH24-1 -- format of datetime
### `date`
syntax

```
date \+"{格式化字元們}"

{格式化字元們}: 多個(以%開頭)格式化字元
```

#### Examples
##### Example 1
`formatting-example-1.bash`

```
## utility function
## 主要目的
## set formatting that returns from `date`
function print_formatting_info(){
    printf "current year (with four digits):\`%s\`\n" $(date +"%Y")
    printf "current year (with two digits):\`%s\`\n" $(date +"%Y")
    printf "current month:\`%s\`\n" $(date +"%m")
    printf "current month (NOT shorthand):\`%s\`\n" $(date +"%B")
    printf "current month (shorthand):\`%s\`\n" $(date +"%b")
    printf "current date:\`%s\`\n" $(date +"%d")
    printf "current days in this month:\`%s\`\n" $(date +"%e")
    printf "current days in this year:\`%s\`\n" $(date +"%j")
    printf "current hour (24 clock culture):\`%s\`\n" $(date +"%H")
    printf "current minute:\`%s\`\n" $(date +"%M")
    printf "current seconds:\`%s\`\n" $(date +"%S")
    printf "current full date (format:\`%%Y-%%m-%%d\`):\`%s\`\n" $(date +"%F")
    printf "current full time (format:\`%%H:%%M:%%S\`):\`%s\`\n" $(date +"%T")
}

main(){
    print_formatting_info
}

set +e
main
set -e

```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\formatting\formatting-example-1.bash"
current year (with four digits):`2025`
current year (with two digits):`2025`
current month:`12`
current month (NOT shorthand):`December`
current month (shorthand):`Dec`
current date:`27`
current days in this month:`27`
current days in this year:`361`
current hour (24 clock culture):`15`
current minute:`20`
current seconds:`41`
current full date (format:`%Y-%m-%d`):`2025-12-27`
current full time (format:`%H:%M:%S`):`15:20:41`

```
