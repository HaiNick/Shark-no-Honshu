# find files/directories because locate everything.

```bash
find / -name *name.txt                  # search by filename
find / -perm /4000 2>/dev/null          # find SUID binaries  
find /path -type f -name "*.log"        # find log files
find /path -user username               # files owned by user
find /path -mtime -7                    # modified last 7 days
find /path -size +100M                  # files larger than 100MB
```

## usage
- `-name`: filename pattern (use wildcards)
- `-type f`: files only (`d` for directories)
- `-perm`: permission search (`/4000` = SUID, `/2000` = SGID)
- `-user`/`-group`: ownership
- `-mtime`/`-atime`: modification/access time
- `-size`: file size (`+` larger, `-` smaller)
- `2>/dev/null`: suppress permission errors

## notes
[!] searching from `/` can be slow and noisy
[!] SUID binaries can be privilege escalation vectors
