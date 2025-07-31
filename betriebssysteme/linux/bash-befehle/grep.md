# grep pattern matching because searching text.

```bash
grep -A 3 -B 2 "pattern" file.txt      # 3 lines after, 2 before match
grep -r "string" /path/                 # recursive search in directory
grep -i "pattern" file.txt              # case insensitive
grep -v "pattern" file.txt              # invert match (exclude)
grep -n "pattern" file.txt              # show line numbers
grep -E "regex|pattern" file.txt        # extended regex
grep -c "pattern" file.txt              # count matches only
```

## common patterns
```bash
grep "^pattern" file.txt                # starts with pattern
grep "pattern$" file.txt                # ends with pattern  
grep "[0-9]" file.txt                   # contains numbers
grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" file.txt    # IP addresses
```

## useful combos
```bash
ps aux | grep process_name              # find running process
grep -r "password" /etc/ 2>/dev/null    # search configs for passwords
grep -v "^#" config.file | grep -v "^$" # remove comments and empty lines
```

## notes
[!] use quotes around patterns with spaces or special chars
[!] `-r` on large directories can be slow
