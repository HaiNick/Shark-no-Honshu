---
description: https://tryhackme.com/room/expose
---

# expose

## Gegeben

* IP

## Vorgehen

### 1. NMAP-Scan:

{% code overflow="wrap" %}
```
21:ftp
22:ssh
53:domain
1337:waste
1883:mqtt

21/tcp   open  ftp                     vsftpd 2.0.8 or later
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.14.66.169
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
22/tcp   open  ssh                     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 c02168cf0362d727cac0d334305e326f (RSA)
|   256 238806911cab5e481881679f36c464a3 (ECDSA)
|_  256 5e621be80110d1d66ac01632631c4bd0 (ED25519)
53/tcp   open  domain                  ISC BIND 9.16.1 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.16.1-Ubuntu
1337/tcp open  http                    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: EXPOSED
1883/tcp open  mosquitto version 1.6.9
| mqtt-subscribe: 
|   Topics and their most recent payloads: 
|     $SYS/broker/clients/active: 1
|     $SYS/broker/load/sockets/5min: 0.39
|     $SYS/broker/load/bytes/received/15min: 4.57
|     $SYS/broker/store/messages/bytes: 146
|     $SYS/broker/messages/received: 3
|     $SYS/broker/retained messages/count: 35
|     $SYS/broker/load/publish/sent/1min: 10.05
|     $SYS/broker/load/messages/received/1min: 2.74
|     $SYS/broker/messages/sent: 14
|     $SYS/broker/heap/maximum: 51456
|     $SYS/broker/load/bytes/sent/5min: 79.73
|     $SYS/broker/clients/maximum: 1
|     $SYS/broker/load/connections/15min: 0.13
|     $SYS/broker/heap/current: 51056
|     $SYS/broker/load/bytes/sent/15min: 26.90
|     $SYS/broker/publish/messages/sent: 11
|     $SYS/broker/load/messages/sent/15min: 0.93
|     $SYS/broker/bytes/sent: 406
|     $SYS/broker/version: mosquitto version 1.6.9
|     $SYS/broker/publish/bytes/sent: 60
|     $SYS/broker/clients/inactive: 0
|     $SYS/broker/bytes/received: 69
|     $SYS/broker/clients/disconnected: 0
|     $SYS/broker/uptime: 319 seconds
|     $SYS/broker/load/bytes/received/5min: 13.55
|     $SYS/broker/load/connections/1min: 1.83
|     $SYS/broker/subscriptions/count: 2
|     $SYS/broker/load/sockets/15min: 0.13
|     $SYS/broker/load/messages/sent/5min: 2.75
|     $SYS/broker/load/bytes/sent/1min: 370.96
|     $SYS/broker/store/messages/count: 31
|     $SYS/broker/clients/connected: 1
|     $SYS/broker/load/publish/sent/5min: 2.16
|     $SYS/broker/messages/stored: 31
|     $SYS/broker/load/publish/sent/15min: 0.73
|     $SYS/broker/load/messages/received/15min: 0.20
|     $SYS/broker/load/sockets/1min: 1.67
|     $SYS/broker/load/bytes/received/1min: 63.04
|     $SYS/broker/clients/total: 1
|     $SYS/broker/load/messages/sent/1min: 12.79
|     $SYS/broker/load/messages/received/5min: 0.59
|_    $SYS/broker/load/connections/5min: 0.39
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
{% endcode %}

### 2. DIRB-Scan

{% code overflow="wrap" fullWidth="false" %}
```
---- Scanning URL: http://10.10.4.112:1337/ ----
==> DIRECTORY: http://10.10.4.112:1337/admin/                                  
+ http://10.10.4.112:1337/index.php (CODE:200|SIZE:91)                         
==> DIRECTORY: http://10.10.4.112:1337/javascript/                             
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/                             
+ http://10.10.4.112:1337/server-status (CODE:403|SIZE:278)                    
                                                                               
---- Entering directory: http://10.10.4.112:1337/admin/ ----
==> DIRECTORY: http://10.10.4.112:1337/admin/assets/                           
+ http://10.10.4.112:1337/admin/index.php (CODE:200|SIZE:1534)                 
==> DIRECTORY: http://10.10.4.112:1337/admin/modules/                          
                                                                               
---- Entering directory: http://10.10.4.112:1337/javascript/ ----
==> DIRECTORY: http://10.10.4.112:1337/javascript/jquery/                      
                                                                               
---- Entering directory: http://10.10.4.112:1337/phpmyadmin/ ----
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/doc/                         
+ http://10.10.4.112:1337/phpmyadmin/favicon.ico (CODE:200|SIZE:22486)         
+ http://10.10.4.112:1337/phpmyadmin/index.php (CODE:200|SIZE:14773)           
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/js/                          
+ http://10.10.4.112:1337/phpmyadmin/libraries (CODE:403|SIZE:278)             
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/                      
+ http://10.10.4.112:1337/phpmyadmin/phpinfo.php (CODE:200|SIZE:14775)         
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/sql/                         
+ http://10.10.4.112:1337/phpmyadmin/templates (CODE:403|SIZE:278)             
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/themes/                      
+ http://10.10.4.112:1337/javascript/jquery/jquery (CODE:200|SIZE:271809)                                                                                
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/doc/html/                                                                                                 
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/js/transformations/          
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/js/vendor/                                                                                                  
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/ar/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/az/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/be/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/bg/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/ca/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/cs/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/da/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/de/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/el/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/es/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/et/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/fi/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/fr/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/gl/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/hu/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/ia/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/id/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/it/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/ja/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/ko/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/lt/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/nl/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/pl/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/pt/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/pt_BR/                
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/ro/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/ru/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/si/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/sk/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/sl/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/sq/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/sv/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/th/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/tr/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/uk/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/vi/                   
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/zh_CN/                
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/locale/zh_TW/                
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/themes/original/             
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/doc/html/_images/            
+ http://10.10.4.112:1337/phpmyadmin/doc/html/index.html (CODE:200|SIZE:14929) 
==> DIRECTORY: http://10.10.4.112:1337/phpmyadmin/js/vendor/jquery/                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       
```
{% endcode %}

### 3. User

```
```



### 4. MQTT

{% code overflow="wrap" %}
```
https://book.hacktricks.xyz/network-services-pentesting/1883-pentesting-mqtt-mosquitto
https://github.com/bapowell/python-mqtt-client-shell
```
{% endcode %}



### x. Root Flag

root.txt gefunden
