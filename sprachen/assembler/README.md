# Assembler

Ein einfaches Beispiel eines kompletten x86-64 Assembler-Programms, das zwei Zahlen addiert und das Ergebnis zurückgibt:

```
section .data
    num1 dq 5         ; Erste Zahl (64-Bit)
    num2 dq 10        ; Zweite Zahl (64-Bit)
    result dq 0       ; Speicherplatz für das Ergebnis

section .text
    global _start

_start:
    mov rax, [num1]   ; Lade num1 in rax
    add rax, [num2]   ; Addiere num2 zu rax
    mov [result], rax ; Speichere das Ergebnis in result

    mov rax, 60       ; Systemaufrufnummer für exit
    xor rdi, rdi      ; Rückkehrwert 0
    syscall           ; Aufruf des Kernels

```



* **section .data:** Enthält initialisierte Daten (Variablen und Konstanten).
* **section .text:** Enthält den ausführbaren Code.
* **\_start:** Definiert den Einstiegspunkt des Programms, wo die Ausführung beginnt.

**section .data**

* **Bedeutung:** Dieser Abschnitt definiert den Datensegment des Programms, in dem statische Daten wie Variablen und Konstanten gespeichert werden.
* **Verwendung:** Hier können Daten initialisiert werden, die zur Laufzeit des Programms verfügbar sind.



**section .text**

* **Bedeutung:** Dieser Abschnitt definiert den Textsegment des Programms, der den ausführbaren Code enthält.
* **Verwendung:** Hier wird der eigentliche Programmcode geschrieben, einschließlich der Instruktionen und Funktionsaufrufe.



**\_start**

* **Bedeutung:** Dies ist der Einstiegs- oder Startpunkt des Programms. Wenn das Programm gestartet wird, beginnt die Ausführung hier.
* **Verwendung:** Hier beginnt der Hauptcode des Programms. Es ist der Punkt, an dem das Betriebssystem die Ausführung des Programms startet.



