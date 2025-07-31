---
description: Aufbau von .exe/.dll, Header, Sections
---

# Portable Executable Format

#### Aufbau von PE-Dateien (.exe, .dll)

Das Portable Executable (PE) Format ist das Standard-Dateiformat für ausführbare Dateien und Bibliotheken unter Windows – inklusive `.exe`, `.dll`, `.sys`, `.ocx`. Es basiert auf dem **COFF-Format** (Common Object File Format) und definiert eine klare Struktur für Header, Sections und Verweise auf dynamische Abhängigkeiten.\
Ein PE-File wird direkt vom **Windows Loader** verarbeitet und in den virtuellen Adressraum eines Prozesses geladen.

**Beispiel:**

*   Analyse mit Dumpbin:

    ```cmd
    dumpbin /headers kernel32.dll
    ```

***

#### Header-Struktur (DOS, NT, Optional Header)

Ein PE-File beginnt mit einem **DOS-Header** (`MZ`-Magisches Zeichen), der zu Kompatibilitätszwecken einen DOS-Stub enthält. Darauf folgt das PE-Signaturfeld (`"PE\0\0"`) und der **NT-Header**, bestehend aus:

* **File Header**: Maschinentyp, Anzahl Sections, Zeitstempel
* **Optional Header**: Eintragsadressen (`AddressOfEntryPoint`), Basisadresse, Subsystem, Image-Größe, Datenverzeichnisse (z. B. Import Table)

**Beispiel:**

*   C-Struktur (vereinfacht):

    ```c
    IMAGE_NT_HEADERS {
        Signature;
        IMAGE_FILE_HEADER FileHeader;
        IMAGE_OPTIONAL_HEADER OptionalHeader;
    };
    ```

***

#### Sections (text, data, rsrc)

Nach dem Header folgen die **Section-Header**, die Abschnitte wie `.text` (Code), `.data` (initialisierte Daten), `.bss` (uninitialisierte Daten), `.rdata` (Read-Only), `.rsrc` (Ressourcen wie Icons, Dialoge) oder `.reloc` beschreiben.\
Jede Section hat Informationen zu Offset, Größe, Zugriffsschutz (z. B. `PAGE_EXECUTE_READ`), virtueller Adresse und physischer Position.

**Beispiel:**

* `dumpbin /section:.text user32.dll`

***

#### Relocation, Import Address Table (IAT)

Wenn eine DLL nicht an ihre bevorzugte Adresse geladen werden kann, muss sie **relocated** werden. Die `.reloc`-Section enthält alle Adressen, die angepasst werden müssen.\
Die **Import Address Table (IAT)** enthält Funktionsadressen, die beim Laden durch den Loader oder Delay-Loader gefüllt werden. Diese werden aus der Import Table (`IMAGE_IMPORT_DESCRIPTOR`) aufgelöst.

**Beispiel:**

* `dumpbin /imports shell32.dll`
*   C (Aufruf über IAT):

    ```c
    MessageBoxA(...) // Adresse wurde aus IAT geladen
    ```

***

#### Laden und Initialisieren durch den Windows Loader

Der Windows-Loader (`ntdll!LdrLoadDll`) übernimmt das Parsing der PE-Struktur, das Mapping in den Adressraum und das Auflösen der Imports.\
Anschließend wird – sofern vorhanden – die Initialisierungsroutine `DllMain()` aufgerufen (für DLLs) bzw. der `EntryPoint` bei EXEs.\
TLS-Callbacks (Thread-Local Storage) können vor dem eigentlichen Start ausgeführt werden. Der Loader prüft auch digitale Signaturen und aktiviert Sicherheitstechnologien wie ASLR und DEP.

**Beispiel:**

*   WinDbg (geladenes Modul anzeigen):

    ```cmd
    lm m kernel32
    ```
