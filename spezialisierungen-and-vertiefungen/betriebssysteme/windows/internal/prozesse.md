---
description: PID, Sicherheitskontext, Handles
---

# Prozesse

Informationen: [https://docs.microsoft.com/en-us/windows/win32/procthread/about-processes-and-threads](https://docs.microsoft.com/en-us/windows/win32/procthread/about-processes-and-threads)

## Prozesslebenszyklus (Erstellung, Terminierung)

Ein Prozess wird in Windows z. B. durch die WinAPI-Funktion `CreateProcess()` gestartet. Der Kernel erzeugt dabei ein neues `EPROCESS`-Objekt und initialisiert alle zugehörigen Ressourcen.\
Die Beendigung erfolgt durch `ExitProcess()` oder `TerminateProcess()` – letzteres kann auch extern durch andere Prozesse ausgelöst werden.

**Beispiele:**

*   C-Code:

    ```c
    STARTUPINFO si = { sizeof(si) };
    PROCESS_INFORMATION pi;
    CreateProcess(NULL, "notepad.exe", NULL, NULL, FALSE, 0, NULL, NULL, &si, &pi);
    ```
*   CMD:

    ```cmd
    taskkill /PID 1234 /F
    ```

***

## Prozesskontext, Handle-Tabelle

Jeder Prozess besitzt einen eigenen Adressraum und eine Handle-Tabelle, in der Objekte wie Dateien, Pipes oder Events verwaltet werden. Handles können vererbt oder zwischen Prozessen dupliziert werden (`DuplicateHandle()`).

**Beispiel:**

*   PowerShell:

    ```powershell
    Get-Process -Id 1234 | Select-Object Handles
    ```

***

## Benutzer-/Kernel-Modus

Prozesse laufen primär im User Mode. Systemfunktionen, etwa Dateizugriffe oder Registry-Operationen, werden über System Calls in den Kernel Mode übergeben. Die Trennung schützt den Kernel vor fehlerhaften oder bösartigen Benutzercode.

**Beispiel:**

*   API-Call (User-zu-Kernel-Übergang):

    ```c
    HANDLE hFile = CreateFile("test.txt", GENERIC_READ, 0, NULL, OPEN_EXISTING, FILE_ATTRIBUTE_NORMAL, NULL);
    ```

***

## Sicherheits-Token, SID-Zuweisung

Jeder Prozess trägt ein Access Token mit Benutzer-SID, Gruppen, Privilegien und Integrity Level. Dieses Token bestimmt, auf welche Ressourcen der Prozess zugreifen darf.

**Beispiel:**

*   CMD (aktuelles Token anzeigen):

    ```cmd
    whoami /groups
    ```
*   PowerShell (Benutzer-SID des Prozesses):

    ```powershell
    ([System.Security.Principal.WindowsIdentity]::GetCurrent()).User.Value
    ```

***

## Beziehung zu Threads

Ein Prozess kann mehrere Threads enthalten, die parallel laufen. Diese Threads teilen sich Ressourcen wie Speicher, aber besitzen eigene Stacks und Kontextdaten.

**Beispiel:**

*   C-Code (Thread erstellen):

    ```c
    CreateThread(NULL, 0, MyThreadFunc, NULL, 0, NULL);
    ```
*   PowerShell (Thread-Anzahl anzeigen):

    ```powershell
    Get-Process | Select-Object Name,Id,Threads
    ```

***

## Prozess-IDs (PID), Parent-Child-Hierarchien

Jeder Prozess erhält eine eindeutige PID. Der übergeordnete Prozess ist beim Start bekannt, wird aber vom System nicht dauerhaft als Eltern-Kind-Beziehung gepflegt.

**Beispiele:**

*   CMD:

    ```cmd
    tasklist /FI "PID eq 1304"
    ```
*   PowerShell (Prozess mit Parent-PID):

    ```powershell
    Get-CimInstance Win32_Process | Select-Object Name, ProcessId, ParentProcessId
    ```
