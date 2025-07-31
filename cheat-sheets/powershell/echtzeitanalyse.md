# Echtzeitanalyse

## (htop) Get-Process

**Beschreibung:** Anzeige aller laufenden Prozesse



## Get-Service

**Beschreibung:** Statusanzeige von Services auf der Maschine



## Get-NetTCPConnection

**Beschreibung:** Anzeige aktiver Netzwerkverbindungen (TCP)



## Get-FileHash

**Beschreibung:** Berechnet den kryptografischen Hashwert einer Datei (bzw. mehrerer Dateien).

**Parameterübersicht:**

* **`-Path <String[]>`**\
  Gibt den Pfad zu der Datei oder den Dateien an, für die der Hashwert berechnet werden soll.
* **`-Algorithm <HashAlgorithm>`**\
  Bestimmt den zu verwendenden Hashalgorithmus. Standardmäßig wird `SHA256` verwendet. Zulässige Werte sind:\
  `SHA1`, `SHA256`, `SHA384`, `SHA512`, `MD5`.
* **`-LiteralPath <String[]>`**\
  Wie `-Path`, behandelt jedoch den eingegebenen Pfad als wörtliche Zeichenfolge. Platzhalterzeichen (z. B. `*`, `?`) werden nicht aufgelöst.
* **`-InformationAction <ActionPreference>`**\
  Gibt an, wie mit Informationsmeldungen umgegangen wird. Mögliche Werte: `SilentlyContinue`, `Continue`, `Ignore`, `Inquire`, `Stop`, `Suspend`.
* **`-InformationVariable <String>`**\
  Gibt eine Variable an, in der alle während der Ausführung erzeugten Informationsmeldungen gespeichert werden.
* **`-WhatIf`**\
  Zeigt an, was passieren würde, wenn der Befehl ausgeführt würde, ohne ihn tatsächlich auszuführen. [https://techcommunity.microsoft.com/blog/itopstalkblog/powershell-basics-dont-fear-hitting-enter-with--whatif/353579](https://techcommunity.microsoft.com/blog/itopstalkblog/powershell-basics-dont-fear-hitting-enter-with--whatif/353579)
* **`-Confirm`**\
  Fordert eine Bestätigung an, bevor der Befehl ausgeführt wird.

