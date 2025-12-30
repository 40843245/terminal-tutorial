# CH25 -- Bash version
## objectives
You will know how to

  + list version of Bash

## CH25-1 -- list version of Bash
### `$BASH_VERSINFO`
`$BASH_VERSINFO` is an array containing the metadata about version of Bash

`$BASH_VERSINFO[0]` indicates major (i.e. release) version number of Bash

`$BASH_VERSINFO[1]` indicates minor version number of Bash

`$BASH_VERSINFO[2]` indicates patch version number of Bash

`$BASH_VERSINFO[3]` indicates build version number of Bash

`$BASH_VERSINFO[4]` indicates release version status of Bash

`$BASH_VERSINFO[5]` indicates value of special `$MACHTYPE`.

### Examples
#### Example 1
`version-example-1.bash`

```
function print_version_info(){
    echo "----------- version info -----------"
    echo "current major (i.e. release) version number:\`${BASH_VERSINFO[0]}\`"
    echo "current minor version number:\`${BASH_VERSINFO[1]}\`"
    echo "current patch version number:\`${BASH_VERSINFO[2]}\`"
    echo "current build version number:\`${BASH_VERSINFO[3]}\`"
    echo "current release version status:\`${BASH_VERSINFO[4]}\`"
    echo "current \`MACHTYPE\` special variable:\`${BASH_VERSINFO[5]}\`"
    echo "Expands to a string describing the version of this instance of Bash:\`${BASH_VERSINFO[5]}\`"
    echo "----------- end of version info -----------"
}

main(){
    print_version_info    
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\version\version-example-1.bash"
----------- version info -----------
current major (i.e. release) version number:`5`
current minor version number:`2`
current patch version number:`37`
current build version number:`1`
current release version status:`release`
current `MACHTYPE` special variable:`x86_64-pc-msys`
Expands to a string describing the version of this instance of Bash:`x86_64-pc-msys`
----------- end of version info -----------

```

#### Example 2
`version-example-2.bash`

```
function print_version_info(){
    echo "----------- version info -----------"
    bash --version
    echo "----------- end of version info -----------"
}

main(){
    print_version_info    
}

main
```

executing this script will echo

```
$ "D:\workspace\Bash\Bash tutorial\examples\version\version-example-2.bash"
----------- version info -----------
GNU bash, version 5.2.37(1)-release (x86_64-pc-msys)
Copyright (C) 2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>

This is free software; you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
----------- end of version info -----------

```

