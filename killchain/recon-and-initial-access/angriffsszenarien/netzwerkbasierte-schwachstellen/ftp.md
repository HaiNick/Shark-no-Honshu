# FTP

### **Einleitung**

FTP ist ein veraltetes, unverschlüsseltes Protokoll zur Dateiübertragung. Fehlkonfigurationen wie _Anonymous Login_, fehlende Verschlüsselung oder globale Schreibrechte ermöglichen es Angreifern, Dateien zu lesen, hochzuladen oder auszuführen.

***

### **Typische Angriffsszenarien**

#### **1. Anonymous Login testen**

```bash
ftp <target-ip>
```

Login:

```
Name: anonymous
Password: anonymous@domain.com
```

Oder automatisiert:

```bash
nmap -p 21 --script ftp-anon <target-ip>
```

***

#### **2. Inhalte auflisten & herunterladen**

```bash
ftp <target-ip>
ls
get filename.txt
```

***

#### **3. Schreibrechte testen / Payload ablegen**

```bash
put reverse_shell.sh
```

Prüfen, ob SUID-fähige oder ausführbare Pfade existieren (z. B. `/var/www/html`, `/cgi-bin/`, `/ftp/scripts/`).

***

#### **4. Upload + Trigger**

Wenn ein Webserver Zugriff auf das FTP-Verzeichnis hat, kann eine Datei über FTP hochgeladen und über HTTP getriggert werden:

```bash
put shell.php
```

Aufruf:

```
http://<target-ip>/uploads/shell.php
```

***

#### **5. Exploits bei verwundbaren FTP-Servern**

Prüfen auf verwundbare Versionen (z. B. `vsftpd 2.3.4` Backdoor, `ProFTPd`, `Pure-FTPd`).

```bash
nmap -p 21 --script ftp-vsftpd-backdoor <target-ip>
```

***

### **Sicherheitsmaßnahmen**

* Anonymer Zugriff deaktivieren (`anonymous_enable=NO`)
* Nur verschlüsselte Verbindungen zulassen (FTPS/SFTP)
* Chroot-Jails konfigurieren
* Schreibzugriffe auf das Nötigste beschränken
* Protokollierung aktivieren
