# Yara

<details>

<summary>Links:</summary>

[https://virustotal.github.io/yara/](https://virustotal.github.io/yara/)

[https://github.com/virustotal/yara](https://github.com/virustotal/yara)

</details>

**YARA** ist ein Werkzeug zur Erkennung und Klassifizierung von Dateien anhand definierter Muster. Es wird vorrangig in der Malware-Analyse eingesetzt, eignet sich aber auch für andere Anwendungsfälle wie Dateiscreening oder Bedrohungssuche.

#### Grundkonzept

YARA verwendet sogenannte **Rules**, um Dateien zu identifizieren. Eine Rule besteht aus:

* **Metadaten** (optional)
* **String-Definitionen** (Text, Hex, Regex)
* **Bedingung** (boolean expression), wann die Rule matcht

#### Syntaxbeispiel

```yara
rule BeispielRegel
{
    strings:
        $a = "malware"           // Text
        $b = { 6A 40 68 00 30 }  // Hex-Pattern
        $c = /Trojan:[a-z]+/     // Regex

    condition:
        $a or $b or $c
}
```

#### Nutzung

```bash
yara regel.yar datei.exe
```

#### Anwendungsbereiche

* Malware-Klassifikation
* Threat Hunting
* Dateityp-Erkennung
* Incident Response
