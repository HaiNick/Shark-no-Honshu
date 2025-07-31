# Linux

<details>

<summary>Links:</summary>

[https://viperone.gitbook.io/pentest-everything/everything/everything-linux/linux-privilege-escalation-techniques](https://viperone.gitbook.io/pentest-everything/everything/everything-linux/linux-privilege-escalation-techniques)

</details>

## Seitenlinks

### Privilege Escalation

{% content-ref url="../../killchain/exploitation-persistence-and-privilege-escalation/privilege-escalation/linux-privesc.md" %}
[linux-privesc.md](../../killchain/exploitation-persistence-and-privilege-escalation/privilege-escalation/linux-privesc.md)
{% endcontent-ref %}

### Forensische Untersuchung

{% content-ref url="../../cheat-sheets/linux-forensik.md" %}
[linux-forensik.md](../../cheat-sheets/linux-forensik.md)
{% endcontent-ref %}

## Installation Programm

### Installation von `.deb`-Paketen

#### 1. Mit `dpkg`

```bash
sudo dpkg -i paketname.deb
sudo apt-get install -f
```

* Installiert Paket und behebt anschließend fehlende Abhängigkeiten.

#### 2. Mit `apt` (empfohlen)

```bash
sudo apt install ./paketname.deb
```

* Führt automatische Abhängigkeitsprüfung durch. `./` kennzeichnet lokale Datei.

#### 3. Mit `gdebi` (optional)

```bash
sudo apt install gdebi
sudo gdebi paketname.deb
```

* Installiert `.deb` mit automatischer Abhängigkeitsauflösung.

#### 4. Über GUI

* `.deb` doppelklicken → Installation via Software-Center o. Ä.

***

### Installation aus `.tar.gz`

1. Entpacken:

```bash
tar -xzf paketname.tar.gz
cd entpackter_ordner
```

2. Mögliche Installationsmethoden (je nach Inhalt):

**a) Mit `configure` / `make`:**

```bash
./configure
make
sudo make install
```

> Optional: `sudo make uninstall`, falls unterstützt.

**b) Mit Installationsscript:**

```bash
./install.sh
# oder
sudo ./install.sh
```

**c) Manuelles Kopieren:**

* Falls kein Build-System vorhanden: Readme/Install-Anleitung lesen, Dateien ggf. manuell verschieben.

{% hint style="info" %}
Hinweis: Bei `tar.gz` handelt es sich oft um Quellcode. Abhängigkeiten vorher prüfen/installieren.
{% endhint %}

***

## Deinstallation Programm

Um ein unter Linux installiertes Programm zu deinstallieren, das in `/opt/` liegt und als `programm.service` unter `systemctl` registriert ist, folgende Schritte:

1.  **Dienst stoppen und deaktivieren:**

    Sicherstellen, dass der Dienst gestoppt und deaktiviert ist, bevor das Programm deinstalliert wird.

    ```bash
    sudo systemctl stop programm.service
    sudo systemctl disable programm.service
    ```
2.  **Dienstdatei entfernen:**

    Entfernen der `.service`-Datei aus dem Systemd-Ordner. Diese liegt meist in `/etc/systemd/system/`.

    ```bash
    sudo rm /etc/systemd/system/programm.service
    ```

    Alternativ könnte sie auch in `/lib/systemd/system/` liegen, falls sie dort installiert wurde.
3.  **Systemd neu laden:**

    Aktualisieren der Systemd-Konfiguration, damit der Dienst vollständig entfernt wird.

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl reset-failed
    ```
4.  **Programmdateien aus `/opt/` entfernen:**

    Löschen des Installationsverzeichnisses des Programms, das normalerweise in `/opt/programmname` liegt.

    ```bash
    sudo rm -rf /opt/programmname
    ```
5.  **Zusätzliche Bereinigungen (optional):**

    Je nach Installation kann es vorkommen, dass das Programm noch weitere Dateien in anderen Verzeichnissen hinterlegt hat, wie z. B. in `/etc`, `/var`, oder in den Konfigurationsverzeichnissen des Benutzers (`~/.config/programmname`).
6. **System nach Resten durchsuchen (optional):**

Zur Sicherstellung, dass keine Dateien übrig bleiben, kann mit `find` nach dem Programmnamen gesucht werden:

```bash
sudo find / -name '*programmname*'
```

***

## Tastatur-Layout umstellen

### Alle verfügbaren Keymaps anzeigen

```bash
$ localectl list-keymaps
```

### Aktuelle Konfiguration anzeigen

```bash
$ setxkbmap -print -verbose 10
$ localectl status
oder auch
$ cat /etc/vconsole.conf ( https://wiki.archlinux.org/index.php/KEYMAP )
```

### Konfiguration setzen

```bash
$ sudo setxkbmap -layout de
```

## JRE herausfinden und setzen

```bash
# Verfügbare Versionen anzeigen
$ sudo archlinux-java status

# Andere Version setzen (hier java-17)
$ sudo archlinux-java set java-17-openjdk
```

## Service-Autostart aktivieren/deaktivieren

Folgender Befehl aktiviert/deaktiviert einen Service im Autostart.\
Die Services liegen in `/etc/init.d/` vor

```
sudo systemctl enable/disable service_name
```

***

<details>

<summary>Erweiterungen</summary>

[https://apps.kde.org/de/](https://apps.kde.org/de/)

[https://apps.kde.org/de/codevis/](https://apps.kde.org/de/codevis/)

[https://apps.kde.org/de/fielding/](https://apps.kde.org/de/fielding/)

[https://apps.kde.org/de/elf-dissector/](https://apps.kde.org/de/elf-dissector/)

[https://apps.kde.org/de/hashomatic/](https://apps.kde.org/de/hashomatic/)

[https://apps.kde.org/de/ktechlab/](https://apps.kde.org/de/ktechlab/)

[https://apps.kde.org/de/okteta/](https://apps.kde.org/de/okteta/)

[https://apps.kde.org/de/kgpg/](https://apps.kde.org/de/kgpg/)

[https://apps.kde.org/de/kcachegrind/](https://apps.kde.org/de/kcachegrind/)

</details>
