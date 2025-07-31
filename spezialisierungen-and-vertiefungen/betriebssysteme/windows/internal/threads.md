---
description: Thread-Kontext, Synchronisation, Zeitplanung
---

# Threads

## Thread-Erstellung, Scheduling

Ein Thread ist die kleinste Ausführungseinheit innerhalb eines Prozesses. Jeder Prozess besitzt mindestens einen primären Thread; zusätzliche Threads können explizit mit `CreateThread()` (WinAPI) oder `_beginthreadex()` (CRT) erzeugt werden.\
Windows verwendet ein **preemptives, prioritätsbasiertes Scheduling**: Der Kernel verwaltet einen Scheduler, der basierend auf Prioritäten und Zeitanteilen entscheidet, welcher Thread auf welcher CPU ausgeführt wird. Multithreading ermöglicht parallele Verarbeitung und bessere Ressourcenausnutzung.

**Beispiele:**

*   C-Code:

    ```c
    DWORD WINAPI ThreadFunc(LPVOID param) { return 0; }
    HANDLE hThread = CreateThread(NULL, 0, ThreadFunc, NULL, 0, NULL);
    ```
*   PowerShell (alle Threads eines Prozesses):

    ```powershell
    Get-Process -Id 1234 | Select-Object -ExpandProperty Threads
    ```

***

## Thread-Kontext und Stack

Jeder Thread besitzt einen eigenen **Stack**, in dem Funktionsaufrufe, lokale Variablen und Rücksprungadressen gespeichert werden. Zusätzlich verwaltet der Kernel für jeden Thread einen **Kontext** (Registerzustände, Instruction Pointer, Stack Pointer usw.), der beim Kontextwechsel (Context Switch) gespeichert und wiederhergestellt wird.\
Der Standard-Stack eines Threads ist 1 MB, kann aber über Parameter verändert werden.

**Beispiel:**

*   WinDbg (Thread-Kontext anzeigen):

    ```cmd
    ~0 kv         ; Stacktrace von Thread 0
    .thread /r    ; Kontext des aktuellen Threads anzeigen
    ```

***

## Thread-Prioritäten und Affinität

Threads besitzen eine Prioritätsstufe (0–31, wobei 8–15 der normale Bereich ist) und können einzelnen CPUs (Cores) durch eine **Prozessor-Affinität** zugewiesen werden. Priorität und Affinität steuern die Reihenfolge und Ort der Ausführung, sind jedoch Empfehlungen – keine Garantien.

**Beispiele:**

*   PowerShell (Thread-Priorität anzeigen):

    ```powershell
    Get-Process | Select-Object Name, Id, PriorityClass
    ```
*   C-Code (Thread auf CPU 0 und 1 binden):

    ```c
    SetThreadAffinityMask(hThread, 0x00000003);  // CPUs 0 und 1
    ```

***

## Synchronisationsmechanismen (Mutex, Semaphore, Events)

Zur Vermeidung von Race Conditions und zur Koordination zwischen Threads stellt Windows verschiedene Synchronisationsobjekte bereit:

* **Mutex**: gegenseitiger Ausschluss, exklusiver Zugriff
* **Semaphore**: Zähler für begrenzten Zugriff (z. B. max. N Threads)
* **Event**: Signalisierung von Zuständen zwischen Threads

Diese Objekte sind kernelbasiert und können auch zwischen Prozessen verwendet werden.

**Beispiel:**

*   C-Code (Mutex verwenden):

    ```c
    HANDLE hMutex = CreateMutex(NULL, FALSE, NULL);
    WaitForSingleObject(hMutex, INFINITE);
    // kritischer Abschnitt
    ReleaseMutex(hMutex);
    ```

***

## Zusammenhang mit Prozessen

Ein Thread kann **nicht unabhängig vom Prozess existieren**. Alle Threads eines Prozesses teilen sich:

* den virtuellen Adressraum
* offene Handles
* globale Variablen (bei statischem Speicher)\
  Threads führen Code des Prozesses aus, können aber durch eigene Parameter oder Thread-Local Storage differenziert werden. Viele Systemaufrufe (z. B. Prozessbeendigung) beeinflussen automatisch alle zugehörigen Threads.

**Beispiele:**

*   CMD (Thread-Anzahl eines Prozesses):

    ```cmd
    tasklist /FI "PID eq 1234" /FO LIST
    ```
*   PowerShell (alle Threads eines Prozesses mit IDs):

    ```powershell
    Get-Process -Id 1234 | ForEach-Object { $_.Threads | Select-Object Id, StartTime }
    ```
