# BlackArch

<details>

<summary>Links:</summary>

[https://blackarch.org/tools.html](https://blackarch.org/tools.html)

</details>



## JDK 21.0.7 manuell unter BlackArch installieren

Download der jdk-tar unter: [https://adoptium.net/download/](https://adoptium.net/download/)

#### Variante 1: Benutzerspezifische Einrichtung

1.  JDK-Verzeichnis entpacken, z. B. in:

    ```bash
    ~/Downloads/jdk-21.0.7+6/
    ```
2.  Umgebungsvariablen in der Shell-Konfigurationsdatei (`~/.bashrc`, `~/.zshrc`, o. ä.) ergänzen:

    ```bash
    export JAVA_HOME="$HOME/Downloads/jdk-21.0.7+6"
    export PATH="$JAVA_HOME/bin:$PATH"
    ```
3.  Änderungen laden:

    ```bash
    source ~/.bashrc
    ```
4.  Installation prüfen:

    ```bash
    java -version
    ```

***

#### Variante 2: Systemweite Einrichtung

1.  JDK nach `/opt` verschieben:

    ```bash
    sudo mv ~/Downloads/jdk-21.0.7+6 /opt/jdk-21.0.7
    ```
2.  Symbolische Links erstellen:

    ```bash
    sudo ln -s /opt/jdk-21.0.7/bin/java /usr/bin/java
    sudo ln -s /opt/jdk-21.0.7/bin/javac /usr/bin/javac
    ```
3.  Installation prüfen:

    ```bash
    java -version
    ```

***

{% hint style="info" %}
Die Verwendung von `archlinux-java` ist nur bei über das Paketmanagement installierten JDKs möglich. Bei manueller Einrichtung erfolgt die Verwaltung ausschließlich über Umgebungsvariablen oder symbolische Links.
{% endhint %}

***

## Fehlerhafte Mirrorliste reparieren

```bash
#!/bin/bash

# Backup der alten Mirrorliste
echo "[*] Backup der alten Mirrorliste wird erstellt unter /etc/pacman.d/mirrorlist.bak"
sudo cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak

# Neue Mirrorliste schreiben
echo "[*] Neue Mirrorliste wird erstellt ..."
sudo tee /etc/pacman.d/mirrorlist > /dev/null << 'EOF'
## Manuell eingetragene, schnelle HTTPS-Mirror (Deutschland)
Server = https://mirror.23media.com/archlinux/$repo/os/$arch
Server = https://mirror.netcologne.de/archlinux/$repo/os/$arch
Server = https://mirror.selfnet.de/archlinux/$repo/os/$arch
EOF

# Datenbank forcieren aktualisieren
echo "[*] Paketdatenbank wird aktualisiert ..."
sudo pacman -Syy

echo "[+] Fertig. Ihre Mirrorliste wurde erfolgreich ersetzt."
```
