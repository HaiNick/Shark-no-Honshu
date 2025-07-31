---
description: Schritte von Beginn bis Ende eines Cyberangriffs
icon: skull
---

# Killchain

<details>

<summary>Links:</summary>

[https://attack.mitre.org/](https://attack.mitre.org/)

[https://www.thehacker.recipes/](https://www.thehacker.recipes/)

[https://book.hacktricks.wiki/en/index.html](https://book.hacktricks.wiki/en/index.html)

[https://github.com/infosecn1nja/AD-Attack-Defense](https://github.com/infosecn1nja/AD-Attack-Defense)

</details>

Nach Wikipedia besteht eine Killchain u.a. aus nachfolgenden Schritten. Diese können sich je nach Killchain unterscheiden und mehr aufgeschlüsselt sein oder kompakt wie hier.

### Phase 1: Erkundung <a href="#phase_1-_erkundung" id="phase_1-_erkundung"></a>

Die Erkundungsphase umfasst das Sammeln von Informationen über das Ziel, sei es eine Einzelperson oder eine Organisation. Dieser Prozess wird in Zielidentifizierung, -auswahl und -profilierung unterteilt. Dabei kommen diverse Quellen und Methoden zum Einsatz, darunter Internetseiten, Konferenzen, Blogs, soziale Netzwerke, Mailinglisten und Netzwerkverfolgungstools. Es wird zwischen aktiver und passiver Erkundung unterschieden: Während die aktive Erkundung potenziell beim Ziel Alarm auslösen kann, bleibt die passive Erkundung unbemerkt. Das Ziel dieser Phase ist es, eine fundierte Grundlage zur Auswahl geeigneter Angriffsstrategien zu schaffen.[<sup>\[1\]</sup>](https://de.wikipedia.org/wiki/Cyber_Kill_Chain#cite_note-:0-1)

### Phase 2: Bewaffnung <a href="#phase_2-_bewaffnung" id="phase_2-_bewaffnung"></a>

In der Bewaffnungsphase werden auf Grundlage der in der Erkundungsphase gewonnenen Informationen eine Hintertür und ein Penetrationsplan entwickelt. Hierbei kommen verschiedene Tools wie [Remote-Access-Trojaner (RAT)](https://de.wikipedia.org/wiki/Fernwartungssoftware) zum Einsatz, darunter beispielsweise Carberp, Ventir Trojan oder Poison Ivy.[<sup>\[1\]</sup>](https://de.wikipedia.org/wiki/Cyber_Kill_Chain#cite_note-:0-1)

### Phase 3: Zustellung <a href="#phase_3-_zustellung" id="phase_3-_zustellung"></a>

In der Zustellungsphase wird die entwickelte [Malware](https://de.wikipedia.org/wiki/Schadprogramm)-Nutzlast ausgeführt, nachdem eine geeignete Hintertür identifiziert wurde. Die Zustellung erfolgt entweder durch das gezielte Lenken oder Zwingen des Ziels zur Interaktion mit dem Malware-Exploit oder durch automatische Ausnutzung von Schwachstellen in Protokollen oder Software. Genutzte Methoden sind beispielsweise [Phishing](https://de.wikipedia.org/wiki/Phishing)-Angriffe, bei denen E-Mails verwendet werden, um das Ziel dazu zu bringen, einen schädlichen Anhang herunterzuladen.[<sup>\[2\]</sup>](https://de.wikipedia.org/wiki/Cyber_Kill_Chain#cite_note-:1-2)

### Phase 4: Ausnutzung <a href="#phase_4-_ausbeutung" id="phase_4-_ausbeutung"></a>

In dieser Phase wird die auf das Zielsystem übertragene Malware vorbereitet, um deren Installation einzuleiten. Dabei findet noch keine eigentliche Installation statt, sondern die Umgebung wird für die anschließende Installationsphase der Kill Chain optimiert. Diese Phase ist eng mit der Installationsphase verknüpft, da in der Ausnutzungsphase alle technischen Voraussetzungen für die Installation geschaffen werden müssen. Um die Nutzlast erfolgreich zu installieren oder auszuführen, werden Schwachstellen in der Hard- oder Software ausgenutzt, die als [Common Vulnerabilities and Exposures (CVE)](https://de.wikipedia.org/wiki/Common_Vulnerabilities_and_Exposures) bezeichnet werden. Informationen zu diesen Schwachstellen werden in einer öffentlich zugänglichen und regelmäßig aktualisierten Datenbank bereitgestellt. [<sup>\[2\]</sup>](https://de.wikipedia.org/wiki/Cyber_Kill_Chain#cite_note-:1-2)

### Phase 5: Installation <a href="#phase_5-_installation" id="phase_5-_installation"></a>

In der Installationsphase wird die [Malware](https://de.wikipedia.org/wiki/Schadprogramm), falls erforderlich, auf dem Zielsystem installiert, wodurch die eigentliche Infektion des Computers beginnt. Dabei nutzt die Malware privilegierte Zugriffsrechte, um die Dateizugriffsmechanismen des Betriebssystems zu verändern. Zusätzlich verändert sie das Erscheinungsbild ihrer Dateien, indem sie das [Dateiformat](https://de.wikipedia.org/wiki/Dateiformat) ändert oder die Dateien vor dem Benutzer verbirgt.

Fortschrittliche Malware, wie polymorphe oder metamorphe Malware, passt ihre Speicherabdrücke an, um Erkennung durch Sandbox-Algorithmen oder verhaltensbasierte [Antivirenprogramme](https://de.wikipedia.org/wiki/Antivirenprogramm) zu umgehen. Polymorphe Malware verändert dabei den Datei-Header, während die Nutzlast gleich bleibt, während metamorphe Malware auch die Struktur der Nutzlast selbst verändert.

Die Installationsphase dient nicht nur dazu, eine Hintertür auf dem Zielsystem zu etablieren, sondern stellt auch sicher, dass die Angreifer eine Verbindung zum infizierten System aufrechterhalten können. Diese Phase unterscheidet sich von der Ausnutzungsphase, die lediglich sicherstellt, dass das Schadpaket für die Installation bereit ist und alle Voraussetzungen erfüllt sind. In der Installationsphase beginnt die eigentliche lokale Verankerung der Schadsoftware im Zielsystem. Die Kommunikation mit dem Command-and-Control-System wird jedoch erst in einer späteren Phase initiiert.[<sup>\[2\]</sup>](https://de.wikipedia.org/wiki/Cyber_Kill_Chain#cite_note-:1-2)

### Phase 6: Befehl und Kontrolle <a href="#phase_6-_befehl_und_kontrolle" id="phase_6-_befehl_und_kontrolle"></a>

Die Befehls- und Kontrollphase ( [c2-command-and-control.md](../red-team/c2-command-and-control.md "mention") ) ermöglicht Angreifern, Anweisungen an die Malware auf dem Zielsystem zu senden und deren Aktivitäten zu steuern. Zu den Zielen dieser Phase gehören der Diebstahl sensibler Informationen wie Passwörter, Finanzdaten oder [geistiges Eigentum](https://de.wikipedia.org/wiki/Geistiges_Eigentum) durch Tools wie [Keylogger](https://de.wikipedia.org/wiki/Keylogger), Zeus oder Trojaner. Darüber hinaus kann die Malware angewiesen werden, sich auf andere Teile des Netzwerks zu verbreiten, weitere Schadsoftware auszuführen oder Verschlüsselungsmechanismen für Ransomware-Angriffe zu aktivieren.[<sup>\[2\]</sup>](https://de.wikipedia.org/wiki/Cyber_Kill_Chain#cite_note-:1-2)

### Phase 7: Erfüllung der gesetzten Zielee <a href="#phase_7-_handeln_nach_zielsetzung" id="phase_7-_handeln_nach_zielsetzung"></a>

In der Abschlussphase eines Cyberangriffs werden die angestrebten Ziele des Angriffs erreicht. Wurde Malware erfolgreich auf einem System installiert, beginnt sie, ihre programmierte Funktion entweder eigenständig oder durch Anweisungen vom Command-and-Control-Server auszuführen. Diese Phase wird auch als Detonationsphase bezeichnet und markiert den erfolgreichen Abschluss der Cyber-Kill-Chain.[<sup>\[2\]</sup>](https://de.wikipedia.org/wiki/Cyber_Kill_Chain#cite_note-:1-2)

Zu den zentralen Aktionen in dieser Phase zählen:

* Datenexfiltration: Der Diebstahl sensibler oder geistiger Daten aus dem Netzwerk.
* Ransomware-Angriffe: Die Verschlüsselung von Daten, die Änderung von Anmeldeinformationen oder das Blockieren von Netzwerkressourcen, um Lösegeld zu erpressen.
* Cyber-Terrorismus: Die gezielte Zerstörung durch Löschen oder vollständige Beschädigung von Dateien, um maximalen Schaden anzurichten.[<sup>\[2\]</sup>](https://de.wikipedia.org/wiki/Cyber_Kill_Chain#cite_note-:1-2)
