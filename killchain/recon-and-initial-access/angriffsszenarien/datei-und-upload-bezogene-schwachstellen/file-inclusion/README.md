---
description: >-
  Die Schwachstelle der File Inclusion ermöglicht eine ungewollte Einbindung von
  Dateien.
---

# File Inclusion

<details>

<summary>Links:</summary>

[https://github.com/payloadbox/rfi-lfi-payload-list](https://github.com/payloadbox/rfi-lfi-payload-list)

[https://www.webmaster-tipps.de/php-sicherheit-local-file-inclusion-und-remote-file-inclusion/](https://www.webmaster-tipps.de/php-sicherheit-local-file-inclusion-und-remote-file-inclusion/)

[https://www.hackingarticles.in/apache-log-poisoning-through-lfi/](https://www.hackingarticles.in/apache-log-poisoning-through-lfi/)

</details>

Dabei wird unterschieden ob lokale Dateien eingebunden werden können (LFI) oder sogar Dateien externer Maschinen.



### Interessante Dateien

<table><thead><tr><th width="267">Pfad/Datei</th><th>Beschreibung</th></tr></thead><tbody><tr><td><code>/etc/issue</code></td><td>Enthält eine Nachricht oder Systemidentifikation, die vor der Anmeldeaufforderung angezeigt wird.</td></tr><tr><td><code>/etc/profile</code></td><td>Steuert systemweite Standardvariablen wie Export-Variablen, Dateierstellungsmaske (umask), Terminaltypen und Benachrichtigungen über neue E-Mails.</td></tr><tr><td><code>/proc/version</code></td><td>Gibt die Version des Linux-Kernels an.</td></tr><tr><td><code>/etc/passwd</code></td><td>Enthält alle registrierten Benutzer, die Zugriff auf das System haben.</td></tr><tr><td><code>/etc/shadow</code></td><td>Enthält Informationen über die Passwörter der Systembenutzer.</td></tr><tr><td><code>/root/.bash_history</code></td><td>Beinhaltet die Befehls-Historie des Root-Benutzers.</td></tr><tr><td><code>/var/log/dmesg</code></td><td>Enthält globale Systemmeldungen, einschließlich jener, die beim Systemstart protokolliert werden. <em>(Hinweis: Dateiname korrigiert von <code>/var/log/dmessage</code> zu <code>/var/log/dmesg</code>)</em></td></tr><tr><td><code>/var/mail/root</code></td><td>Enthält alle E-Mails für den Root-Benutzer.</td></tr><tr><td><code>/root/.ssh/id_rsa</code></td><td>Private SSH-Schlüssel für den Root-Benutzer oder andere bekannte gültige Benutzer des Servers.</td></tr><tr><td><code>/var/log/apache2/access.log</code></td><td>Protokolliert alle Zugriffsanfragen an den Apache-Webserver.</td></tr><tr><td><code>C:\boot.ini</code></td><td>Enthält die Startoptionen für Computer mit BIOS-Firmware.</td></tr></tbody></table>
