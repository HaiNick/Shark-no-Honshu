---
description: Durch pipe | Umleitung in weiteres cmdlet
---

# Filtering / Sorting per Pipe

#### **Allgemeine Beispiele**

```powershell
# Verzeichnis nach Dateigröße sortieren
Get-ChildItem | Sort-Object Length -Descending

# Nur .txt-Dateien anzeigen
Get-ChildItem | Where-Object -Property Extension -eq ".txt"

# Dateien mit Namen beginnend mit "ship"
Get-ChildItem | Where-Object -Property Name -like "ship*"

# Größte Datei anzeigen (Name + Größe)
Get-ChildItem | Sort-Object Length -Descending | Select-Object Name, Length -First 1
```

***

#### **Textbasierte Filterung**

```powershell
# Finde Text "hat" in Datei
Select-String -Path ".\captain-hat.txt" -Pattern "hat"

# Zeilen mit "Adresse" aus ipconfig-Output
ipconfig /all | Where-Object { $_ -like "*Adresse*" }

# Zeilen, die NICHT "fehler" enthalten (case-insensitive)
Get-Content log.txt | Where-Object { $_ -notmatch "fehler" }
```

***

### Parameter-Referenzen

***

#### `Select-Object` – Objekteigenschaften auswählen

| Parameter          | Beschreibung                                |
| ------------------ | ------------------------------------------- |
| `-Property`        | Wählt bestimmte Eigenschaften aus.          |
| `-InputObject`     | Einzelnes Objekt zur Verarbeitung.          |
| `-ExcludeProperty` | Schließt bestimmte Eigenschaften aus.       |
| `-ExpandProperty`  | Gibt nur den Wert einer Eigenschaft zurück. |
| `-First`           | Gibt die ersten _n_ Objekte zurück.         |
| `-Last`            | Gibt die letzten _n_ Objekte zurück.        |
| `-Skip`            | Überspringt _n_ Objekte.                    |
| `-Unique`          | Entfernt doppelte Einträge.                 |
| `-Index` _(PS 7)_  | Wählt Objekte per Index.                    |
| `-Wait` _(PS 7.1)_ | Wartet auf Eingabe aus der Pipeline.        |

***

#### `Sort-Object` – Sortieren von Objekten

| Parameter            | Beschreibung                                          |
| -------------------- | ----------------------------------------------------- |
| `-Property`          | Sortierkriterium (z. B. Name, Größe).                 |
| `-Descending`        | Absteigend sortieren.                                 |
| `-Ascending`         | Aufsteigend (Standard).                               |
| `-Unique`            | Duplikate entfernen.                                  |
| `-Culture`           | Kulturelle Sortierung.                                |
| `-CaseSensitive`     | Groß-/Kleinschreibung beachten.                       |
| `-Stable` _(PS 7.1)_ | Beibehaltung der Originalreihenfolge bei Gleichstand. |

***

#### `Where-Object` – Objekte filtern

| Parameter                    | Beschreibung                        |
| ---------------------------- | ----------------------------------- |
| `-Property`                  | Eigenschaft, auf die geprüft wird.  |
| `-Value`                     | Vergleichswert.                     |
| `-EQ`                        | Gleich (=).                         |
| `-NE`                        | Ungleich (!=).                      |
| `-GT`, `-LT`, `-GE`, `-LE`   | Vergleichsoperatoren (>, <, ≥, ≤).  |
| `-Like` / `-NotLike`         | Wildcard-Vergleich (z. B. `*.txt`). |
| `-Match` / `-NotMatch`       | Regex-Vergleich.                    |
| `-Contains` / `-NotContains` | Wert in Sammlung enthalten?         |
| `-In` / `-NotIn`             | Wert in Liste vorhanden?            |
| `-CaseSensitive` _(PS 7)_    | Groß-/Kleinschreibung beachten.     |
| `-FilterScript`              | Eigener ScriptBlock zur Filterung.  |

***

#### `Select-String` – Text durchsuchen

| Parameter                 | Beschreibung                                |
| ------------------------- | ------------------------------------------- |
| `-Pattern`                | Suchmuster (Text oder Regex).               |
| `-InputObject`            | Direkt übergebener Text.                    |
| `-Path`                   | Datei(en) zum Durchsuchen.                  |
| `-LiteralPath`            | Wie `-Path`, aber ohne Wildcards.           |
| `-Include`                | Nur bestimmte Dateien einbeziehen.          |
| `-Exclude`                | Dateien ausschließen.                       |
| `-CaseSensitive` _(PS 7)_ | Groß-/Kleinschreibung beachten.             |
| `-AllMatches`             | Gibt alle Treffer pro Zeile zurück.         |
| `-NotMatch`               | Gibt Zeilen zurück, die NICHT passen.       |
| `-SimpleMatch`            | Textvergleich ohne Regex.                   |
| `-Quiet`                  | Gibt `$true/$false` zurück.                 |
| `-List`                   | Gibt erste gefundene Zeile je Datei zurück. |
| `-Context`                | Zeilen davor/danach (z. B. `-Context 1,2`). |
| `-Encoding`               | Textkodierung festlegen (UTF8, ASCII etc.). |
| `-Raw` _(PS 7)_           | Gibt rohe Textzeilen zurück.                |
