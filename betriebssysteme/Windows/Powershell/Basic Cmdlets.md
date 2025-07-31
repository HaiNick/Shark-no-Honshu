## Grundsyntax: Verb-Nomen

Wie bereits erwähnt, werden PowerShell-Befehle als **Cmdlets** (ausgesprochen "command-lets") bezeichnet. Sie sind viel leistungsfähiger als die traditionellen Windows-Befehle und ermöglichen eine fortgeschrittenere Datenmanipulation.

### Verb-Nomen-Namenskonvention

Cmdlets folgen einer konsistenten **Verb-Nomen**-Namenskonvention. Diese Struktur erleichtert das Verständnis dessen, was jedes Cmdlet tut. Das Verb beschreibt die durchzuführende Aktion, und das Nomen spezifiziert das Objekt, auf das die Aktion ausgeführt wird.

Beispiele für Cmdlets:

- **Get-Content**: Ruft (holt) den Inhalt einer Datei ab und zeigt ihn in der Konsole an.
- **Set-Location**: Ändert (setzt) das aktuelle Arbeitsverzeichnis.

## Grundlegende Cmdlets

Um alle verfügbaren Cmdlets, Funktionen, Aliase und Skripte aufzulisten, die in der aktuellen PowerShell-Sitzung ausgeführt werden können, verwenden wir **Get-Command**. Es ist ein essentielles Werkzeug, um herauszufinden, welche Befehle verfügbar sind und genutzt werden können.

### Beispiel:

  ```powershell
  Get-Command
  ```

Für jedes mit dem Cmdlet **Get-Command** abgerufene **CommandInfo**-Objekt werden einige wesentliche Informationen (Eigenschaften) in der Konsole angezeigt. Es ist möglich, die Liste der Befehle basierend auf den angezeigten Eigenschaftswerten zu filtern. Zum Beispiel, wenn wir nur die verfügbaren Befehle vom Typ „Function“ anzeigen möchten, können wir den Parameter **-CommandType** verwenden:

### Beispiel:

 ```powershell
  Get-Command -CommandType Function
  ```
Dieser Befehl zeigt nur die Cmdlets, die als Funktionen definiert sind.

### Abrufen von Hilfe

Wie in den obigen Ergebnissen gezeigt, informiert **Get-Help** uns, dass wir andere nützliche Informationen über ein Cmdlet abrufen können, indem wir einige Optionen an die grundlegende Syntax anhängen. Zum Beispiel zeigt uns die Verwendung von **-Examples** mit dem obigen Befehl eine Liste von häufigen Anwendungsbeispielen des gewählten Cmdlets.
### Beispiel:

 ```powershell
  Get-Help -Examples Function
  ```
### Verwendung von Aliases

Um den Übergang für IT-Profis zu erleichtern, enthält PowerShell Aliases — Abkürzungen oder alternative Namen für Cmdlets — für viele traditionelle Windows-Befehle. Diese sind unerlässlich für Benutzer, die bereits mit anderen Befehlszeilentools vertraut sind. Der Befehl **Get-Alias** listet alle verfügbaren Aliases auf. Zum Beispiel ist **dir** ein Alias für **Get-ChildItem**, und **cd** ist ein Alias für **Set-Location**.

### Beispiel:

 ```powershell
  Get-Alias
  ```
  Dieser Befehl zeigt eine Liste aller Aliases in der aktuellen PowerShell-Sitzung an.

## Wo man Cmdlets findet und herunterlädt

Eine weitere leistungsstarke Funktion von PowerShell ist die Möglichkeit, ihre Funktionalität durch das Herunterladen zusätzlicher Cmdlets aus Online-Repositorys zu erweitern.

**HINWEIS:** Bitte beachten Sie, dass die in diesem Abschnitt aufgeführten Cmdlets eine funktionierende Internetverbindung erfordern, um Online-Repositorys abzufragen.

### Suchen von Modulen

Um nach Modulen (Sammlungen von Cmdlets) in Online-Repositorys wie der PowerShell Gallery zu suchen, verwenden wir den Befehl **Find-Module**. Manchmal kann es hilfreich sein, nach Modulen mit einem ähnlichen Namen zu suchen, wenn wir den genauen Namen des Moduls nicht kennen. Dies erreichen wir, indem wir die **Name**-Eigenschaft filtern und ein Wildcard-Zeichen (*) an den teilweisen Namen des Moduls anhängen. Die Standard-PowerShell-Syntax dafür lautet:

### Beispiel:

```powershell
Find-Module -Name "pattern*"
```

Dieser Befehl sucht nach Modulen, deren Namen mit „pattern“ beginnen.

### Installieren von Modulen

Sobald die Module identifiziert sind, können sie aus dem Repository mit dem Befehl **Install-Module** heruntergeladen und installiert werden. Die neuen Cmdlets, die im Modul enthalten sind, stehen dann zur Verwendung zur Verfügung.

### Beispiel:

```powershell
Install-Module -Name "ModuleName"
```

Dieser Befehl installiert das angegebene Modul und macht die darin enthaltenen Cmdlets verfügbar.
