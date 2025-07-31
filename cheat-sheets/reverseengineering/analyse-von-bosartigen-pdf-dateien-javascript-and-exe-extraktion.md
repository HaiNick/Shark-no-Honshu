# Analyse von bösartigen PDF-Dateien (JavaScript & Exe-Extraktion)

**PDF-Inhaltstypen, die eingebettet sein können:**

* **JavaScript**: Kann beim Öffnen des PDFs ausgeführt werden.
* **Python**: Eingebettete Skripte zur Ausführung.
* **Exe-Dateien**: Versteckte ausführbare Programme.
* **PowerShell-Shellcode**: Kann für Remote-Code-Ausführung verwendet werden.



#### **Analyse von PDFs auf schädlichen Inhalt**

**Schritt 1: Nach eingebettetem JavaScript suchen**

* **Ziel**: JavaScript kann verwendet werden, um zusätzlichen Schadcode herunterzuladen oder den Computer zu manipulieren.
* **Tool**: **peepdf**

**Grundlegende Peepdf-Befehle:**

1.  **Nach JavaScript suchen**:

    ```bash
    peepdf demo_notsuspicious.pdf
    ```

    * Achte auf **OpenAction**, das anzeigt, dass Code automatisch beim Öffnen des PDFs ausgeführt wird.

**Schritt 2: JavaScript extrahieren**

1.  Erstelle ein Skript zur Extraktion von JavaScript:

    ```bash
    echo 'extract js > javascript-from-demo_notsuspicious.pdf' > extracted_javascript.txt
    ```
2.  Führe die Extraktion mit dem erstellten Skript durch:

    ```bash
    peepdf -s extracted_javascript.txt demo_notsuspicious.pdf
    ```
3.  Den extrahierten JavaScript-Code anzeigen:

    ```bash
    cat javascript-from-demo_notsuspicious.pdf
    ```

**Beispielausgabe:**

Der extrahierte Code könnte so aussehen:

```javascript
app.alert("All your Cooctus are belong to us!")
```



#### **Schritt 3: Nach eingebetteten Exe-Dateien suchen**

**Exe-Dateien in PDFs erkennen und extrahieren**

1.  **Nach eingebetteten Dateien suchen**: Um nach eingebetteten Dateien, einschließlich Exe-Dateien, zu suchen, kannst du `peepdf` verwenden, um nach spezifischen Objekten im PDF zu suchen.

    Beispielbefehl:

    ```bash
    peepdf advert.pdf
    ```
2.  **Extrahieren eingebetteter Dateien (z.B. Exe-Dateien)**: Du kannst alle eingebetteten Dateien im PDF mit einem Skript ähnlich wie bei der JavaScript-Extraktion extrahieren. Hier suchst du speziell nach Objekten wie `/EmbeddedFile`.

    Extraktion des Objekts:

    ```bash
    peepdf -s "extract embedded > embedded-files-from-advert.pdf" advert.pdf
    ```

    In diesem Fall wird die eingebettete Exe-Datei oder andere Dateien extrahiert und können weiter analysiert werden.
3.  **Überprüfen der extrahierten Dateien**: Nach der Extraktion kannst du das Ergebnis mit `file` oder `strings` analysieren, um zu prüfen, ob es sich um eine Exe-Datei handelt:

    ```bash
    file embedded-files-from-advert.pdf
    ```

