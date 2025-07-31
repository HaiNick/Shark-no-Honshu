# /etc/shadow password hashes because security through obscurity.

## format
```
username:password_hash:last_change:min_days:max_days:warn_days:inactive:expire:reserved
```

## field breakdown
1. **username**: account name
2. **password_hash**: encrypted password (`*` or `!` = disabled)  
3. **last_change**: days since 1970-01-01 of last password change
4. **min_days**: minimum days between password changes
5. **max_days**: maximum days password valid
6. **warn_days**: warning days before expiration
7. **inactive**: days after expiry before account disabled
8. **expire**: account expiration date (days since 1970-01-01)

## hash formats
```
$1$ = MD5 (weak)
$2a$ = Blowfish
$2y$ = Blowfish  
$5$ = SHA-256
$6$ = SHA-512
$y$ = yescrypt (modern default)
```

## example entries
```bash
# SHA-512 hash
user:$6$salt$hash:19446:0:99999:7:::

# yescrypt hash (debian 11+)  
user:$y$j9T$salt$hash:19447:0:99999:7:::

# disabled account
user:*:19446:0:99999:7:::
```

## generate password hash
```bash
openssl passwd -6 -salt mysalt mypassword        # SHA-512
openssl passwd -1 -salt mysalt mypassword        # MD5 (legacy)
python3 -c "import crypt; print(crypt.crypt('password', crypt.METHOD_SHA512))"
```

## analysis
```bash
cat /etc/shadow                                  # requires root
grep '^\w*:\$' /etc/shadow                       # accounts with passwords
awk -F: '$2 ~ /^\$/ {print $1}' /etc/shadow      # users with hashes
grep ':*:' /etc/shadow                           # disabled accounts
```

## cracking prep
```bash
unshadow /etc/passwd /etc/shadow > combined.txt  # combine for john
john combined.txt                                # crack passwords
hashcat -m 1800 combined.txt wordlist.txt       # SHA-512 with hashcat
```

## notes
[!] readable only by root (mode 640, owner root:shadow)
[!] yescrypt is modern default - harder to crack than SHA-512
[!] empty password field = no password required (dangerous)
[!] `*` or `!` in password field = account disabled