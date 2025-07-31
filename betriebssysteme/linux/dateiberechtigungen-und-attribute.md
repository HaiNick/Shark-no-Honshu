# linux file permissions because access control matters.

## permission structure
```
drwxr-xr-x    # directory with permissions
-rw-r--r--    # regular file permissions
```

format: `[type][owner][group][others]`
- position 1: file type (`-` file, `d` directory, `l` link)
- positions 2-4: owner permissions (rwx)
- positions 5-7: group permissions (rwx)  
- positions 8-10: other permissions (rwx)

## permission types
- `r` (read): view file content, list directory
- `w` (write): modify file, create/delete in directory
- `x` (execute): run file, enter directory

## numeric notation
```
4 = read (r)
2 = write (w)  
1 = execute (x)
```

common combinations:
```
755 = rwxr-xr-x    # owner: full, group/others: read+execute
644 = rw-r--r--    # owner: read+write, group/others: read only
600 = rw-------    # owner: read+write, group/others: none
777 = rwxrwxrwx    # everyone: full permissions
```

## modify permissions
```bash
chmod 755 file.txt              # set specific permissions
chmod u+x file.txt              # add execute for user
chmod g-w file.txt              # remove write for group
chmod o=r file.txt              # set others to read only
chmod a+x file.txt              # add execute for all
```

## change ownership
```bash
chown user:group file.txt       # change owner and group
chown user file.txt             # change owner only
chown :group file.txt           # change group only
chown -R user:group directory/  # recursive change
```

## special permissions
```bash
chmod +s file.txt               # SUID/SGID bit
chmod +t directory/             # sticky bit (only owner can delete)
```

SUID/SGID in ls output:
```
-rwsr-xr-x    # SUID set (s in owner execute)
-rwxr-sr-x    # SGID set (s in group execute)  
drwxrwxrwt    # sticky bit (t in others execute)
```

## security notes
[!] SUID binaries run with owner privileges - major privesc vector
[!] 777 permissions = security nightmare
[!] sticky bit prevents deletion by non-owners (like /tmp)