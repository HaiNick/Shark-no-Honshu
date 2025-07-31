# unshadow combine passwd/shadow because john needs unified format.

## purpose
combines `/etc/passwd` and `/etc/shadow` for password cracking with john the ripper.

## usage
```bash
unshadow passwd.txt shadow.txt > crackme.txt    # create john-compatible file
john crackme.txt                                # crack with john
hashcat -m 1800 crackme.txt wordlist.txt       # or use hashcat
```

## workflow
```bash
# 1. get the files
cat /etc/passwd > passwd.txt
cat /etc/shadow > shadow.txt

# 2. combine them  
unshadow passwd.txt shadow.txt > combined.txt

# 3. crack passwords
john --wordlist=/usr/share/wordlists/rockyou.txt combined.txt
john --show combined.txt                        # show cracked passwords
```

## output format
creates file with format: `username:hash:uid:gid:gecos:home:shell`
```
root:$6$salt$hash...:0:0:root:/root:/bin/bash
user:$6$salt$hash...:1000:1000:User:/home/user:/bin/bash
```

## hash identification
```bash
grep '$1$' combined.txt     # MD5 hashes
grep '$6$' combined.txt     # SHA-512 hashes  
grep '$y$' combined.txt     # yescrypt hashes
```

## notes
[!] requires both `/etc/passwd` and `/etc/shadow` files
[!] shadow file usually requires root access to read
[!] modern systems use strong hashes (SHA-512, yescrypt) - slow to crack
[!] part of john the ripper suite