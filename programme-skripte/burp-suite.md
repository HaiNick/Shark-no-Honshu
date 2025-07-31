---
description: https://tryhackme.com/room/burpsuitebasics
cover: >-
  https://uploads-ssl.webflow.com/62efedb360a7998b0e43cb84/6321a0f076706854ff591093_All%20about%20BurpSuite.jpg
coverY: 0
---

# Burp Suite

## Beschreibung

Ein auf Java-basierendes Framework zum Penetrationstest von Web/mobile - Anwendungen.

Es gibt 3 Editionen:

* Burp Suite **Community** (kostenlos)
* Burp Suite **Professional** (erweiterte Funktionalität:autom. Vuln. Scanner, kein Limit, API zur Integration weiterer Tools, Speicherfunktion, Zugriff auf Burp Suite Collaborator)
* Burp Suite **Enterprise** (kontinuierlicher Scanner)

## Grundfunktionen (built-in)

Zusätzlich können Erweiterungen (**kostenlos**/**-pflichtig**) hinzugefügt werden durch:

* **Burp Suite Extender-Modul** zum einfachen Laden von Erweiterungen geschrieben in Java, Python (mit Java Jython Interpreter) oder Ruby (mit Java JRuby Interpreter).
* **Markplatz BApp Store** zum Herunterladen von Modulen Dritter

### Proxy

Ermöglicht das Abfangen und Ändern von Anfragen und Antworten bei der Interaktion mit Webanwendungen.

### Repeater ( [https://tryhackme.com/room/burpsuiterepeater](https://tryhackme.com/room/burpsuiterepeater) )

Ermöglicht es, dieselbe Anfrage mehrmals zu erfassen, zu ändern und erneut zu senden. Diese Funktion ist besonders nützlich bei der Erstellung von Nutzdaten durch Ausprobieren (z. B. bei SQLi - Structured Query Language Injection) oder beim Testen der Funktionalität eines Endpunkts auf Schwachstellen.

### Intruder ( [https://tryhackme.com/room/burpsuiteintruder](https://tryhackme.com/room/burpsuiteintruder) )

Trotz der Ratenbeschränkungen in Burp Suite Community ermöglicht Intruder das Besprühen von Endpunkten mit Anfragen. Es wird üblicherweise für Brute-Force-Angriffe oder Fuzzing von Endpunkten verwendet.

### Decoder

Decoder bietet einen wertvollen Dienst für die Datentransformation. Er kann erbeutete Informationen dekodieren oder Nutzdaten verschlüsseln, bevor sie an das Ziel gesendet werden. Es gibt zwar alternative Dienste für diesen Zweck, aber die Nutzung von Decoder innerhalb der Burp Suite kann sehr effizient sein.

### Comparer

Wie der Name schon sagt, ermöglicht der Comparer den Vergleich zweier Daten auf Wort- oder Byte-Ebene. Die Möglichkeit, potenziell große Datensegmente mit einer einzigen Tastenkombination direkt an ein Vergleichstool zu senden, ist zwar nicht exklusiv für Burp Suite, beschleunigt den Prozess aber erheblich.

### Sequencer

Der Sequencer wird typischerweise eingesetzt, um die Zufälligkeit von Token wie Session-Cookie-Werten oder anderen vermeintlich zufällig generierten Daten zu bewerten. Wenn der Algorithmus, der für die Generierung dieser Werte verwendet wird, keine sichere Zufälligkeit aufweist, kann er Anhaltspunkte für verheerende Angriffe liefern.

## Shortcuts

<table><thead><tr><th width="310">Shortcut</th><th>Tab</th></tr></thead><tbody><tr><td><code>CTRL + Shift + D</code></td><td>Dashboard</td></tr><tr><td><code>CTRL + Shift + T</code></td><td>Target Tab</td></tr><tr><td><code>CTRL + Shift + P</code></td><td>Proxy Tab</td></tr><tr><td><code>CTRL + Shift + I</code></td><td>Intruder Tab</td></tr><tr><td><code>CTRL + Shift + R</code></td><td>Repeater Tab</td></tr></tbody></table>

## Erweiterungen entwickeln

[https://portswigger.net/burp/extender/writing-your-first-burp-suite-extension](https://portswigger.net/burp/extender/writing-your-first-burp-suite-extension)

## Tipps

* NTLM mit Burp : [https://san3ncrypt3d.com/2021/10/26/burp/](https://san3ncrypt3d.com/2021/10/26/burp/) + Name/Passwort unter Network>Connections>"Platform authentication"

