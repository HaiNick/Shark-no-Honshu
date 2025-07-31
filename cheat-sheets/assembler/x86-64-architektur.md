# x86-64-Architektur

## **Übersicht: Allgemeine Register der x86-64-Architektur mit Funktion und Größen**

| Register   | Name                | Funktion / Verwendung                                      | Größen (zugreifbar als)                          |
| ---------- | ------------------- | ---------------------------------------------------------- | ------------------------------------------------ |
| `rax`      | Accumulator         | Rückgabewert von Funktionen, allgemeine Berechnungen       | `rax` (64), `eax` (32), `ax` (16), `al`/`ah` (8) |
| `rbx`      | Base Register       | Allgemeiner Zweck, bei manchen Operationen erhalten        | `rbx`, `ebx`, `bx`, `bl`/`bh`                    |
| `rcx`      | Counter Register    | Zähler für Schleifen, `rep`-Instruktionen                  | `rcx`, `ecx`, `cx`, `cl`/`ch`                    |
| `rdx`      | Data Register       | Argumentregister, I/O, Multiplikation/Division             | `rdx`, `edx`, `dx`, `dl`/`dh`                    |
| `rsi`      | Source Index        | Quellzeiger bei Speicheroperationen (`movs`, `scas`, ...)  | `rsi`, `esi`, `si`, `sil`                        |
| `rdi`      | Destination Index   | Zielzeiger bei Speicheroperationen                         | `rdi`, `edi`, `di`, `dil`                        |
| `rbp`      | Base Pointer        | Basis des aktuellen Stackframes (lokale Variablenzugriffe) | `rbp`, `ebp`, `bp`, `bpl`                        |
| `rsp`      | Stack Pointer       | Aktuelle Spitze des Stacks                                 | `rsp`, `esp`, `sp`, `spl`                        |
| `rip`      | Instruction Pointer | Adresse der nächsten auszuführenden Instruktion            | Nur `rip` (64) – nicht direkt beschreibbar       |
| `r8`–`r15` | Erweiterte Register | Zusätzliche allgemeine Register, auch für Argumente        | z. B. `r8`, `r8d`, `r8w`, `r8b`                  |

{% hint style="info" %}
**Hinweis zur Argumentübergabe (System V AMD64 ABI)**:

* Erste 6 Integer-Argumente: `rdi`, `rsi`, `rdx`, `rcx`, `r8`, `r9`
* Rückgabe: `rax`
* Gleitkomma-Argumente: `xmm0`–`xmm7`

Alle Register können direkt oder über Teilregister mit 8, 16, 32 oder 64 Bit angesprochen werden, außer `rip`, das nur lesbar ist.
{% endhint %}

***

## **Übersicht: Rückgaberegister wichtiger C-Funktionen in x86-64 (System V AMD64 ABI)**

| Funktion / Operation | Rückgabewert gespeichert in | Bedeutung des Werts                                 |
| -------------------- | --------------------------- | --------------------------------------------------- |
| `scanf(...)`         | `rax`                       | Anzahl erfolgreich gelesener Eingaben (`int`)       |
| `printf(...)`        | `rax`                       | Anzahl ausgegebener Zeichen (`int`)                 |
| `malloc(size)`       | `rax`                       | Zeiger auf allokierten Speicher                     |
| `free(ptr)`          | —                           | Kein Rückgabewert                                   |
| `strcmp(a, b)`       | `eax`                       | Vergleichsergebnis: `<0`, `0`, `>0` (`int`)         |
| `strlen(str)`        | `rax`                       | Länge des Strings ohne Nullterminator (`size_t`)    |
| `read(fd, buf, sz)`  | `rax`                       | Anzahl gelesener Bytes oder `-1` bei Fehler         |
| `write(fd, buf, sz)` | `rax`                       | Anzahl geschriebener Bytes oder `-1` bei Fehler     |
| `open(...)`          | `rax`                       | File Descriptor (`int`) oder `-1` bei Fehler        |
| `close(fd)`          | `rax`                       | `0` bei Erfolg, `-1` bei Fehler                     |
| `exit(code)`         | —                           | Kehrt nicht zurück, beendet das Programm            |
| `time(NULL)`         | `rax`                       | Aktueller Zeitstempel (Sekunden seit Epoch)         |
| `getchar()`          | `eax`                       | Eingelesenes Zeichen (`int`)                        |
| `puts(str)`          | `eax`                       | Nicht-negativer Wert bei Erfolg, negativ bei Fehler |

{% hint style="info" %}
**Hinweise:**

* Rückgabewert liegt immer in `rax` (oder `eax`, wenn 32 Bit), gemäß ABI.
* Bei `strcmp` ist nur die Zahl entscheidend: `0` (gleich), `<0` (kleiner), `>0` (größer).
* Funktionsargumente sind nicht berücksichtigt – nur Rückgabeverhalten.
{% endhint %}

***

## **Übersicht: Syscall-Rückgabewerte in x86-64**

| System Call              | Rückgaberegister | Bedeutung des Rückgabewerts                          |
| ------------------------ | ---------------- | ---------------------------------------------------- |
| `read(fd, buf, size)`    | `rax`            | Gelesene Bytes, `-1` bei Fehler                      |
| `write(fd, buf, size)`   | `rax`            | Geschriebene Bytes, `-1` bei Fehler                  |
| `open(path, flags, ...)` | `rax`            | File Descriptor (`int`), `-1` bei Fehler             |
| `close(fd)`              | `rax`            | `0` bei Erfolg, `-1` bei Fehler                      |
| `exit(status)`           | —                | Kehrt nicht zurück                                   |
| `fork()`                 | `rax`            | `0` für Kind, PID für Elternprozess, `-1` bei Fehler |
| `getpid()`               | `rax`            | PID des aufrufenden Prozesses                        |
| `execve(...)`            | —                | Kehrt nur bei Fehler zurück, sonst ersetzt Prozess   |
| `wait4(...)`             | `rax`            | PID des beendeten Prozesses, `-1` bei Fehler         |
| `mmap(...)`              | `rax`            | Zeiger auf Speicherbereich, `-1` bei Fehler          |
| `munmap(addr, length)`   | `rax`            | `0` bei Erfolg, `-1` bei Fehler                      |
| `brk(addr)`              | `rax`            | Neue Programmbreak (Heap-Ende) oder `-1` bei Fehler  |
| `nanosleep(...)`         | `rax`            | `0` bei Erfolg, `-1` bei Fehler                      |
| `kill(pid, sig)`         | `rax`            | `0` bei Erfolg, `-1` bei Fehler                      |

**Allgemeines Verhalten:**

* Rückgabewert liegt **immer in `rax`**
* Fehlerkennzeichnung: Wenn `rax` ≥ (unsigned `-4095`), dann ist ein Fehler passiert → Fehlernummer = `-rax`
* Kein Rückgabewert bei System Calls, die den Prozess ersetzen oder beenden (`exit`, `execve`)

{% hint style="info" %}
**Hinweis zur Ausführung:**

* Syscalls werden typischerweise durch `syscall`-Instruktion aufgerufen
* Syscall-Nummer: `rax`
* Argumente: `rdi`, `rsi`, `rdx`, `r10`, `r8`, `r9`
{% endhint %}

***

### Übersicht: System Call Argumente & Rückgabeverhalten&#x20;

#### **1. Register-Zuweisung für Syscall-Argumente**

| Argumentposition | Register |
| ---------------- | -------- |
| Syscall-Nummer   | `rax`    |
| 1. Argument      | `rdi`    |
| 2. Argument      | `rsi`    |
| 3. Argument      | `rdx`    |
| 4. Argument      | `r10`    |
| 5. Argument      | `r8`     |
| 6. Argument      | `r9`     |

**Aufruf erfolgt mit `syscall`-Instruktion.**

***

#### **2. Tabelle: Wichtige Syscalls (x86-64 Linux)**

| Syscall (Name) | Syscall-Nr. | Argumente (in Registern)                                                            | Rückgabewert (`rax`)                   |
| -------------- | ----------- | ----------------------------------------------------------------------------------- | -------------------------------------- |
| `read`         | 0           | `rdi` = fd, `rsi` = buf, `rdx` = count                                              | Bytes gelesen, `-errno` bei Fehler     |
| `write`        | 1           | `rdi` = fd, `rsi` = buf, `rdx` = count                                              | Bytes geschrieben, `-errno` bei Fehler |
| `open`         | 2           | `rdi` = filename, `rsi` = flags, `rdx` = mode                                       | FD oder `-errno`                       |
| `close`        | 3           | `rdi` = fd                                                                          | `0` oder `-errno`                      |
| `stat`         | 4           | `rdi` = filename, `rsi` = struct stat\*                                             | `0` oder `-errno`                      |
| `fstat`        | 5           | `rdi` = fd, `rsi` = struct stat\*                                                   | `0` oder `-errno`                      |
| `lseek`        | 8           | `rdi` = fd, `rsi` = offset, `rdx` = whence                                          | neue Position oder `-errno`            |
| `mmap`         | 9           | `rdi` = addr, `rsi` = length, `rdx` = prot, `r10` = flags, `r8` = fd, `r9` = offset | Addr oder `-errno`                     |
| `munmap`       | 11          | `rdi` = addr, `rsi` = length                                                        | `0` oder `-errno`                      |
| `brk`          | 12          | `rdi` = new program break                                                           | Neue Heap-Endadresse oder `-errno`     |
| `rt_sigaction` | 13          | `rdi` = signum, `rsi` = new, `rdx` = old, `r10` = size                              | `0` oder `-errno`                      |
| `clone`        | 56          | `rdi` = flags, `rsi` = stack, `rdx` = ptid, `r10` = ctid, `r8` = tls                | PID oder `-errno`                      |
| `execve`       | 59          | `rdi` = filename, `rsi` = argv, `rdx` = envp                                        | kehrt nie zurück bei Erfolg            |
| `exit`         | 60          | `rdi` = status                                                                      | kehrt nicht zurück                     |
| `wait4`        | 61          | `rdi` = pid, `rsi` = wstatus, `rdx` = options, `r10` = rusage                       | PID oder `-errno`                      |
| `kill`         | 62          | `rdi` = pid, `rsi` = signal                                                         | `0` oder `-errno`                      |
| `getpid`       | 39          | —                                                                                   | PID                                    |
| `nanosleep`    | 35          | `rdi` = req, `rsi` = rem                                                            | `0` oder `-errno`                      |

***

#### Rückgabeverhalten

* **Erfolg**: Rückgabewert in `rax`
* **Fehler**: `rax` enthält **-errno** (negative Fehlernummer, z. B. `-13` für `EACCES`)
* Prüfen durch: `cmp rax, 0`, `js` (jump if sign), `jl`, oder `test rax, rax`
