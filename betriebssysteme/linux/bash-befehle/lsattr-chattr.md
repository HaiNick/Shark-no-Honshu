# lsattr/chattr file attributes because filesystem permissions aren't enough.

## view attributes
```bash
lsattr file.txt                         # show file attributes
lsattr -d directory/                    # directory attributes only
lsattr -R directory/                    # recursive listing
lsattr -a                               # include hidden files
```

## common attributes
```
i = immutable (cannot modify/delete)
a = append-only (can only append data)
e = extent format (ext4 default)
s = secure deletion (zero on delete)
u = undeletable (allow undelete)
```

## modify attributes
```bash
chattr +i file.txt                      # make immutable
chattr -i file.txt                      # remove immutable
chattr +a logfile.txt                   # append-only mode
chattr -a logfile.txt                   # remove append-only
chattr +s sensitive.txt                 # secure deletion
```

## attribute combinations
```bash
chattr +ai file.txt                     # append-only + immutable
chattr -ai file.txt                     # remove both
chattr =i file.txt                      # set only immutable (clear others)
```

## persistence vector
attackers use immutable attribute to prevent cleanup:
```bash
chattr +i /tmp/.hidden_backdoor         # make backdoor undeletable
chattr +i /etc/passwd                   # prevent user removal
```

## defense/analysis
```bash
find /tmp -type f -exec lsattr {} \; | grep "\-i\-"    # find immutable files
lsattr /etc/passwd /etc/shadow          # check critical files
```

## notes
[!] immutable files cannot be deleted even by root
[!] remove +i attribute before trying to delete files  
[!] append-only useful for log files - prevents tampering
[!] some attributes require filesystem support (ext2/3/4)