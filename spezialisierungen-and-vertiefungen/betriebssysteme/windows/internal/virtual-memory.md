---
description: Adressierung, Page Tables, Speicherschutz
---

# Virtual Memory

## Virtuelle Adressierung

Windows verwendet für jeden Prozess einen eigenen **virtuellen Adressraum**, der vom physischen Speicher abstrahiert ist. Auf 64-Bit-Systemen ist dieser typischerweise 128 TB groß (davon 8 TB nutzbar für Benutzerprozesse). Der Kernel isoliert Prozesse durch getrennte Adressräume – Prozesse können ohne spezielle Mechanismen (z. B. Shared Memory, Pipes) nicht direkt in Speicherbereiche anderer Prozesse zugreifen.\
Die **virtuelle Adresse** wird vom Memory Manager über die MMU in physische Seitenrahmen übersetzt.

**Beispiel:**

*   WinDbg (virtuellen Speicher anzeigen):

    ```cmd
    !address
    ```

***

## Page Tables, Page Faults

Die Speicherzuordnung erfolgt über **Seitentabellen (Page Tables)**. Beim ersten Zugriff auf eine noch nicht im RAM befindliche Seite löst die CPU einen **Page Fault** aus. Der Kernel bedient diesen durch Laden der Seite aus dem Auslagerungsbereich (Pagefile) oder durch Initialisieren einer neuen Seite.\
Es gibt verschiedene Page Fault-Typen: Demand Zero, Soft Fault (RAM), Hard Fault (von Disk). Häufige Hard Faults deuten auf Speichermangel hin.

**Beispiel:**

*   PowerShell (Hard Faults anzeigen):

    ```powershell
    Get-Counter "\Memory\Page Faults/sec"
    ```

***

## Working Sets, Commit/Reserve

Der **Working Set** eines Prozesses ist die Menge der aktuell im RAM gehaltenen Seiten. Der Memory Manager verschiebt Seiten je nach Bedarf zwischen RAM und Pagefile, um Speicher effizient zu nutzen.\
Beim Anfordern von Speicher (z. B. `VirtualAlloc`) unterscheidet Windows zwischen:

* **Reserviertem Speicher**: Adresse blockiert, aber keine physischen Ressourcen zugewiesen.
* **Commit-Speicher**: Physischer RAM oder Pagefile-Speicher ist zugeordnet.

**Beispiele:**

*   C-Code (Reservierung und Commit):

    ```c
    void* ptr = VirtualAlloc(NULL, 4096, MEM_RESERVE | MEM_COMMIT, PAGE_READWRITE);
    ```
*   CMD:

    ```cmd
    tasklist /FI "PID eq 1234" /FO LIST
    ```

***

## Speicherschutz (Memory Protection Flags)

Windows erlaubt pro Speicherbereich definierte Zugriffsrechte, z. B. `PAGE_READONLY`, `PAGE_EXECUTE_READWRITE`. Diese Flags steuern den Zugriff auf Speicherbereiche zur Laufzeit und sind Grundlage für Sicherheitsmechanismen wie DEP (Data Execution Prevention).\
Ein illegaler Zugriff (z. B. Schreiben auf `PAGE_READONLY`) führt zu einem Zugriffsfehler (Access Violation).

**Beispiel:**

*   C-Code:

    ```c
    VirtualProtect(ptr, 4096, PAGE_READONLY, &oldProtect);
    ```

***

## Speicherabbildung (Mapping von DLLs und Dateien)

DLLs, ausführbare Dateien und auch reguläre Dateien können als **Memory-Mapped Files** in den Adressraum eingebunden werden. Dies reduziert Speicherbedarf durch **Shared Pages** und erlaubt schnellen Zugriff ohne explizites Lesen/Schreiben.\
Windows nutzt dies für das **Laden von DLLs** (z. B. `kernel32.dll`) sowie für File-Mapping über `CreateFileMapping()` + `MapViewOfFile()`.

**Beispiele:**

*   CMD (DLLs anzeigen, die ein Prozess geladen hat):

    ```cmd
    tasklist /m /fi "pid eq 1304"
    ```
*   C-Code (Datei ins Memory mappen):

    ```c
    HANDLE hFile = CreateFile("data.bin", GENERIC_READ, 0, NULL, OPEN_EXISTING, 0, NULL);
    HANDLE hMap = CreateFileMapping(hFile, NULL, PAGE_READONLY, 0, 0, NULL);
    LPVOID view = MapViewOfFile(hMap, FILE_MAP_READ, 0, 0, 0);
    ```
