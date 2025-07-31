---
description: Tools, Diagnose, Systemanalyse
---

# Interaktion mit Windows Internals

## Tools und APIs zur Analyse (z. B. Sysinternals, WinDbg)

Windows stellt zahlreiche Tools und APIs zur Verfügung, um interne Zustände und Abläufe sichtbar zu machen.

* **Sysinternals Suite** (z. B. `Process Explorer`, `Process Monitor`, `PsTools`) erlaubt die Analyse von Prozessen, Handles, Registry- und Dateizugriffen auf hoher Abstraktionsebene.
* **WinDbg (Windows Debugger)** bietet Zugriff auf Kernel- und Benutzermodus-Datenstrukturen, Stack-Traces, Speicheranalysen und Treiberinformationen.
* APIs wie `NtQuerySystemInformation()`, `ToolHelp32`, oder `Performance Data Helper (PDH)` ermöglichen eigene Tools oder tiefe Automatisierung.

**Beispiele:**

*   WinDbg:

    ```cmd
    !process 0 0  // Alle Prozesse auflisten
    ```
*   Sysinternals:

    ```cmd
    procexp.exe
    ```

***

## WMI, ETW, Performance Counters

* **WMI (Windows Management Instrumentation)** erlaubt programmgesteuerten Zugriff auf Systeminformationen (Prozesse, Hardware, Services). WMI ist über PowerShell (`Get-WmiObject`, `Get-CimInstance`) und COM/API nutzbar.
* **ETW (Event Tracing for Windows)** ermöglicht performantes Event-Logging in Kernel- und Usermode, u. a. für I/O, Scheduling, Netzwerk, DLL-Ladevorgänge. Tools wie **PerfView**, **xperf**, und `logman` basieren auf ETW.
* **Performance Counters** stellen aggregierte Metriken über Systemzustände bereit (RAM, CPU, Paging, Netzwerke, Prozesse), abrufbar über `perfmon`, PowerShell oder APIs.

**Beispiele:**

*   PowerShell:

    ```powershell
    Get-Counter "\Processor(_Total)\% Processor Time"
    ```
*   ETW-Sitzung starten:

    ```cmd
    logman start trace MyTrace -p "Microsoft-Windows-Kernel-Process" -o trace.etl
    ```

***

## Sicherheits- und Diagnosetechniken

Systeminternes Verhalten lässt sich zur Fehlerdiagnose oder Sicherheitsüberwachung nutzen:

* **Audit Logs**: Über Gruppenrichtlinien kann sicherheitsrelevantes Verhalten (z. B. Logons, Objektzugriffe) mitprotokolliert werden.
* **AppLocker, SRP**: Analyse, welche Binaries ausgeführt werden dürfen.
* **Memory Dumps** (Crash Dumps, Full Dumps) können mit WinDbg analysiert werden – etwa zur Ursachenforschung bei BSODs (Bugchecks).
* **Code Integrity Logs** zeigen Signaturprüfungen und Policy-Eingriffe.
* Stack-Walking und Call Graphs unterstützen Reverse Engineering und Exploit-Analysen.

**Beispiel (Crash Dump Analyse):**

```cmd
windbg -z MEMORY.DMP
!analyze -v
```

***

## Zugriff auf Kernel-Datenstrukturen

Mit WinDbg, Symbolfiles und entsprechender Berechtigung lassen sich intern verwaltete Kernel-Datenstrukturen auslesen (z. B. `EPROCESS`, `ETHREAD`, `HANDLE_TABLE`).\
Dies ist essenziell für Treiberentwickler, Sicherheitsforscher oder Forensiker. Die öffentliche Symbolserver-Infrastruktur (`srv*`) erlaubt symbolbasierten Zugriff.

**Beispiel:**

```cmd
dt nt!_EPROCESS
```

***

## Tasklist, Handle, Process Explorer, etc.

Windows stellt auch im Benutzermodus viele Werkzeuge bereit:

* `tasklist`: listet Prozesse, RAM-Verbrauch, DLLs
* `handle.exe` (Sysinternals): zeigt offene Handles auf Dateien, Registry usw.
* `Process Explorer`: GUI-basierter Task-Manager mit erweiterten Funktionen (Stack-Traces, Zugriff auf Token, Dienstzuordnung)
* `tasklist /m` zeigt Modulzuordnungen (welche DLLs in welchem Prozess geladen sind)
* `taskkill`, `pskill`: Beenden von Prozessen

**Beispiele:**

```cmd
tasklist /fi "imagename eq notepad.exe"
handle -p 1234
tasklist /m /fi "pid eq 1304"
```
