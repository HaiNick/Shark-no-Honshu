---
description: https://serverspace.io/support/help/windows-cmd-commands-cheat-sheet/
---

# Windows CMD

## Verwaltung von Dateien und Ordnern

- **`COPY`**: Kopiert Dateien an einen anderen Ort  
  ```bash
  COPY C:\Ordner\datei.txt D:\NeuerOrdner\
  ```

- **`DIR`**: Zeigt Dateien und Ordner im aktuellen Verzeichnis an  
  ```bash
  DIR
  ```

- **`DEL`** oder **`ERASE`**: Löscht Dateien  
  ```bash
  DEL C:\Ordner\datei.txt
  ```

- **`EDIT`**: Startet den Datei-Editor  
  ```bash
  EDIT C:\Ordner\datei.txt
  ```

- **`CD`**: Wechselt das Verzeichnis  
  ```bash
  CD D:\NeuerOrdner
  ```

- **`EXPAND`**: Dekomprimiert komprimierte Dateien  
  ```bash
  EXPAND C:\Archiv.zip -F:* D:\
  ```

- **`FC`**: Vergleicht Dateien und zeigt die Unterschiede zwischen ihnen an  
  ```bash
  FC C:\Ordner\datei1.txt C:\Ordner\datei2.txt
  ```

- **`FIND /N "" "datei"`**: Findet eine Textzeichenfolge in der Datei  
  ```bash
  FIND /N "Suchbegriff" C:\Ordner\datei.txt
  ```

- **`MD`** oder **`MAKEDIR`**: Erzeugt einen Ordner  
  ```bash
  MD C:\NeuerOrdner
  ```

- **`MOVE`**: Verschiebt Dateien von einem Ordner in einen anderen  
  ```bash
  MOVE C:\Ordner\datei.txt D:\NeuerOrdner\
  ```

- **`PRINT`**: Gibt den Inhalt der Textdatei aus  
  ```bash
  PRINT C:\Ordner\datei.txt
  ```

- **`RD`** oder **`RMDIR`**: Löscht einen Ordner  
  ```bash
  RD C:\Ordner
  ```

- **`REN`** oder **`RENAME`**: Benennt eine Datei oder einen Ordner um  
  ```bash
  REN C:\Ordner\alte_datei.txt neue_datei.txt
  ```

- **`REPLACE`**: Ersetzt Dateien in einem Verzeichnis durch gleichnamige Dateien in einem anderen Verzeichnis (Überschreiben)  
  ```bash
  REPLACE D:\NeuerOrdner\*.* C:\Ordner\
  ```

- **`ROBOCOPY`**: Verwendet ein fortschrittliches Werkzeug zum Kopieren von Dateien und Verzeichnissen  
  ```bash
  ROBOCOPY C:\Ordner D:\NeuerOrdner /E
  ```

- **`TREE`**: Zeigt die Verzeichnisstruktur eines Laufwerks oder Ordners an  
  ```bash
  TREE C:\
  ```

- **`TYPE`**: Zeigt den Inhalt von Textdateien an  
  ```bash
  TYPE C:\Ordner\datei.txt
  ```

- **`OPENFILES`**: Verwaltet geöffnete lokale oder Netzwerkdateien  
  ```bash
  OPENFILES /QUERY
  ```

- **`XCOPY`**: Kopiert Dateien und Verzeichnisbäume  
  ```bash
  XCOPY C:\Ordner D:\NeuerOrdner /E /I
  ```
## Anwendungen und Prozesse
Diese Befehle sind nützlich, um verschiedene Aufgaben im Windows-Betriebssystem auszuführen.

- **`SCHTASKS`**: Führt einen Befehl aus oder startet eine geplante Anwendung (Task Scheduler)  
  ```bash
  SCHTASKS /CREATE /TN "MeinTask" /TR "C:\Programme\MeinProgramm.exe" /SC DAILY /ST 10:00
  ```

- **`SHUTDOWN`**: Fährt den Computer herunter oder fährt ihn neu hoch  
  ```bash
  SHUTDOWN /S /T 0
  ```

- **`TASKLIST`**: Listet die laufenden Prozesse auf  ( *"/?"* für Infos)
  ```bash
  TASKLIST
  ```

- **`TASKKILL`**: Hält einen Task an oder stoppt ihn (zum Anhalten eines Tasks verwenden Sie eine PID, die Sie über die TASKLISTE herausfinden können)  
  ```bash
  TASKKILL /PID 1234 /F
  ```

- **`REG`**: Startet den Registry-Editor  
  ```bash
  REG EDIT
  ```

- **`RUNAS`**: Startet die Aufgabe als anderer Benutzer  
  ```bash
  RUNAS /USER:Benutzername "C:\Programme\MeinProgramm.exe"
  ``` 

## System-Informationen
Diese Befehle sind hilfreich, um verschiedene Informationen über das System zu erhalten und Konfigurationen anzupassen.

- **`DATE`**: Gibt das aktuelle Datum aus oder setzt es  
  ```bash
  DATE
  ```

- **`TIME`**: Zeigt die Systemzeit an oder stellt sie ein  
  ```bash
  TIME
  ```

- **`DRIVERQUERY`**: Zeigt den aktuellen Zustand und die Eigenschaften des Gerätetreibers an  
  ```bash
  DRIVERQUERY
  ```

- **`HOSTNAME`**: Zeigt den Namen des Computers an  
  ```bash
  HOSTNAME
  ```

- **`SYSTEMINFO`**: Zeigt Konfigurationsinformationen über Ihren Computer an  
  ```bash
  SYSTEMINFO
  ```

- **`VER`**: Anzeige der Windows-Version  
  ```bash
  VER
  ```

- **`GPRESULT`**: Zeigt die aktuell angewandten Gruppenrichtlinien an (RSoP)  
  ```bash
  GPRESULT /H report.html
  ```

- **`GPUPDATE`**: Aktualisiert Gruppenrichtlinien  
  ```bash
  GPUPDATE /force
  ```
## Festplatten Management
Diese Befehle sind nützlich, um Wartungsaufgaben an Festplatten und Partitionen durchzuführen sowie um Dateisysteme zu verwalten.

- **`CHKDISK`**: Prüft die Festplatte und zeigt Statistiken an  
  ```bash
  CHKDISK C:
  ```

- **`DEFRAG`**: Startet die Defragmentierung der Festplatte  
  ```bash
  DEFRAG C:
  ```

- **`CHKNTFS`**: Zeigt oder ändert die Ausführung der Festplattenprüfung beim Booten  
  ```bash
  CHKNTFS C:
  ```

- **`COMPACT`**: Zeigt die Komprimierung von Dateien in NTFS-Partitionen an und ändert sie  
  ```bash
  COMPACT /A
  ```

- **`CONVERT`**: Konvertiert FAT-Datenträger nach NTFS  
  ```bash
  CONVERT D: /FS:NTFS
  ```

- **`DISKPART`**: Zeigt die Eigenschaften von Festplattenpartitionen an und passt sie an  
  ```bash
  DISKPART
  ```

- **`DRIVERQUERY`**: Anzeige einer Liste der installierten Gerätetreiber
  ```bash
  driverquery
    ```

- **`FORMAT`**: Formatiert den Datenträger  
  ```bash
  FORMAT E: /FS:NTFS
  ```

- **`FSUTIL`**: Zeigt und konfiguriert Dateisystemeigenschaften  
  ```bash
  FSUTIL behavior query disablelastaccess
  ```

- **`LABEL`**: Erstellt, ändert oder löscht einen Datenträger-Label  
  ```bash
  LABEL D: MyDrive
  ```

- **`RECOVER`**: Stellt Daten von einem defekten oder beschädigten Datenträger wieder her  
  ```bash
  RECOVER E:
  ```
  
- **`sfc`**: Scannt Systemdateien nach Korruption und repariert wenn möglich
  ```bash
  sfc /scannow
    ```

- **`VOL`**: Zeigt die Datenträgerbezeichnung und die Seriennummer des Datenträgers an  
  ```bash
  VOL C:
  ```

## Netzwerk
Diese Befehle sind nützlich, um Netzwerkverbindungen zu überprüfen, Probleme zu diagnostizieren und Netzwerkeinstellungen zu verwalten.

- **`IPCONFIG`** (/all): Zeigt Informationen über Netzwerkschnittstellen an  
  ```bash
  IPCONFIG /all
  ```

- **`PING`**: Sendet ICMP-Anfragen an den Zielhost, prüft die Verfügbarkeit des Hosts  
  ```bash
  PING google.com
  ```

- **`TRACERT`**: Findet den Pfad für Pakete, die über das Netzwerk laufen  
  ```bash
  TRACERT google.com
  ```

- **`NSLOOKUP`**: Sucht IP-Adresse nach Ressourcenname  
  ```bash
  NSLOOKUP google.com
  ```

- **`ROUTE`**: Zeigt Netzwerkroutentabellen an  
  ```bash
  ROUTE PRINT
  ```

- **`ARP`**: Zeigt eine Tabelle mit IP-Adressen, die in physische Adressen umgewandelt wurden  
  ```bash
  ARP -A
  ```

- **`NETSH`**: Startet ein Programm zur Kontrolle der Netzwerkeinstellungen  
  ```bash
  NETSH INTERFACE SHOW INTERFACES
  ```

- **`GETMAC`**: Zeigt die MAC-Adresse des Netzwerkadapters an  
  ```bash
  GETMAC
  ```

- **`TFTP`**: Startet den TFTP-Client in der Konsole  
  ```bash
  TFTP
  ```

- **`NETSTAT`**: Anzeige aller aktuellen Verbindungen (-h für mehr Anzeige)  
  ```bash
  NETSTAT -a
  ```

## Befehlszeilen-Setup
Diese Befehle sind hilfreich, um die Benutzeroberfläche der Eingabeaufforderung anzupassen und verschiedene Funktionen der Kommandozeile zu nutzen.

- **`CLS`**: Löscht den Bildschirm  
  ```bash
  CLS
  ```

- **`CMD`**: Zeigt eine weitere Eingabeaufforderung an  
  ```bash
  CMD
  ```

- **`COLOR`**: Legt die Text- und Hintergrundfarbe fest  
  ```bash
  COLOR 0A
  ```

- **`PROMPT`**: Ändert die Eingabeaufforderung der Kommandozeile  
  ```bash
  PROMPT $P$G
  ```

- **`TITLE`**: Weist einen Titel für die aktuelle Sitzung zu  
  ```bash
  TITLE Meine Eingabeaufforderung
  ```

- **`HELP`**: Ruft die CMD-Hilfe auf  
  ```bash
  HELP
  ```

- **`EXIT`**: Beendet die Befehlszeile  
  ```bash
  EXIT
  ```

- **`SET`**: Zeigt Systemvariablen an  
  ```bash
  SET
  ```

## Sonstige Befehle


- **`FOR /L %i IN (1,1,254) DO ping -a -n 1 192.168.10.%i | FIND /i "Antwort">>d:\ipaddresses.txt`**: Überprüft alle IP-Adressen im lokalen Netzwerk von 192.168.10.1 bis 192.168.10.254 und speichert die Antworten in einer Datei. Bei englischen Windows-Installationen wird "Antwort" durch "Reply" ersetzt.  
  ```bash
  FOR /L %i IN (1,1,254) DO ping -a -n 1 192.168.10.%i | FIND /i "Antwort">>d:\ipaddresses.txt
  ```
**Erklärung**: Diese Befehlszeile wird verwendet, um aktiv nach Geräten im lokalen Netzwerk zu suchen und deren IP-Adressen zu dokumentieren.



