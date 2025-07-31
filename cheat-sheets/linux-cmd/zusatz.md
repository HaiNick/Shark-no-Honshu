# Zusatz

#### 1. **`ncdu`** – NCurses Disk Usage

* **Was es macht:** Interaktive Anzeige der Festplattennutzung in einer textbasierten Oberfläche.
* **Ausführung:** `ncdu [Pfad]`
* **Parameter:**
  * `-x` – Beschränkt die Analyse auf das aktuelle Dateisystem.
  * `-q` – Unterdrückt Warnungen.
  * `-r` – Nur Lesezugriff.
* **Beispiel:** `ncdu /home`([GitHub](https://github.com/sharkdp/fd?utm_source=chatgpt.com), [glances.readthedocs.io](https://glances.readthedocs.io/en/latest/glances.html?utm_source=chatgpt.com), [Akamai](https://www.linode.com/docs/guides/how-to-use-glances-system-monitoring/?utm_source=chatgpt.com))

***

#### 2. **`duff`** – Duplicate File Finder

* **Was es macht:** Findet doppelte Dateien basierend auf ihrem Inhalt.
* **Ausführung:** `duff [Optionen] [Pfad]`
* **Parameter:**
  * `-r` – Rekursive Suche.
  * `-e0` – Ausgabe im Null-Byte-getrennten Format.
* **Beispiel:** `duff -r /home/user`

***

#### 3. **`rg` (ripgrep)** – Schnelle Textsuche

* **Was es macht:** Durchsucht Dateien nach regulären Ausdrücken, schneller als `grep`.
* **Ausführung:** `rg [Optionen] [Muster]`
* **Parameter:**
  * `-i` – Groß-/Kleinschreibung ignorieren.
  * `-t` – Beschränkt die Suche auf bestimmte Dateitypen.
* **Beispiel:** `rg -i "Fehler" /var/log`

***

#### 4. **`mosh`** – Mobile Shell

* **Was es macht:** Stellt eine robuste SSH-Verbindung her, die bei Verbindungsunterbrechungen bestehen bleibt.
* **Ausführung:** `mosh [Benutzer@]Host`
* **Parameter:**
  * `--ssh="Befehl"` – Verwendet einen benutzerdefinierten SSH-Befehl.
* **Beispiel:** `mosh user@example.com`([dstack.ai](https://dstack.ai/docs/reference/cli/dstack/server/?utm_source=chatgpt.com))

***

#### 5. **`lshw`** – List Hardware

* **Was es macht:** Zeigt detaillierte Informationen zur Hardwarekonfiguration an.
* **Ausführung:** `sudo lshw [Optionen]`
* **Parameter:**
  * `-short` – Kurzfassung der Hardwareinformationen.
  * `-class` – Zeigt nur eine bestimmte Hardwareklasse an.
* **Beispiel:** `sudo lshw -short`([linux.die.net](https://linux.die.net/man/1/lshw?utm_source=chatgpt.com), [GeeksforGeeks](https://www.geeksforgeeks.org/lshw-command-in-linux-with-examples/?utm_source=chatgpt.com), [dstack.ai](https://dstack.ai/docs/reference/cli/dstack/volume/?utm_source=chatgpt.com))

***

#### 6. **`mtr`** – Netzwerkdiagnose

* **Was es macht:** Kombiniert `traceroute` und `ping` zur Analyse der Netzwerkverbindung.
* **Ausführung:** `mtr [Optionen] Ziel`
* **Parameter:**
  * `-r` – Berichtmodus.
  * `-c` – Anzahl der Pings.
* **Beispiel:** `mtr -r -c 10 example.com`([ClouDNS](https://www.cloudns.net/blog/linux-mtr-command/?utm_source=chatgpt.com))

***

#### 7. **`fd`** – Schnelle Dateisuche

* **Was es macht:** Schneller und benutzerfreundlicher Ersatz für `find`.
* **Ausführung:** `fd [Muster] [Pfad]`
* **Parameter:**
  * `-e` – Filtert nach Dateierweiterung.
  * `-t` – Filtert nach Dateityp.
* **Beispiel:** `fd -e txt "Notiz" /home/user`([GitHub](https://github.com/ogham/exa?utm_source=chatgpt.com), [GitHub](https://github.com/sharkdp/fd?utm_source=chatgpt.com), [dstack.ai](https://dstack.ai/docs/reference/cli/dstack/config/?utm_source=chatgpt.com))

***

#### 8. **`fzf`** – Fuzzy Finder

* **Was es macht:** Interaktive, unscharfe Suche in Listen (z. B. Dateien, Befehle).
* **Ausführung:** `fzf`
* **Parameter:**
  * `--preview` – Zeigt eine Vorschau der Auswahl an.
* **Beispiel:** `ls | fzf`([Baeldung](https://www.baeldung.com/linux/fzf-command?utm_source=chatgpt.com), [Red Hat](https://www.redhat.com/en/blog/fzf-linux-fuzzy-finder?utm_source=chatgpt.com))

***

#### 9. **`ranger`** – Konsolen-Dateimanager

* **Was es macht:** Textbasierter Dateimanager mit Vim-ähnlicher Navigation.
* **Ausführung:** `ranger`
* **Parameter:**
  * `--choosefile` – Gibt den ausgewählten Pfad zurück.
* **Beispiel:** `ranger --choosefile=/tmp/auswahl`([Medium](https://jponge.medium.com/using-exa-as-a-modern-replacement-to-the-venerable-unix-ls-command-c08837c2f839?utm_source=chatgpt.com))

***

#### 10. **`zoxide`** – Smarter `cd`-Ersatz

* **Was es macht:** Ermöglicht schnelles Wechseln zu häufig genutzten Verzeichnissen.
* **Ausführung:** `z [Verzeichnis]`
* **Parameter:**
  * `z` – Springt zum wahrscheinlichsten Verzeichnis.
* **Beispiel:** `z Projekte`

***

#### 11. **`exa`** – Moderner `ls`-Ersatz

* **Was es macht:** Listet Dateien mit erweiterten Informationen und Farben auf.
* **Ausführung:** `exa [Optionen] [Pfad]`
* **Parameter:**
  * `-l` – Detaillierte Listenansicht.
  * `--tree` – Zeigt Verzeichnisbaum an.
* **Beispiel:** `exa -l --tree /home/user`

***

#### 12. **`glances`** – Systemüberwachung

* **Was es macht:** Überwacht Systemressourcen in Echtzeit.
* **Ausführung:** `glances`
* **Parameter:**
  * `-w` – Startet Webserver-Modus.
  * `-c` – Verbindet sich mit einem entfernten Server.
* **Beispiel:** `glances -w`([Akamai](https://www.linode.com/docs/guides/how-to-use-glances-system-monitoring/?utm_source=chatgpt.com))

***

#### 13. **`iotop`** – I/O-Monitoring

* **Was es macht:** Zeigt laufende Prozesse mit hoher Festplatten-I/O-Nutzung an.
* **Ausführung:** `sudo iotop`
* **Parameter:**
  * `-o` – Zeigt nur Prozesse mit I/O-Aktivität.
  * `-a` – Zeigt kumulierte I/O-Nutzung.
* **Beispiel:** `sudo iotop -o`([LabEx](https://labex.io/tutorials/linux-linux-iotop-command-with-practical-examples-422740?utm_source=chatgpt.com), [GeeksforGeeks](https://www.geeksforgeeks.org/iotop-command-in-linux-with-examples/?utm_source=chatgpt.com), [Gist](https://gist.github.com/heroheman/aba73e47443340c35526755ef79647eb?utm_source=chatgpt.com))

***

#### 14. **`stat`** – Dateiinformationen

* **Was es macht:** Gibt detaillierte Informationen zu Dateien aus.
* **Ausführung:** `stat [Datei]`
* **Beispiel:** `stat /etc/passwd`([phoenixNAP | Global IT Services](https://phoenixnap.com/kb/linux-stat?utm_source=chatgpt.com))

***

#### 15. **`dstack`** – Remote-Stack-Management

* **Was es macht:** Verwaltet Remote-Stacks für maschinelles Lernen und Datenanalyse.
* **Ausführung:** `dstack [Befehl]`
* **Parameter:**
  * `config` – Konfiguriert die Verbindung.
  * `server` – Startet den Server.
* **Beispiel:** `dstack config --project meinprojekt`([dstack.ai](https://dstack.ai/docs/reference/cli/dstack/config/?utm_source=chatgpt.com), [dstack.ai](https://dstack.ai/docs/reference/cli/dstack/server/?utm_source=chatgpt.com))

***

#### 16. **`watch`** – Befehlsüberwachung

* **Was es macht:** Führt einen Befehl periodisch aus und zeigt die Ausgabe an.
* **Ausführung:** `watch [Optionen] Befehl`
* **Parameter:**
  * `-n` – Intervall in Sekunden.
  * `-d` – Hebt Änderungen hervor.
* **Beispiel:** `watch -n 5 df -h`

***

#### 17. **`progress`** – Fortschrittsanzeige

* **Was es macht:** Zeigt den Fortschritt laufender Kopier- oder Verschiebevorgänge an.
* **Ausführung:** `progress`
* **Parameter:**
  * `-M` – Zeigt nur die PID.
* **Beispiel:** `progress -M`

***

#### 18. **`dig`** – DNS Lookup

* **Was es macht:** Führt DNS-Abfragen durch (wie A, MX, TXT Records).
* **Ausführung:** `dig [Domain] [Typ]`
* **Parameter:**
  * `@dns-server` – Nutzt einen bestimmten DNS-Server.
  * `+short` – Kürzere Ausgabe.
* **Beispiel:** `dig @8.8.8.8 google.com +short`

***

#### 19. **`dog`** – Farbiger `dig`-Ersatz

* **Was es macht:** DNS-Tool wie `dig`, aber menschenlesbarer.
* **Ausführung:** `dog [Domain]`
* **Parameter:**
  * `--type A` – Abfragetyp.
* **Beispiel:** `dog --type MX example.com`

***

#### 20. **`tcpdump`** – Netzwerkpakete mitschneiden

* **Was es macht:** Zeichnet Netzwerkverkehr auf.
* **Ausführung:** `sudo tcpdump [Filter]`
* **Parameter:**
  * `-i` – Interface (z. B. `eth0`).
  * `-nn` – Keine Namensauflösung.
* **Beispiel:** `sudo tcpdump -i eth0 port 80`

***

#### 21. **`tshark`** – CLI von Wireshark

* **Was es macht:** Paketmitschnitt wie `tcpdump`, aber mit Wireshark-Parser.
* **Ausführung:** `sudo tshark [Optionen]`
* **Parameter:**
  * `-i eth0` – Interface.
  * `-Y "http"` – Wireshark-Filter.
* **Beispiel:** `sudo tshark -i eth0 -Y "http"`

***

#### 22. **`termshark`** – TUI für `tshark`

* **Was es macht:** GUI im Terminal für Netzwerkpaketanalyse.
* **Ausführung:** `termshark -i eth0`
* **Parameter:** übernimmt die `tshark`-Syntax.
* **Beispiel:** `termshark -i eth0`

***

#### 23. **`lsof`** – List Open Files

* **Was es macht:** Zeigt geöffnete Dateien/Ports an.
* **Ausführung:** `lsof [Optionen]`
* **Parameter:**
  * `-i` – Netzwerkverbindungen.
  * `-u user` – Nach Nutzer.
* **Beispiel:** `lsof -i :22`

***

#### 24. **`ipcalc`** – IP-Bereichsberechnung

* **Was es macht:** Berechnet Netzwerkeigenschaften.
* **Ausführung:** `ipcalc [IP/Subnetz]`
* **Beispiel:** `ipcalc 192.168.1.0/24`

***

#### 25. **`wormhole`** – Dateiübertragung

* **Was es macht:** Sendet Dateien sicher über das Netz.
* **Ausführung:** `wormhole send [Datei]`
* **Beispiel:** `wormhole send dokument.txt`

***

#### 26. **`systemd-analyze blame`**

* **Was es macht:** Zeigt Startdauer von Systemdiensten.
* **Ausführung:** `systemd-analyze blame`

***

#### 27. **`systemd-analyze critical-chain`**

* **Was es macht:** Zeigt kritische Startreihenfolge.
* **Ausführung:** `systemd-analyze critical-chain`

***

#### 28. **`ps`** – Prozessstatus

* **Was es macht:** Listet laufende Prozesse.
* **Ausführung:** `ps aux`
* **Parameter:**
  * `-ef` – Volle Ausgabe mit Elternprozess.
* **Beispiel:** `ps -ef | grep ssh`

***

#### 29. **`procs`** – Moderner `ps`-Ersatz

* **Was es macht:** Farbig und interaktiv, besser lesbar.
* **Ausführung:** `procs`

***

#### 30. **`lazydocker`** – Docker TUI

* **Was es macht:** Terminal UI für Docker-Management.
* **Ausführung:** `lazydocker`

***

#### 31. **`rsync`** – Datei-Synchronisation

* **Was es macht:** Kopiert/vergleicht Verzeichnisse.
* **Ausführung:** `rsync [Optionen] Quelle Ziel`
* **Parameter:**
  * `-a` – Archivmodus.
  * `-v` – Verbose.
* **Beispiel:** `rsync -av /src /ziel`

***

#### 32. **`rm`** – Dateien löschen

* **Was es macht:** Entfernt Dateien oder Verzeichnisse.
* **Ausführung:** `rm [Optionen] Datei`
* **Parameter:**
  * `-r` – Rekursiv.
  * `-f` – Erzwingen.
* **Beispiel:** `rm -rf /tmp/test`

***

#### 33. **`shred`** – Sichere Datei-Löschung

* **Was es macht:** Überschreibt Datei mehrfach.
* **Ausführung:** `shred -u [Datei]`
* **Beispiel:** `shred -u geheim.txt`

***

#### 34. **`moreutils`** – Zusatztoolsammlung

* **Enthält z. B.:**
  * `ts` – Zeitstempel Zeilen.
  * `vidir` – Dateien per Editor umbenennen.
  * `errno` – Fehlermeldung anzeigen.
  * `ifdata` – Interface-Daten anzeigen.

***

#### 35. **`ts`** – Zeitstempel

* **Was es macht:** Fügt Zeitstempel in Pipelines ein.
* **Ausführung:** `echo "Text" | ts`

***

#### 36. **`errno`** – Fehlernummer auflösen

* **Was es macht:** Zeigt Bedeutung von Fehlernummern.
* **Ausführung:** `errno 2`

***

#### 37. **`ifdata`** – Netzwerkinterface-Infos

* **Was es macht:** Gibt IP, Status etc. aus.
* **Ausführung:** `ifdata eth0`

***

#### 38. **`vidir`** – Dateinamen im Editor ändern

* **Was es macht:** Liste der Dateinamen im Editor ändern.
* **Ausführung:** `vidir [Verzeichnis]`

***

#### 39. **`vip`** – Datei-Inhalt in Pipe bearbeiten

* **Was es macht:** Bearbeitet gepipte Daten in `$EDITOR`.
* **Beispiel:** `cat datei.txt | vip`

***

#### 40. **`unp`** – Archiv-Entpacker

* **Was es macht:** Entpackt viele Archivformate automatisch.
* **Ausführung:** `unp archiv.zip`

***

#### 41. **`jq`** – JSON-Parser

* **Was es macht:** Formatiert und bearbeitet JSON.
* **Ausführung:** `jq [Filter]`
* **Beispiel:** `cat datei.json | jq '.user.name'`

***

#### 42. **`taskwarrior`** – Aufgabenverwaltung

* **Was es macht:** CLI-basierte To-Do-Verwaltung.
* **Ausführung:** `task [Befehl]`
* **Beispiel:** `task add "Backup erstellen"`

***

#### 43. **`asciinema` (`asc`)** – Terminal-Recording

* **Was es macht:** Nimmt Terminal-Sitzungen auf.
* **Ausführung:** `asciinema rec`
* **Beispiel:** `asciinema rec session.cast`

***

#### 44. **`fabric`** – SSH-Automatisierung

* **Was es macht:** Führt Remote-Befehle über SSH aus.
* **Ausführung:** `fab [task]`
* **Beispiel:** `fab deploy`

***

#### 45. **`ollama`** – LLM-Ausführung lokal

* **Was es macht:** Startet und verwaltet lokale Sprachmodelle.
* **Ausführung:** `ollama run llama2`
* **Beispiel:** `ollama run mistral`
