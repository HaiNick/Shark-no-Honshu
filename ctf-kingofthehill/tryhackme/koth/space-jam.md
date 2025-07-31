# 🐧 Space Jam

```
22/tcp   open  ssh
23/tcp   open  telnet
80/tcp   open  http
9999/tcp open  abyss
```

{% code overflow="wrap" %}
```
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 1df0d5f2671e5599dec62685b386ea81 (RSA)
|   256 4f5f6298aab1dda28161169ba529cdbd (ECDSA)
|_  256 9b12b0f31ffbb7d8a89c6be6bdf44055 (ED25519)
23/tcp   open  telnet  Linux telnetd
9999/tcp open  http    Golang net/http server
| fingerprint-strings: 
|   FourOhFourRequest, GetRequest, HTTPOptions: 
|     HTTP/1.0 200 OK
|     Date: Fri, 02 Feb 2024 18:57:26 GMT
|     Content-Length: 1
|     Content-Type: text/plain; charset=utf-8
|   GenericLines, Help, LPDString, RTSPRequest, SIPOptions, SSLSessionReq, Socks5: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain
|     Connection: close
|     Request
|   OfficeScan: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain
|     Connection: close
|_    Request: missing required Host header
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port9999-TCP:V=7.93%I=7%D=2/2%Time=65BD3B16%P=x86_64-pc-linux-gnu%r(Get
SF:Request,75,"HTTP/1\.0\x20200\x20OK\r\nDate:\x20Fri,\x2002\x20Feb\x20202
SF:4\x2018:57:26\x20GMT\r\nContent-Length:\x201\r\nContent-Type:\x20text/p
SF:lain;\x20charset=utf-8\r\n\r\n\n")%r(HTTPOptions,75,"HTTP/1\.0\x20200\x
SF:20OK\r\nDate:\x20Fri,\x2002\x20Feb\x202024\x2018:57:26\x20GMT\r\nConten
SF:t-Length:\x201\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\n\r\n
SF:\n")%r(FourOhFourRequest,75,"HTTP/1\.0\x20200\x20OK\r\nDate:\x20Fri,\x2
SF:002\x20Feb\x202024\x2018:57:26\x20GMT\r\nContent-Length:\x201\r\nConten
SF:t-Type:\x20text/plain;\x20charset=utf-8\r\n\r\n\n")%r(GenericLines,58,"
SF:HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain\r\nCo
SF:nnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(RTSPRequest,58,"HTT
SF:P/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain\r\nConne
SF:ction:\x20close\r\n\r\n400\x20Bad\x20Request")%r(Help,58,"HTTP/1\.1\x20
SF:400\x20Bad\x20Request\r\nContent-Type:\x20text/plain\r\nConnection:\x20
SF:close\r\n\r\n400\x20Bad\x20Request")%r(SSLSessionReq,58,"HTTP/1\.1\x204
SF:00\x20Bad\x20Request\r\nContent-Type:\x20text/plain\r\nConnection:\x20c
SF:lose\r\n\r\n400\x20Bad\x20Request")%r(LPDString,58,"HTTP/1\.1\x20400\x2
SF:0Bad\x20Request\r\nContent-Type:\x20text/plain\r\nConnection:\x20close\
SF:r\n\r\n400\x20Bad\x20Request")%r(SIPOptions,58,"HTTP/1\.1\x20400\x20Bad
SF:\x20Request\r\nContent-Type:\x20text/plain\r\nConnection:\x20close\r\n\
SF:r\n400\x20Bad\x20Request")%r(Socks5,58,"HTTP/1\.1\x20400\x20Bad\x20Requ
SF:est\r\nContent-Type:\x20text/plain\r\nConnection:\x20close\r\n\r\n400\x
SF:20Bad\x20Request")%r(OfficeScan,76,"HTTP/1\.1\x20400\x20Bad\x20Request\
SF:r\nContent-Type:\x20text/plain\r\nConnection:\x20close\r\n\r\n400\x20Ba
SF:d\x20Request:\x20missing\x20required\x20Host\x20header");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```
{% endcode %}

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
systemd:x:0:0::/root:/bin/sh
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
lxd:x:106:65534::/var/lib/lxd/:/bin/false
mysql:x:107:111:MySQL Server,,,:/nonexistent:/bin/false
messagebus:x:108:112::/var/run/dbus:/bin/false
uuidd:x:109:113::/run/uuidd:/bin/false
dnsmasq:x:110:65534:dnsmasq,,,:/var/lib/misc:/bin/false
sshd:x:111:65534::/var/run/sshd:/usr/sbin/nologin
jordan:x:1000:1000:,,,:/home/jordan:/bin/bash
bunny:x:1001:1001:,,,:/home/bunny:/bin/rbash
telnetd:x:112:119::/nonexistent:/bin/false
colord:x:113:121:colord colour management daemon,,,:/var/lib/colord:/bin/false
postgres:x:114:122:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
steve:x:1002:1002::/home/steve:/bin/bash
```

{% code overflow="wrap" %}
```
root:$6$a.kpa8p7$8MyQWqdXi8kIyb4CgqAViyCa51fW55x0m3paAVcXESs3t03iuUuLq9OQRsoHk7c5HhIsBC5HDddfXWElTK1Ha0:18317:0:99999:7:::
daemon:*:17953:0:99999:7:::
bin:*:17953:0:99999:7:::
sys:*:17953:0:99999:7:::
sync:*:17953:0:99999:7:::
games:*:17953:0:99999:7:::
man:*:17953:0:99999:7:::
lp:*:17953:0:99999:7:::
mail:*:17953:0:99999:7:::
news:*:17953:0:99999:7:::
uucp:*:17953:0:99999:7:::
proxy:*:17953:0:99999:7:::
www-data:*:17953:0:99999:7:::
backup:*:17953:0:99999:7:::
list:*:17953:0:99999:7:::
irc:*:17953:0:99999:7:::
gnats:*:17953:0:99999:7:::
nobody:*:17953:0:99999:7:::
systemd-timesync:*:17953:0:99999:7:::
systemd-network:*:17953:0:99999:7:::
systemd-resolve:*:17953:0:99999:7:::
systemd-bus-proxy:*:17953:0:99999:7:::
syslog:*:17953:0:99999:7:::
_apt:*:17953:0:99999:7:::
lxd:*:18317:0:99999:7:::
mysql:!:18317:0:99999:7:::
messagebus:*:18317:0:99999:7:::
uuidd:*:18317:0:99999:7:::
dnsmasq:*:18317:0:99999:7:::
sshd:*:18317:0:99999:7:::
jordan:$6$6jKm02ie$qiEzYLKjGwoxz5Fl.r/oSXEqeX3qgAZRKmZqdz8GFpTck6lj5b3RUhA6weUckJSFHuFdAbvdI6K5YWGx7N5Ph.:18317:0:99999:7:::
bunny:$6$HOScqZHA$ewn.WIghxLt7yv.nn.6jtaR8HNaKGklcG5bQqZirIDtZUNIgnek3JREVx.0huE8.oYYsaGJv4FTJhLR296czW1:18317:0:99999:7:::
telnetd:*:18317:0:99999:7:::
colord:*:18317:0:99999:7:::
postgres:*:18317:0:99999:7:::
steve:!:19755:0:99999:7:::
systemd:$6$A1YRRuH8$6ShkDaxO/GSoPKPf2voWzZc1wcQjc1TI3UW8oQRJxU1fXCC3vebmjWOWGH11GZQLTUiXZgQTYcF3i62RvJL8d.:19755::::::
```
{% endcode %}

```
// Some code
```
