## Piping in PowerShell

Piping ist eine Technik, die in Kommandozeilenumgebungen verwendet wird und es ermöglicht, die Ausgabe eines Befehls als Eingabe für einen anderen Befehl zu nutzen. Dadurch entsteht eine Abfolge von Operationen, bei denen die Daten von einem Befehl zum nächsten fließen. Piping wird durch das Symbol | dargestellt und ist sowohl in der Windows-Eingabeaufforderung als auch in Unix-basierten Shells weit verbreitet.

In PowerShell ist Piping noch leistungsfähiger, da es Objekte anstelle von nur Text überträgt. Diese Objekte enthalten nicht nur die Daten, sondern auch die Eigenschaften und Methoden, die die Daten beschreiben und mit ihnen interagieren.
### Beispiel:

  ```powershell
  # Liste eines Ordners und Sortieren nach Größe
  Get-ChildItem | Sort-Object Length
  ```
## Verwendung von Where-Object und Vergleichsoperatoren in PowerShell

Das Cmdlet **Where-Object** filtert die Dateien anhand ihrer **Extension**-Eigenschaft, wodurch sichergestellt wird, dass nur Dateien mit der Erweiterung gleich (-eq) **.txt** aufgelistet werden.

Der Operator **-eq** (d. h. "gleich") ist Teil einer Reihe von Vergleichsoperatoren, die auch in anderen Skriptsprachen (z. B. Bash, Python) verwendet werden. 

- **-ne**: "nicht gleich". Dieser Operator kann verwendet werden, um Objekte aus den Ergebnissen basierend auf festgelegten Kriterien auszuschließen.
- **-gt**: "größer als". Dieser Operator filtert nur Objekte, die einen bestimmten Wert überschreiten. Es ist wichtig zu beachten, dass es sich um einen strengen Vergleich handelt, was bedeutet, dass Objekte, die gleich dem angegebenen Wert sind, von den Ergebnissen ausgeschlossen werden.
- **-ge**: "größer als oder gleich". Dies ist die nicht-strenge Version des vorherigen Operators. Eine Kombination aus **-gt** und **-eq**.
- **-lt**: "weniger als". Wie sein Pendant "größer als" ist dies ein strenger Operator. Er schließt nur Objekte ein, die strikt unter einem bestimmten Wert liegen.
- **-le**: "weniger als oder gleich". Genau wie sein Pendant **-ge** ist dies die nicht-strenge Version des vorherigen Operators. Eine Kombination aus **-lt** und **-eq**.
### Beispiel:
  ```powershell
  # Where-Objekt filtert nach Extension-Property mit Eigenschaft ".txt"
  Get-ChildItem | Where-Object -Property "Extension" -eq ".txt"
  # Filterung mit Wildcard
  Get-ChildItem | Where-Object -Property "Name" like "ship*"
  ```
## Verwendung von Select-Object in PowerShell

Das nächste Filter-Cmdlet, **Select-Object**, wird verwendet, um spezifische Eigenschaften aus Objekten auszuwählen oder die Anzahl der zurückgegebenen Objekte zu begrenzen. Es ist nützlich, um die Ausgabe zu verfeinern und nur die Details anzuzeigen, die benötigt werden.
### Beispiel:
  ```powershell
  # Where-Objekt filtert nach Extension-Property mit Eigenschaft ".txt"
  Get-ChildItem | Select-Object Name,Length
  ```