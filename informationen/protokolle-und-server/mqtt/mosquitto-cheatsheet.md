# Mosquitto Cheatsheet

### 1. mosquitto\_sub — MQTT Nachrichten abonnieren

#### Grundsyntax

```bash
mosquitto_sub -h <broker> -t <topic> [Optionen]
```

#### Wichtige Optionen

| Option             | Bedeutung                                     | Beispiel                 |
| ------------------ | --------------------------------------------- | ------------------------ |
| `-h <host>`        | MQTT-Broker-Adresse (Standard: localhost)     | `-h test.mosquitto.org`  |
| `-p <port>`        | Port des Brokers (Standard: 1883)             | `-p 1883`                |
| `-t <topic>`       | Thema (Topic), das abonniert werden soll      | `-t sensors/temperature` |
| `-q <QoS>`         | QoS-Level (0, 1 oder 2)                       | `-q 1`                   |
| `-v`               | Ausgabe mit Topic-Name vor Nachricht          | `-v`                     |
| `-d`               | Debug-Ausgabe                                 | `-d`                     |
| `-u <username>`    | Benutzername für Authentifizierung            | `-u user`                |
| `-P <password>`    | Passwort für Authentifizierung                | `-P pass`                |
| `--cafile <datei>` | CA-Zertifikat für TLS-Verbindung              | `--cafile ca.crt`        |
| `-k <keepalive>`   | Keepalive-Intervall in Sekunden (Standard 60) | `-k 120`                 |
| `--insecure`       | TLS-Verbindung ohne Zertifikatprüfung         | `--insecure`             |

#### Beispiel

```bash
mosquitto_sub -h broker.example.com -t sensors/temperature -q 1 -v
```

***

### 2. mosquitto\_pub — MQTT Nachrichten senden (publizieren)

#### Grundsyntax

```bash
mosquitto_pub -h <broker> -t <topic> -m <message> [Optionen]
```

#### Wichtige Optionen

| Option             | Bedeutung                                     | Beispiel                 |
| ------------------ | --------------------------------------------- | ------------------------ |
| `-h <host>`        | MQTT-Broker-Adresse (Standard: localhost)     | `-h test.mosquitto.org`  |
| `-p <port>`        | Port des Brokers (Standard: 1883)             | `-p 1883`                |
| `-t <topic>`       | Thema (Topic), auf dem veröffentlicht wird    | `-t sensors/temperature` |
| `-m <message>`     | Nachricht (Payload)                           | `-m "23.4"`              |
| `-q <QoS>`         | QoS-Level (0, 1 oder 2)                       | `-q 1`                   |
| `-r`               | Retain-Flag setzen (Nachricht wird behalten)  | `-r`                     |
| `-u <username>`    | Benutzername für Authentifizierung            | `-u user`                |
| `-P <password>`    | Passwort für Authentifizierung                | `-P pass`                |
| `--cafile <datei>` | CA-Zertifikat für TLS-Verbindung              | `--cafile ca.crt`        |
| `-k <keepalive>`   | Keepalive-Intervall in Sekunden (Standard 60) | `-k 120`                 |
| `--insecure`       | TLS-Verbindung ohne Zertifikatprüfung         | `--insecure`             |

#### Beispiel

```bash
mosquitto_pub -h broker.example.com -t sensors/temperature -m "23.4" -q 1
```

***

### 3. Nützliche Tipps

*   **Themen mit Wildcards abonnieren:**

    ```bash
    mosquitto_sub -t 'sensors/#'
    ```
*   **Veröffentlichen von Nachrichten aus Datei:**

    ```bash
    mosquitto_pub -t sensors/temperature -f /path/to/file.txt
    ```
*   **Fortlaufende Nachrichten publizieren (z.B. in Bash-Schleife):**

    ```bash
    while true; do mosquitto_pub -t sensors/temp -m "$(date +%s)"; sleep 5; done
    ```
*   **TLS-Verbindung mit Client-Zertifikat:**

    ```bash
    mosquitto_sub -h broker.example.com --cafile ca.crt --cert client.crt --key client.key -t sensors/temp
    ```
*   **Debug-Modus zum Testen:**

    ```bash
    mosquitto_sub -h broker.example.com -t test -d
    mosquitto_pub -h broker.example.com -t test -m "hello" -d
    ```
