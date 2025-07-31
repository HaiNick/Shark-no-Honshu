# Git

### **Repository verwalten**

```bash
git init                 # Neues Git-Repository initialisieren
git clone <url>          # Repository von Remote klonen
```

***

### **Änderungen verfolgen**

```bash
git status               # Zeigt aktuellen Status der Dateien
git diff                 # Zeigt nicht gestagte Änderungen
git diff --staged        # Zeigt gestagte Änderungen
git add <datei>          # Datei zur Staging-Area hinzufügen
git add .                # Alle Änderungen hinzufügen
git reset <datei>        # Datei wieder aus Staging entfernen
```

***

### **Snapshots erstellen**

```bash
git commit -m "Nachricht"     # Commit erstellen mit Nachricht
git commit -am "Nachricht"    # Add & Commit in einem (nur getrackte Dateien)
```

***

### **Verlauf anzeigen**

```bash
git log                 # Chronologischer Commit-Verlauf
git log --oneline       # Kurzform
git log --graph         # Visualisierte Branch-Struktur
```

***

### **Branches verwalten**

```bash
git branch              # Liste lokaler Branches
git branch <name>       # Neuen Branch erstellen
git checkout <name>     # Zu Branch wechseln
git checkout -b <name>  # Neuer Branch + Wechsel
git merge <branch>      # Branch in aktuellen mergen
git branch -d <name>    # Branch löschen
```

***

### **Remote-Repos**

```bash
git remote -v                # Zeigt Remotes (z. B. origin)
git remote add origin <url> # Remote hinzufügen
git push -u origin main      # Push und Tracking setzen
git pull origin main         # Änderungen holen & mergen
git fetch                    # Nur neue Daten holen (kein Merge)
```

***

### **Fortgeschrittenes**

```bash
git stash                  # Änderungen zwischenspeichern
git stash pop              # Wiederherstellen
git rebase main            # Aktuellen Branch auf neuesten Stand bringen
git cherry-pick <hash>     # Einzelnen Commit übernehmen
git revert <hash>          # Commit rückgängig machen (neuer Commit)
git reset --hard <hash>    # Vorsicht! Setzt Repo hart zurück
git reflog                 # Alle HEAD-Bewegungen anzeigen
```

***

### **.gitignore**

`.gitignore` definiert Dateien, die Git ignorieren soll. Beispiel:

```
# Python
__pycache__/
*.py[cod]
.env

# Node.js
node_modules/
dist/
```

***

### Begrifflichkeiten

**1. Repository (Repo)**\
Ein Repository ist ein Speicherort, in dem der gesamte Quellcode eines Projekts sowie dessen Versionshistorie verwaltet werden. Es kann lokal oder auf einer Plattform wie GitHub gehostet sein.

**2. Commit**\
Ein Commit ist eine Momentaufnahme des aktuellen Projektzustands. Jeder Commit enthält eine Nachricht, die beschreibt, welche Änderungen vorgenommen wurden, sowie einen eindeutigen Hash-Wert.

**3. Branch**\
Ein Branch ist eine unabhängige Entwicklungsreihe innerhalb eines Repositories. Er erlaubt parallele Arbeiten, z. B. für neue Features oder Bugfixes, ohne den Hauptzweig (meist `main` oder `master`) zu beeinträchtigen.

**4. Merge**\
Der Merge-Vorgang integriert Änderungen eines Branches in einen anderen. Häufig wird ein Feature-Branch nach Fertigstellung in den Hauptbranch zurückgeführt.

**5. Pull Request (Merge Request)**\
Ein Pull Request ist eine Anfrage, Änderungen aus einem Branch in einen anderen zu übernehmen. Dies wird oft mit einer Code-Review durch Teammitglieder kombiniert.

**6. Clone**\
Der Befehl `git clone` erstellt eine vollständige Kopie eines entfernten Repositories auf dem lokalen System, einschließlich aller Branches und Commits.

**7. Pull**\
Mit `git pull` werden Änderungen aus einem entfernten Repository abgerufen und in das lokale Repository integriert. Es kombiniert die Befehle `fetch` und `merge`.

**8. Push**\
`git push` überträgt lokale Commits in ein entferntes Repository, um den dortigen Entwicklungsstand zu aktualisieren.

**9. Staging Area (Index)**\
Die Staging Area ist ein Zwischenspeicher für Änderungen, die für den nächsten Commit vorgemerkt wurden. Dateien gelangen mit `git add` dorthin.

**10. Remote**\
Ein Remote ist eine Referenz auf ein entferntes Repository. Typischerweise verweist `origin` auf das Original-Repository, von dem das lokale Repository geklont wurde.

***

### Fortgeschrittene Begrifflichkeiten

#### **1. Rebase**

`git rebase` wird verwendet, um die Basis eines Branches auf einen anderen Commit zu verschieben. Dies erlaubt eine „saubere“ Historie ohne Merge-Commits, birgt jedoch Risiken bei der Bearbeitung veröffentlichter Branches.

**Beispiel:**

```bash
git rebase main
```

***

#### **2. Cherry-Pick**

Mit `git cherry-pick` kann ein einzelner Commit aus einem Branch selektiv in einen anderen übernommen werden, ohne die komplette Branch-Historie zu übernehmen.

**Beispiel:**

```bash
git cherry-pick <commit-hash>
```

***

#### **3. Detached HEAD**

Ein Zustand, in dem der HEAD auf einen bestimmten Commit zeigt und nicht auf einen Branch. Änderungen in diesem Zustand sind flüchtig und sollten in einem Branch gespeichert werden.

***

#### **4. Git Hooks**

Skripte, die bei bestimmten Git-Ereignissen automatisch ausgeführt werden (z. B. `pre-commit`, `post-merge`). Sie ermöglichen die Automatisierung von Prüfungen oder Deployments.

***

#### **5. Git Bisect**

Ein Werkzeug zur Fehlersuche. Mit `git bisect` wird ein Binärsuchverfahren verwendet, um den Commit zu finden, der einen Bug eingeführt hat.

**Beispiel:**

```bash
git bisect start
git bisect bad
git bisect good <commit>
```

***

#### **6. Reflog**

Das Reflog (`git reflog`) protokolliert alle Bewegungen des HEAD – auch solche, die nicht mehr über die normale Commit-Historie nachvollziehbar sind. Es ist ein wertvolles Rettungswerkzeug.

***

#### **7. Submodule**

Ein Submodule ist ein Repository, das innerhalb eines anderen Git-Repositories als Teilprojekt eingebunden wird. Änderungen am Submodule müssen separat verwaltet werden.

***

#### **8. Tag**

Ein Tag ist eine Referenz auf einen spezifischen Commit, häufig zur Markierung von Release-Versionen. Es gibt „lightweight“ und „annotated“ Tags.

**Beispiel:**

```bash
git tag -a v1.0 -m "Release 1.0"
```

***

#### **9. Squash**

Beim „Squashen“ werden mehrere Commits zu einem einzigen zusammengeführt, typischerweise vor einem Merge, um eine übersichtliche Historie zu gewährleisten.

**Beispiel im Rebase-Kontext:**

```bash
git rebase -i HEAD~3
```

***

#### **10. Fast-Forward Merge**

Ein Merge, bei dem der Ziel-Branch einfach „vorgespult“ wird, weil keine divergierenden Commits existieren. Es entsteht kein neuer Merge-Commit.
