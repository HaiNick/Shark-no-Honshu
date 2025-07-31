# Aktive Reconnaissance

Direktes Agieren mit dem Ziel.

Typische Aktivitäten sind:

* Verbindung zu Unternehmensservern testen ( HTTP, FTP, SMTP, ping, etc. )
* Anrufe durchführen ( Social Engineering )
* Betreten des Geländes/ Sicherheitsbereiche, etc. durch Ausgeben als Handwerker etc.

## Ping ( ICMP )

Verbindung zu Zieladresse testen

```
ping -c <ip> # Linux
ping -n <ip> # Windows
```

| Parameter | Bedeutung                               |
| --------- | --------------------------------------- |
| -c (-n)   | count ( Anzahl zu versendender Pakete ) |
| -s        | size ( Paketgröße pro Einzelpaket )     |
|           |                                         |

Microsoft Firewall blockiert ICMP per Default

## Telnet:23 ( Teletype Network )

Kommunikation mit Remote System via CLI (Command Line Interface).\
Es kann mit jedem Service der mit TCP arbeitet kommuniziert werden.\
-> HTTP, FTP, SMTP, POP3, IMAP ( [protokolle-und-server](../informationen/protokolle-und-server/ "mention") )

Daten werden in Cleartext versendet, auch Username/Password

```
telnet <ip> <port>

z.B telnet 10.10.10.10 80  # Kommunikation mit Webserver ( Banner Grab )
Trying 10.10.10.10...
Connected to MACHINE_IP.
Escape character is '^]'.
GET / HTTP/1.1
host: telnet

```

## Traceroute ( UDP )&#x20;

TTL (Time to Live) bestimmt Anzahl an Hops die ein Paket erreichen kann. Wenn die TTL auf 0 fällt, liefert der empfangende Knoten eine `"ICMP time exceeded in-transit error message"`

Bei jedem Durchlauf von traceroute werden 3 Pakete mit TTL=( i - 1) für i=n -> 0 versendet.

```
# Linux
traceroute <ip>

# Windows
tracert <ip>
```

<pre data-full-width="true"><code>traceroute to &#x3C;REDACTED>.com (172.67.27.10), 30 hops max, 60 byte packets
 1  _gateway (10.0.2.2)  0.146 ms  0.104 ms  0.079 ms
 2  192.168.0.1 (192.168.0.1)  14.442 ms  14.418 ms  14.473 ms
 3  * * *
 4  de-kae01a-ra03-ae-18-0.aorta.net (84.116.190.253)  41.751 ms  41.726 ms  41.505 ms
 5  de-fra04d-rc1-ae-39-0.aorta.net (84.116.190.237)  41.678 ms  41.655 ms  41.622 ms
 6  84.116.190.94 (84.116.190.94)  41.401 ms  41.197 ms  41.364 ms
 7  162.158.84.8 (162.158.84.8)  41.270 ms * *
<strong> 8  162.158.92.2 (162.158.92.2)  58.641 ms  58.599 ms 172.70.240.3 (172.70.240.3)  34.539 ms # 2 Knotenpunkte
</strong> 9  172.67.27.10 (172.67.27.10)  34.425 ms  34.406 ms  34.488 ms                            
 
<strong>[?] Die Ergebnisse der Anfragen mit 3 simultan gesendeten Paketen zeigt, dass gleiche Pakete über unterschiedliche
</strong><strong>Knotenpunkte laufen. Die zuerst gezeigte IP in jeder Zeile ist der Knoten, der am meisten Antworten zurückliefert.
</strong></code></pre>

Manche Routerkonfigurationen senden keine Error-Message zurück!

## Netcat ( TCP & UDP )

Es kann als Client fungieren, der eine Verbindung zu einem abhörenden Port herstellt; alternativ kann es als Server fungieren, der an einem Port lauscht.

```
nc <ip> <port>  # Client

nc ip 80     # Banner Grab
GET / HTTP/1.1
host: netcat

nc -lvnp <port> # Server
```

| Parameter | Bedeutung                                      |
| --------- | ---------------------------------------------- |
| -l        | Modus (Listen)                                 |
| -p        | Port                                           |
| -n        | keine Auflösung via DNS (hostname)             |
| -v        | ausführlicher(verbose) output                  |
| -vv       | noch ausführlicher                             |
| -k        | (keep) weiterlauschen wenn client disconnected |

## ARP

```
arp-scan <ip>/<subnetz>

# ARP-Query für alle validen IPs
arp-scan -l            # im lokalen Netz
arp-scan --localnet    # " "
arp-scan -I eth0 -l  #  auf Interface eth0

```

## fping

```
fping -g <ip>/<subnet>
```

## GoBuster

```
gobuster -w
```

Cheatsheet: [https://3os.org/penetration-testing/cheatsheets/gobuster-cheatsheet/](https://3os.org/penetration-testing/cheatsheets/gobuster-cheatsheet/)

## nmap

Siehe: [nmap.md](../programme-skripte/nmap.md "mention")&#x20;

## Masscan

aggressiver Scanner

```
masscan <ip>/<subnetz> -p<port>,<port>
```
