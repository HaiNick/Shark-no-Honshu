# Manipulation von Strings

### 1. **Grundlegende Extraktion**

| Tool      | Befehl                   | Beschreibung                            |
| --------- | ------------------------ | --------------------------------------- |
| `grep`    | `grep 'pattern' file`    | Zeilen mit "pattern" anzeigen           |
| `grep -o` | `grep -o 'pattern' file` | Nur den Treffer ausgeben                |
| `cut`     | `cut -d':' -f2`          | Nach Trennzeichen (z. B. `:`) aufteilen |
| `awk`     | `awk '{print $2}'`       | Spalte extrahieren                      |
| `sed`     | `sed 's/abc/xyz/'`       | Text ersetzen (einmal pro Zeile)        |
| `sed`     | `sed 's/.*: //'`         | Alles bis zum letzten `:` entfernen     |

***

### 2. **Fortgeschrittene Extraktion**

| Tool         | Befehl                                       | Beschreibung                      |
| ------------ | -------------------------------------------- | --------------------------------- |
| `awk -F ':'` | `awk -F ':' '{print $2}'`                    | Benutzerdefiniertes Trennzeichen  |
| `grep -P`    | `grep -Po '(?<=Name: ).*'`                   | Perl-regex Lookbehind             |
| `sed -n`     | `sed -n 's/^Name: //p'`                      | Nur Zeilen mit „Name:“ bearbeiten |
| `perl`       | `perl -ne 'print "$1\n" if /Name:\s*(\S+)/'` | Regex-Match mit Perl              |

***

### 3. **Strings kürzen oder trennen**

| Tool                             | Befehl                      | Beschreibung |
| -------------------------------- | --------------------------- | ------------ |
| `cut -c1-5`                      | Gibt die ersten 5 Zeichen   |              |
| `cut -d' ' -f1`                  | Erstes Wort in einer Zeile  |              |
| `awk '{print substr($0, 1, 5)}'` | Zeichenpositionen mit `awk` |              |
| `rev`                            | Dreht Zeichenfolge um       |              |
| `head -c N`                      | Gibt die ersten N Zeichen   |              |
| `tail -c N`                      | Gibt die letzten N Zeichen  |              |

***

### 4. **Ersetzen & Umwandeln**

| Tool                               | Befehl                                 | Beschreibung |
| ---------------------------------- | -------------------------------------- | ------------ |
| `tr 'a-z' 'A-Z'`                   | Kleinschreibung → Großschreibung       |              |
| `tr -d '\r'`                       | Entferne Steuerzeichen                 |              |
| `sed 's/foo/bar/g'`                | Alle „foo“ durch „bar“ ersetzen        |              |
| `awk '{gsub("foo","bar"); print}'` | Global ersetzen in `awk`               |              |
| `perl -pe 's/foo/bar/g'`           | Sehr flexibel mit regulären Ausdrücken |              |

***

### 5. **Filtern & Bedingte Verarbeitung**

| Tool                               | Befehl                           | Beschreibung |
| ---------------------------------- | -------------------------------- | ------------ |
| `grep '^Name:'`                    | Zeilen, die mit "Name:" beginnen |              |
| `awk '/admin/ {print $2}'`         | Nur Zeilen mit "admin"           |              |
| `awk '$3 == "enabled" {print $1}'` | Bedingte Ausgabe                 |              |
| `sed -n '/error/ p'`               | Nur Zeilen mit "error" anzeigen  |              |

***

### 6. **Zusammenfügen, Kombinieren, Erweitern**

| Tool                      | Befehl                              | Beschreibung |
| ------------------------- | ----------------------------------- | ------------ |
| `paste file1 file2`       | Dateien spaltenweise zusammenführen |              |
| `awk '{print $1 "-" $2}'` | Felder kombinieren                  |              |
| `xargs`                   | Übergibt Inhalte an andere Befehle  |              |
| `jq`                      | JSON zerlegen und manipulieren      |              |

***

### 7. **Spezial: Leerzeichen, Zeilen, Zeichen trimmen**

| Tool                            | Befehl                                      | Beschreibung |
| ------------------------------- | ------------------------------------------- | ------------ |
| `xargs`                         | Entfernt führende/nachgestellte Leerzeichen |              |
| `awk '{$1=$1; print}'`          | Entfernt doppelte Leerzeichen               |              |
| `sed 's/^[ \t]*//;s/[ \t]*$//'` | Trimmt vorne/hinten                         |              |
| `tr -s ' '`                     | Ersetzt mehrere Leerzeichen durch eines     |              |

***

### 8. **Komplexere Operationen (Kombiniert)**

```bash
# Nur Namen extrahieren, Leerzeichen entfernen, sortieren
cat res.txt | grep 'sAMAccountName' | cut -d' ' -f2 | tr -d ' ' | sort | uniq
```

```bash
# Nur Werte mit bestimmten Bedingungen extrahieren
awk -F': ' '/sAMAccountName/ && $2 ~ /^[A-Za-z]+$/ {print $2}' res.txt
```

```bash
# JSON-Felder extrahieren
cat data.json | jq '.users[].name'
```

***

### 9. **Beispiele für typische Aufgaben**

#### ✅ Nur Benutzernamen extrahieren:

```bash
grep sAMAccountName res.txt | cut -d' ' -f2
```

#### ✅ Alle Werte nach dem letzten `:` extrahieren:

```bash
sed 's/.*: //'
```

#### ✅ Nur Buchstaben extrahieren (keine Zahlen oder Sonderzeichen):

```bash
sed 's/[^a-zA-Z]//g'
```
