# bash scripting essentials for command line automation. example: loops, functions, variables because shell scripts get stuff done

## string manipulation commands  
- [manipulation-von-strings](manipulation-von-strings.md) - extract, search, replace text with unix tools

## quick bash reference
```bash
# variables and expansion
var="value"                    # set variable (no spaces around =)
echo $var                      # use variable
echo ${var}                    # explicit variable expansion
echo ${var:-default}           # use default if var is empty
echo ${var##*/}                # remove everything up to last /
echo ${var%%.*}                # remove everything after first .

# command substitution
result=$(command)              # capture command output
result=`command`               # old style (avoid)

# arithmetic
((i++))                        # increment variable
result=$((5 + 3))              # arithmetic expansion
if (( i < 10 )); then          # arithmetic comparison

# arrays
arr=(a b c)                    # create array
echo ${arr[0]}                 # first element
echo ${arr[@]}                 # all elements
echo ${#arr[@]}                # array length
```

## special characters
```bash
$0                             # script name
$1 $2 $3                       # positional parameters
$@                             # all parameters as separate words
$*                             # all parameters as single word
$#                             # number of parameters
$?                             # exit status of last command
$$                             # process id of current shell
$!                             # process id of last background job
$_                             # last argument of previous command
```

## conditional tests
```bash
# file tests
[[ -f file ]]                  # file exists and is regular file
[[ -d dir ]]                   # directory exists
[[ -r file ]]                  # file is readable
[[ -w file ]]                  # file is writable
[[ -x file ]]                  # file is executable
[[ -s file ]]                  # file exists and is not empty

# string tests
[[ -z "$var" ]]                # string is empty
[[ -n "$var" ]]                # string is not empty
[[ "$a" == "$b" ]]             # strings are equal
[[ "$a" != "$b" ]]             # strings are not equal
[[ "$a" =~ regex ]]            # string matches regex

# numeric tests
[[ $a -eq $b ]]                # equal
[[ $a -ne $b ]]                # not equal
[[ $a -lt $b ]]                # less than
[[ $a -le $b ]]                # less than or equal
[[ $a -gt $b ]]                # greater than
[[ $a -ge $b ]]                # greater than or equal
```

## loops and conditionals
```bash
# for loops
for i in {1..10}; do echo $i; done
for file in *.txt; do echo $file; done
for ((i=0; i<10; i++)); do echo $i; done

# while loops
while read line; do echo $line; done < file.txt
while [[ $i -lt 10 ]]; do ((i++)); done

# if statements
if [[ condition ]]; then
    echo "true"
elif [[ other_condition ]]; then
    echo "maybe"
else
    echo "false"
fi

# case statements
case $var in
    pattern1) echo "match1" ;;
    pattern2) echo "match2" ;;
    *) echo "default" ;;
esac
```

## functions
```bash
# define function
function_name() {
    local var="local variable"
    echo "argument 1: $1"
    echo "argument 2: $2"
    return 0
}

# call function
function_name arg1 arg2
echo "function returned: $?"
```

## redirection and pipes
```bash
command > file                 # redirect stdout to file
command >> file                # append stdout to file
command 2> file                # redirect stderr to file
command 2>&1                   # redirect stderr to stdout
command < file                 # use file as stdin
command | other_command        # pipe stdout to other command
command |& other_command       # pipe stdout and stderr
```

## useful one-liners
```bash
# find and replace in files
sed -i 's/old/new/g' *.txt     # replace in all txt files
grep -r "pattern" . --include="*.py"  # recursive search in python files

# file operations
find . -name "*.log" -delete   # delete all log files
find . -type f -exec chmod 644 {} \;  # set permissions on all files

# process management
jobs                           # list background jobs
fg %1                          # bring job 1 to foreground
bg %1                          # send job 1 to background
nohup command &                # run command immune to hangup

# quick file server
python3 -m http.server 8000    # serve current directory on port 8000
```

## debugging and error handling
```bash
set -e                         # exit on any error
set -u                         # exit on undefined variable
set -x                         # print commands before execution
set -o pipefail                # exit on pipe failure

# error handling
command || echo "command failed"
if ! command; then echo "failed"; exit 1; fi

# trap signals
trap 'echo "script interrupted"' INT
trap 'cleanup_function' EXIT
```

# TODO: add process substitution, coprocesses, bash-specific features vs posix sh