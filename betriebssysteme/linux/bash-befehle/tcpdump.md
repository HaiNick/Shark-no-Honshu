# tcpdump

```
sudo tcpdump port 110 -A  #Pakete von Port 110 filtern und im (A)scii-Format anzeigen
```



```
# ICMP (ping) -Datenverkehr mitlesen
sudo tcpdump ip proto \\icmp -i tun0
```



