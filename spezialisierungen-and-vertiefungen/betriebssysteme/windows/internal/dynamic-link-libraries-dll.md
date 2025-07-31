---
description: Laden, Teilen, Import/Export von gemeinsam genutztem Code
---

# Dynamic Link Libraries (DLL)

## DLL-Import/Export-Tabellen

DLLs stellen Funktionen und Daten über **Export-Tabellen** bereit, die andere Module per **Import-Tabelle** zur Laufzeit referenzieren. Beim Start eines Prozesses wird die Import-Tabelle durch den Windows Loader aufgelöst, wobei die Adressen exportierter Funktionen in die IAT (Import Address Table) eingetragen werden.\
Das Tool `dumpbin /exports` zeigt exportierte Symbole einer DLL.

**Beispiel:**

*   CMD (Exporte anzeigen):

    ```cmd
    dumpbin /exports user32.dll
    ```

***

## Laden von DLLs (LoadLibrary, delay-loading)

Standardmäßig lädt Windows DLLs beim Start des Prozesses (static linking). Alternativ kann zur Laufzeit mit `LoadLibrary()` und `GetProcAddress()` eine DLL dynamisch geladen werden. Dies ermöglicht **plugin-artige Architektur** oder das verzögerte Nachladen (Delay-Loading), bei dem der Loader Funktionen erst beim ersten Aufruf lädt (setzt `__delayLoadHelper2()` ein).

**Beispiele:**

*   C-Code (dynamisches Laden):

    ```c
    HMODULE hLib = LoadLibrary("user32.dll");
    FARPROC msgbox = GetProcAddress(hLib, "MessageBoxA");
    ```

***

#### Getrennte vs. gemeinsame Nutzung

DLLs können sowohl **prozesslokal** (private Kopie im Speicher) als auch **prozessübergreifend** (shared) geladen sein. Der Kernel verwendet **Copy-on-Write**-Mechanismen: Codeabschnitte (PAGE\_EXECUTE\_READ) sind i. d. R. geteilt, während schreibbare Daten pro Prozess repliziert werden.\
Globale Daten in DLLs sind also **nicht automatisch** zwischen Prozessen geteilt – hierfür braucht es Shared Sections oder Memory-Mapped Files.

**Beispiel:**

*   C-Code (globale Variable in DLL ist nicht geteilt):

    ```c
    __declspec(dllexport) int counter = 0;  // Jede Instanz hat eigene Kopie
    ```

***

## Speicherabbildung im Prozessraum

Beim Laden einer DLL wird diese durch den Windows Loader in den virtuellen Adressraum des Prozesses eingebunden (Memory Mapping). Die bevorzugte Basisadresse kann im PE-Header angegeben werden – bei Kollisionen erfolgt Relocation.\
Mittels `VirtualQuery()` oder Tools wie VMMap kann die Platzierung im Speicher sichtbar gemacht werden.

**Beispiel:**

*   C-Code (Basisadresse anzeigen):

    ```c
    HMODULE h = LoadLibrary("kernel32.dll");
    printf("DLL base: %p\n", h);
    ```

***

## Abhängigkeiten zwischen Modulen

DLLs können weitere DLLs importieren – der Loader löst diese Kette rekursiv auf. Ein Fehler in der Auflösung (fehlende DLL, falsche Version) kann zur **Load-Time Error** führen. Tools wie **Dependency Walker (depends.exe)** oder **`ldd` (unter Linux)** helfen beim Aufspüren solcher Ketten.\
Windows schützt vor rekursiven Ladezyklen und verwendet eine interne Liste (`LDR_DATA_TABLE_ENTRY`), um geladene Module zu verwalten.

**Beispiele:**

*   Tool:

    ```cmd
    depends.exe notepad.exe
    ```
*   PowerShell (geladene DLLs auflisten):

    ```powershell
    (Get-Process -Id 1304).Modules | Select-Object ModuleName, FileName
    ```
