# PHP Wrappers und LFI/RFI-Bypass-Techniken

**PHP Wrappers** sind spezielle Schnittstellen in PHP, die den Zugriff auf verschiedene Datenströme ermöglichen. Sie bieten mächtige Funktionen, bergen jedoch erhebliche Sicherheitsrisiken, wenn sie in Verbindung mit unzureichend validierten Benutzereingaben verwendet werden – insbesondere bei **Local File Inclusion (LFI)**- oder **Remote File Inclusion (RFI)**-Schwachstellen.

***

#### 1. PHP Filter Wrapper (`php://filter`)

Der `php://filter`-Wrapper erlaubt es, Dateiinhalte zu manipulieren, bevor sie von PHP gelesen werden. Besonders häufig wird der Filter `convert.base64-encode` verwendet, um sensible Dateien wie `/etc/passwd` zu Base64-kodieren und deren Inhalt auszugeben:

**Beispiel:**

```
php://filter/convert.base64-encode/resource=/etc/passwd
```

Durch Dekodieren der Rückgabe kann der Angreifer den ursprünglichen Dateiinhalt einsehen. Weitere Filter sind u. a.:

* **String-Filter**: `string.rot13`, `string.toupper`, `string.tolower`, `string.strip_tags`
* **Konvertierungs-Filter**: `convert.base64-encode`, `convert.quoted-printable-decode`
* **Kompressions-Filter**: `zlib.deflate`, `zlib.inflate`
* **Verschlüsselungs-Filter** _(veraltet)_: `mcrypt`, `mdecrypt`

***

#### 2. Data Wrapper (`data://`)

Der `data://`-Wrapper erlaubt das direkte Einbetten von Daten in URLs, z. B. zur Ausführung eingebetteten PHP-Codes bei entsprechend unsicher konfigurierten Anwendungen.

**Beispiel:**

```
data:text/plain,<?php phpinfo(); ?>
```

Dies kann zur **Remote Code Execution (RCE)** führen, wenn die Eingabe nicht sicher verarbeitet wird.

***

#### 3. Base Directory Bypass

Manche Anwendungen implementieren Schutzmechanismen, die Dateizugriffe nur innerhalb eines Basisverzeichnisses wie `/var/www/html` erlauben. Angreifer können dies umgehen, indem sie alternative Schreibweisen verwenden, etwa:

**Beispiel:**

```
/var/www/html/..//..//..//etc/passwd
```

Da `..//` vom Dateisystem wie `../` interpretiert wird, aber vom einfachen Stringvergleich nicht erkannt wird, lässt sich so der Filter umgehen.

***

#### 4. Obfuskationstechniken zur Filterumgehung

Angreifer verwenden verschiedene Kodierungen und Schreibweisen, um grundlegende Filtermechanismen zu umgehen:

* **Standard-URL-Encoding**: `../` → `%2e%2e%2f`
* **Doppelte Kodierung**: `../` → `%252e%252e%252f` (wenn zweimal dekodiert wird)
* **Obfuskation**: z. B. `....//config.php` anstelle von `../config.php`

**Beispielcode mit unsicherem Filter:**

```php
$file = str_replace('../', '', $_GET['file']);
include('files/' . $file);
```

→ Lässt sich mit `%2e%2e%2fconfig.php` oder `....//config.php` umgehen.
