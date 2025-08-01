# extract and manipulate text with unix tools. example: parse command output, clean data, extract fields because regex is hard to remember

## basic text extraction
```bash
# grep - find and extract patterns
grep 'pattern' file.txt        # lines containing pattern
grep -o 'pattern' file.txt     # only the matching text
grep -v 'pattern' file.txt     # lines NOT containing pattern
grep -i 'pattern' file.txt     # case insensitive
grep -E 'reg(ex|exp)' file.txt # extended regex
grep -P '(?<=Name: ).*' file.txt  # perl regex with lookbehind

# cut - extract columns/fields
cut -d':' -f2 file.txt         # 2nd field, colon delimiter
cut -c1-5 file.txt             # characters 1-5
cut -d' ' -f1 file.txt         # first word per line
cut -d',' -f2,4 file.txt       # fields 2 and 4
```

## advanced text processing
```bash
# awk - powerful field processor
awk '{print $2}' file.txt      # print 2nd column
awk -F':' '{print $2}' file.txt  # custom field separator
awk '/pattern/ {print $1}' file.txt  # print field 1 where pattern matches
awk '{gsub(/old/, "new"); print}' file.txt  # global search/replace
awk 'NR==5 {print}' file.txt   # print line 5 only
awk '{$1=$1; print}' file.txt  # normalize whitespace

# sed - stream editor
sed 's/abc/xyz/' file.txt      # replace first abc with xyz per line
sed 's/abc/xyz/g' file.txt     # replace all abc with xyz
sed 's/.*: //' file.txt        # remove everything up to last colon
sed -n 's/^Name: //p' file.txt # print only lines starting with "Name:"
sed '5d' file.txt              # delete line 5
sed '1,5d' file.txt            # delete lines 1-5
```

## string manipulation
```bash
# tr - translate/replace characters
tr 'a-z' 'A-Z' < file.txt     # convert to uppercase
tr -d '\r' < file.txt          # remove carriage returns
tr -s ' ' < file.txt           # squeeze multiple spaces to one
tr ';' '\n' < file.txt         # replace semicolons with newlines
tr -d '[:punct:]' < file.txt   # remove punctuation

# character/substring extraction
head -c 10 file.txt            # first 10 characters
tail -c 10 file.txt            # last 10 characters
rev file.txt                   # reverse each line
```

## combining and formatting
```bash
# paste - merge files side by side
paste file1.txt file2.txt      # combine files column-wise
paste -d',' file1.txt file2.txt # use comma as delimiter

# column formatting
column -t file.txt             # format as table
column -t -s',' file.txt       # csv to table

# join files on common field
join file1.txt file2.txt       # join on first field
join -t':' -1 2 -2 1 file1.txt file2.txt  # custom delimiter and fields
```

## pattern matching and extraction
```bash
# perl one-liners for complex patterns
perl -ne 'print "$1\n" if /Name:\s*(\S+)/' file.txt  # extract after "Name:"
perl -pe 's/foo/bar/g' file.txt    # global replace
perl -ne 'print if /start/..!/end/' file.txt  # print between patterns

# bash parameter expansion
echo ${var##*/}                # remove everything up to last /
echo ${var%%.*}                # remove everything after first .
echo ${var//old/new}           # replace all occurrences
echo ${var:5:10}               # substring from position 5, length 10
```

## whitespace and line handling
```bash
# clean up whitespace
xargs < file.txt               # remove leading/trailing whitespace
awk '{$1=$1; print}' file.txt  # remove extra whitespace
sed 's/^[ \t]*//;s/[ \t]*$//' file.txt  # trim leading/trailing

# line operations
sort file.txt                  # sort lines
sort -u file.txt               # sort and remove duplicates
uniq file.txt                  # remove consecutive duplicates
wc -l file.txt                 # count lines
nl file.txt                    # number lines
```

## json and structured data
```bash
# jq for json processing
jq '.users[].name' data.json   # extract name field from users array
jq -r '.key' data.json         # raw output (no quotes)
jq '.[] | select(.status == "active")' data.json  # filter objects

# xml processing with xmlstarlet
xmlstarlet sel -t -v "//name" file.xml  # extract name elements
```

## practical examples
```bash
# extract usernames from ldap output
grep sAMAccountName output.txt | cut -d' ' -f2 | tr -d ' ' | sort -u

# clean up csv data
sed 's/"//g' data.csv | cut -d',' -f2 | sort | uniq

# extract ip addresses from logs
grep -oE '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' access.log

# get only alphabetic characters
sed 's/[^a-zA-Z]//g' file.txt

# extract everything after last colon
sed 's/.*: //' file.txt

# extract domain from email addresses
sed 's/.*@//' emails.txt

# convert windows line endings
tr -d '\r' < windows.txt > unix.txt

# extract fields from ps output
ps aux | awk '{print $2, $11}'  # pid and command
```

## quick data processing chains
```bash
# common pipeline patterns
cat file | grep pattern | cut -d' ' -f2 | sort | uniq
ps aux | grep -v grep | awk '{print $2}' | xargs kill
netstat -an | grep LISTEN | awk '{print $4}' | cut -d':' -f2 | sort -u
find . -name "*.log" | xargs grep ERROR | cut -d':' -f1 | sort -u
```

## (._.) common gotchas
```bash
# field separators matter
awk -F: '{print $2}'           # use -F for field separator
cut -d: -f2                    # use -d for delimiter

# regex flavors differ
grep -E 'regex'                # extended regex for grep
grep -P 'regex'                # perl regex for grep
sed -E 's/regex/replace/'      # extended regex for sed (or -r on linux)

# quote your variables
grep "$pattern" file           # quote variables with spaces
awk -v var="$value" '{print var}'  # pass shell variables to awk
```

# TODO: add csv processing, xml parsing, binary data extraction, encoding conversions