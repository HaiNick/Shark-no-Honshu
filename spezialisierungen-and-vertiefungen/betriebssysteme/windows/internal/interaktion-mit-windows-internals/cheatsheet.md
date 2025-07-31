# Cheatsheet

| Kategorie                          | Befehl/Tool / Konzept            | Beschreibung / Nutzung                                                  |
| ---------------------------------- | -------------------------------- | ----------------------------------------------------------------------- |
| **Prozess- und Modulinfo**         | `tasklist /m /fi "pid eq <PID>"` | Zeigt alle geladenen Module (DLLs) des Prozesses mit Prozess-ID `<PID>` |
|                                    | `Process Explorer`               | GUI-Tool, zeigt Prozesse, Threads, Handles, DLLs, Netzwerkverbindungen  |
| **Debugging**                      | `WinDbg`                         | Microsoft Debugger für User- und Kernelmode, Analyse von Crash Dumps    |
|                                    | `cdb`                            | Kommandozeilen-Debugger für tiefgehende Untersuchungen                  |
| **Systeminformationen**            | `systeminfo`                     | Zeigt grundlegende Systemdaten und Konfigurationen                      |
|                                    | `wmic`                           | Windows Management Instrumentation für Abfragen und Steuerung           |
| **Datei- und Registry-Monitoring** | `Process Monitor (ProcMon)`      | Echtzeit-Monitor für Registry, Dateisystem, Prozesse                    |
| **Native API Calls**               | `NtQuerySystemInformation`       | Unoffizielle, native System-API für tiefere Systeminformationen         |
|                                    | `ZwQueryInformationProcess`      | Kernelnahe Abfrage von Prozessinformationen                             |
| **Event Tracing**                  | `logman`                         | Verwaltung von ETW-Sessions (Event Tracing for Windows)                 |
|                                    | `xperf`                          | Leistungsanalyse und detailliertes Tracing                              |
| **Token und Sicherheit**           | `whoami /groups`                 | Zeigt Benutzer- und Gruppeninformationen inkl. SIDs                     |
|                                    | `PsGetSid` (Sysinternals)        | Anzeige von SIDs für Benutzer, Computer und Gruppen                     |
| **Systemsteuerung**                | `services.msc`                   | Verwaltung von Diensten                                                 |
| **Treiber und Kernel**             | `sc query`                       | Abfragen des Status von Windows-Diensten und Treibern                   |
|                                    | `Driver Verifier`                | Überwachung von Treibern zur Fehlersuche                                |
| **Diagnose & Analyse**             | `Event Viewer (eventvwr.msc)`    | Ereignisanzeige für System- und Anwendungsprotokolle                    |
|                                    | `dumpbin /headers <Datei>`       | Zeigt Headerinformationen von PE-Dateien (EXE/DLL)                      |
