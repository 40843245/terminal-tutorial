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
    printf "current year (with four digits):\`%s\`\n" "$(date +"%Y")"
    printf "current year (with two digits):\`%s\`\n" "$(date +"%y")"
    printf "current month:\`%s\`\n" "$(date +"%m")"
    printf "current month (NOT shorthand):\`%s\`\n" "$(date +"%B")"
    printf "current month (shorthand):\`%s\`\n" "$(date +"%b")"
    printf "current date:\`%s\`\n" "$(date +"%d")"
    printf "current days in this week (indexed number starts from 0,0:Sunday):\`%s\`\n" "$(date +"%u")"
    printf "current days in this week (indexed number starts from 1,0:Monday):\`%s\`\n" "$(date +"%w")"
    printf "current days in this week (NOT shorthand):\`%s\`\n" "$(date +"%A")"
    printf "current days in this week (shorthand):\`%s\`\n" "$(date +"%a")"
    printf "current days in this month:\`%s\`\n" "$(date +"%e")"
    printf "current days in this year:\`%s\`\n" "$(date +"%j")"
    printf "current week in this year (indexed number starts from 0,0:Sunday):\`%s\`\n" "$(date +"%U")"
    printf "current week in this year (indexed number starts from 1,0:Monday):\`%s\`\n" "$(date +"%W")"
    printf "current hour (24 clock culture):\`%s\`\n" "$(date +"%H")"
    printf "current hour (12 clock culture):\`%s %s\`\n" "$(date +"%I")" "$(date +"%P")"
    printf "current hour (12 clock culture):\`%s %s\`\n" "$(date +"%I")" "$(date +"%p")"
    printf "current minute:\`%s\`\n" "$(date +"%M")"
    printf "current seconds:\`%s\`\n" "$(date +"%S")"
    printf "current nanoseconds:\`%s\`\n" "$(date +"%N")"
    printf "current full date (format:\`%%Y-%%m-%%d\`):\`%s\`\n" "$(date +"%F")"
    printf "current full date (in Aermica format:\`%%m/%%d/%%y\`):\`%s\`\n" "$(date +"%D")"
    printf "current full time (format:\`%%H:%%M:%%S\`):\`%s\`\n" "$(date +"%T")"
    printf "current full datetime (24 clock culture) (format:\`%%H:%%M\`):\`%s\`\n" "$(date +"%R")"
    printf "current full datetime (12 clock culture) (format:\`%%I:%%M:%%S %%p\`):\`%s\`\n" "$(date +"%r")"
    printf "current Unix timestamp:\`%s\`\n" "$(date +"%s")"
    printf "current time zone name:\`%s\`\n" "$(date +"%Z")"
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
current year (with two digits):`25`
current month:`12`
current month (NOT shorthand):`December`
current month (shorthand):`Dec`
current date:`27`
current days in this week (indexed number starts from 0,0:Sunday):`6`
current days in this week (indexed number starts from 1,0:Monday):`6`
current days in this week (NOT shorthand):`Saturday`
current days in this week (shorthand):`Sat`
current days in this month:`27`
current days in this year:`361`
current week in this year (indexed number starts from 0,0:Sunday):`51`
current week in this year (indexed number starts from 1,0:Monday):`51`
current hour (24 clock culture):`17`
current hour (12 clock culture):`05 pm`
current hour (12 clock culture):`05 PM`
current minute:`17`
current seconds:`10`
current nanoseconds:`852372900`
current full date (format:`%Y-%m-%d`):`2025-12-27`
current full date (in Aermica format:`%m/%d/%y`):`12/27/25`
current full time (format:`%H:%M:%S`):`17:17:11`
current full datetime (24 clock culture) (format:`%H:%M`):`17:17`
current full datetime (12 clock culture) (format:`%I:%M:%S %p`):`05:17:11 PM`
current Unix timestamp:`1766827031`
current time zone name:`TST`

```
