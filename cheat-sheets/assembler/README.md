# Assembler

#### Übersicht: x86-64 Assemblerbefehle im Dump `main()`

| Befehl   | Bedeutung (allgemein)                                | Typischer Aufruf / Syntax |
| -------- | ---------------------------------------------------- | ------------------------- |
| `push`   | Stack: Register auf Stack legen                      | `push reg`                |
| `pop`    | Stack: Register vom Stack holen                      | `pop reg`                 |
| `mov`    | Datenübertragung zwischen Registern/Speicher         | `mov dst, src`            |
| `movabs` | 64-bit absolute Wert in Register laden               | `movabs rax, 0xVALUE`     |
| `lea`    | Adresse berechnen (ohne Zugriff)                     | `lea reg, [addr]`         |
| `sub`    | Subtraktion, hier Stack-Frame anlegen                | `sub rsp, imm`            |
| `call`   | Funktion aufrufen (legt Rücksprungadresse auf Stack) | `call func_addr`          |
| `ret`    | Rückkehr von Funktion                                | `ret`                     |
| `leave`  | Stack-Frame freigeben (`mov rsp, rbp; pop rbp`)      | `leave`                   |
| `test`   | Bitweise UND zweier Operanden, setzt Flags           | `test reg, reg`           |
| `cmp`    | Vergleich (implizit Subtraktion, nur Flags gesetzt)  | `cmp reg1, reg2`          |
| `js`     | Jump if sign (negativ, SF=1)                         | `js label`                |
| `jle`    | Jump if less or equal (ZF=1 or SF≠OF)                | `jle label`               |
| `jne`    | Jump if not equal (ZF=0)                             | `jne label`               |
| `jmp`    | Unbedingter Sprung                                   | `jmp label`               |

***

#### Hinweise zu speziellen Instruktionen im Kontext:

* **`movabs`**: Wird verwendet, um große 64-bit-Konstanten direkt in Register zu laden.
* **`lea`**: Wird häufig zur Berechnung von Speicheradressen verwendet, z. B. für Formatstrings oder Buffer.
* **`call`**: Hier Aufrufe zu `fwrite`, `scanf`, `strcmp`, `printf`.
* **`test eax, eax`**: Prüft, ob `eax == 0` (häufig nach `strcmp` oder Funktionsrückgaben).
* **`leave` / `ret`**: Funktion korrekt verlassen.

***

<table data-full-width="false"><thead><tr><th width="122">Register</th><th width="166">Wert</th><th>Zweck</th></tr></thead><tbody><tr><td>rax</td><td>0x5555555551e9</td><td>Hauptakkumulator, wird häufig für Rückgabewerte von Funktionen und Operationen verwendet.</td></tr><tr><td>rbx</td><td>0x0</td><td>Basisregister, wird oft zur Speicherung von Daten verwendet.</td></tr><tr><td>rcx</td><td>0x555555557d98</td><td>Zählerregister, wird oft für Schleifen- und string-Manipulationsbefehle verwendet.</td></tr><tr><td>rdx</td><td>0x7fffffffdfb8</td><td>Datenregister, häufig für die Ein-/Ausgabe und für Multiplikations- und Divisionsbefehle.</td></tr><tr><td>rsi</td><td>0x7fffffffdfa8</td><td>Quellindex für string-Operationen.</td></tr><tr><td>rdi</td><td>0x1</td><td>Zielindex für string-Operationen.</td></tr><tr><td>rbp</td><td>0x1</td><td>Basiszeiger, wird verwendet, um den aktuellen Stackrahmen zu speichern.</td></tr><tr><td>rsp</td><td>0x7fffffffde98</td><td>Stapelzeiger, zeigt auf die Spitze des aktuellen Stapels.</td></tr><tr><td>r8</td><td>0x7ffff7e1bf10</td><td>Allgemeines Register, zusätzliche Argumente bei Funktionsaufrufen.</td></tr><tr><td>r9</td><td>0x7ffff7fc9040</td><td>Allgemeines Register, zusätzliche Argumente bei Funktionsaufrufen.</td></tr><tr><td>r10</td><td>0x7ffff7fc3908</td><td>Allgemeines Register, häufig verwendet für temporäre Daten.</td></tr><tr><td>r11</td><td>0x7ffff7fde660</td><td>Allgemeines Register, häufig verwendet für temporäre Daten.</td></tr><tr><td>r12</td><td>0x7fffffffdfa8</td><td>Allgemeines Register, häufig zur Speicherung von Daten verwendet.</td></tr><tr><td>r13</td><td>0x5555555551e9</td><td>Allgemeines Register, häufig zur Speicherung von Daten verwendet.</td></tr><tr><td>r14</td><td>0x555555557d98</td><td>Allgemeines Register, häufig zur Speicherung von Daten verwendet.</td></tr><tr><td>r15</td><td>0x7ffff7ffd040</td><td>Allgemeines Register, häufig zur Speicherung von Daten verwendet.</td></tr><tr><td>rip</td><td>0x5555555551e9</td><td>Befehlszeiger, zeigt auf die nächste auszuführende Anweisung.</td></tr><tr><td>eflags</td><td>0x246</td><td>Flags-Register, enthält Status- und Steuerflags.</td></tr><tr><td>cs</td><td>0x33</td><td>Code-Segment-Register, bestimmt das aktuelle Code-Segment.</td></tr><tr><td>ss</td><td>0x2b</td><td>Stack-Segment-Register, bestimmt das aktuelle Stack-Segment.</td></tr><tr><td>ds</td><td>0x0</td><td>Daten-Segment-Register, bestimmt das aktuelle Daten-Segment.</td></tr><tr><td>es</td><td>0x0</td><td>Extra-Segment-Register, häufig für zusätzliche Daten verwendet.</td></tr><tr><td>fs</td><td>0x0</td><td>F-Segment-Register, häufig für Thread-spezifische Daten verwendet.</td></tr><tr><td>gs</td><td>0x0</td><td>G-Segment-Register, häufig für Thread-spezifische Daten verwendet.</td></tr></tbody></table>

<table data-header-hidden data-full-width="false"><thead><tr><th width="96"></th><th width="468"></th><th width="201"></th><th></th></tr></thead><tbody><tr><td><strong>Befehl</strong></td><td><strong>Beschreibung</strong></td><td><strong>Syntax</strong></td><td><strong>Beispiel</strong></td></tr><tr><td><strong>add</strong></td><td>Addiert zwei Operanden und speichert das Ergebnis im Ziel</td><td><code>add Ziel, Quelle</code></td><td><code>add rax, rbx</code></td></tr><tr><td><strong>and</strong></td><td>Führt eine bitweise UND-Operation durch</td><td><code>and Ziel, Quelle</code></td><td><code>and rax, rbx</code></td></tr><tr><td><strong>call</strong></td><td>Ruft eine Unterroutine auf</td><td><code>call Adresse</code></td><td><code>call func</code></td></tr><tr><td><strong>cmp</strong></td><td>Vergleicht zwei Operanden</td><td><code>cmp Ziel, Quelle</code></td><td><code>cmp rax, rbx</code></td></tr><tr><td><strong>dec</strong></td><td>Verringert den Wert eines Operanden um eins</td><td><code>dec Ziel</code></td><td><code>dec rax</code></td></tr><tr><td><strong>div</strong></td><td>Teilt zwei Operanden</td><td><code>div Quelle</code></td><td><code>div rbx</code></td></tr><tr><td><strong>inc</strong></td><td>Erhöht den Wert eines Operanden um eins</td><td><code>inc Ziel</code></td><td><code>inc rax</code></td></tr><tr><td><strong>je</strong></td><td>Springt, wenn gleich (Equal)</td><td><code>je Adresse</code></td><td><code>je label</code></td></tr><tr><td><strong>jg</strong></td><td>Springt, wenn größer (Greater)</td><td><code>jg Adresse</code></td><td><code>jg label</code></td></tr><tr><td><strong>jl</strong></td><td>Springt, wenn kleiner (Less)</td><td><code>jl Adresse</code></td><td><code>jl label</code></td></tr><tr><td><strong>jne</strong></td><td>Springt, wenn ungleich (Not Equal)</td><td><code>jne Adresse</code></td><td><code>jne label</code></td></tr><tr><td><strong>lea</strong></td><td>Lädt die Adresse eines Operanden in ein Register</td><td><code>lea Ziel, [Speicheradresse]</code></td><td><code>lea rax, [rbp-0x10]</code></td></tr><tr><td><strong>mov</strong></td><td>Kopiert Daten von einer Quelle in ein Ziel</td><td><code>mov Ziel, Quelle</code></td><td><code>mov rax, rbx</code></td></tr><tr><td><strong>mul</strong></td><td>Multipliziert zwei Operanden</td><td><code>mul Quelle</code></td><td><code>mul rbx</code></td></tr><tr><td><strong>nop</strong></td><td>Führt keine Operation aus</td><td><code>nop</code></td><td><code>nop</code></td></tr><tr><td><strong>not</strong></td><td>Führt eine bitweise Negation durch</td><td><code>not Ziel</code></td><td><code>not rax</code></td></tr><tr><td><strong>or</strong></td><td>Führt eine bitweise ODER-Operation durch</td><td><code>or Ziel, Quelle</code></td><td><code>or rax, rbx</code></td></tr><tr><td><strong>pop</strong></td><td>Nimmt einen Wert vom Stack</td><td><code>pop Ziel</code></td><td><code>pop rbx</code></td></tr><tr><td><strong>push</strong></td><td>Schiebt einen Wert auf den Stack</td><td><code>push Quelle</code></td><td><code>push rax</code></td></tr><tr><td><strong>ret</strong></td><td>Kehrt von einer Unterroutine zurück</td><td><code>ret</code></td><td><code>ret</code></td></tr><tr><td><strong>sub</strong></td><td>Subtrahiert einen Operanden von einem anderen</td><td><code>sub Ziel, Quelle</code></td><td><code>sub rax, 0x10</code></td></tr><tr><td><strong>xor</strong></td><td>Führt eine bitweise XOR-Operation durch</td><td><code>xor Ziel, Quelle</code></td><td><code>xor rax, rax</code></td></tr><tr><td><strong>leave</strong></td><td>Bereitet den Stack für die Rückkehr aus einer Funktion vor</td><td><code>leave</code></td><td><code>leave</code></td></tr></tbody></table>

### Vergleich: `test` vs `cmp`

| Instruktion | Operation                                   | Effekt auf Flags          | Zweck                   |
| ----------- | ------------------------------------------- | ------------------------- | ----------------------- |
| `cmp A, B`  | `A - B` (Subtraktion, Ergebnis verworfen)   | Setzt ZF, SF, CF, OF usw. | Numerischer Vergleich   |
| `test A, B` | `A & B` (bitweises AND, Ergebnis verworfen) | Setzt ZF, SF, PF          | Bitprüfung, Nullprüfung |

***

### Häufige bedingte Sprungbefehle (nach Flags)

| Mnemonic | Bedeutung         | Sprungbedingung    | Genutzt nach `cmp`? | Genutzt nach `test`? | Bedeutung in C         |
| -------- | ----------------- | ------------------ | ------------------- | -------------------- | ---------------------- |
| `je`     | Jump if Equal     | ZF = 1             | ✅ ja                | ✅ ja                 | `==`                   |
| `jne`    | Jump if Not Equal | ZF = 0             | ✅ ja                | ✅ ja                 | `!=`                   |
| `jz`     | Jump if Zero      | ZF = 1             | ✅ (alias für `je`)  | ✅ (alias für `je`)   | `== 0`                 |
| `jnz`    | Jump if Not Zero  | ZF = 0             | ✅ (alias für `jne`) | ✅ (alias für `jne`)  | `!= 0`                 |
| `jg`     | Jump if Greater   | ZF = 0 and SF = OF | ✅ ja                | ❌ selten             | `>` (signed)           |
| `jge`    | Jump if ≥         | SF = OF            | ✅ ja                | ❌ selten             | `>=` (signed)          |
| `jl`     | Jump if Less      | SF ≠ OF            | ✅ ja                | ❌ selten             | `<` (signed)           |
| `jle`    | Jump if ≤         | ZF = 1 or SF ≠ OF  | ✅ ja                | ❌ selten             | `<=` (signed)          |
| `ja`     | Jump if Above     | CF = 0 and ZF = 0  | ✅ ja                | ❌ selten             | `>` (unsigned)         |
| `jb`     | Jump if Below     | CF = 1             | ✅ ja                | ❌ selten             | `<` (unsigned)         |
| `js`     | Jump if Sign      | SF = 1             | ✅ / ❌ je nach Test  | ✅ ja                 | Wert ist negativ       |
| `jns`    | Jump if Not Sign  | SF = 0             | ✅ / ❌               | ✅ ja                 | Wert ist nicht negativ |

***

### Beispiele

**`cmp`-basiert**

```asm
cmp eax, 5
je equal         ; springt, wenn eax == 5
jl less_than     ; springt, wenn eax < 5
```

**`test`-basiert (häufiger Bit-Check oder Null-Check)**

```asm
test eax, eax
jz zero_case     ; eax == 0
jnz nonzero_case ; eax != 0
```

```asm
test al, 1
jnz lsb_set      ; prüft, ob niedrigstes Bit von al gesetzt ist (z. B. ungerade Zahl)
```

***

### Flags

<table><thead><tr><th width="55" data-type="number">Bit</th><th width="64">Flag</th><th width="189">Bedeutung</th><th width="95">Maske</th><th>Set bei ...</th><th>Setzen/Entfernen durch</th></tr></thead><tbody><tr><td>0</td><td>CF</td><td>Carry Flag</td><td>0x1</td><td>Übertrag bei unsigned Addition/Subtraktion</td><td><p>set $eflags = <strong>$eflags | 0x1</strong></p><p>set $eflags = <strong>$eflags &#x26;~0x1</strong></p></td></tr><tr><td>2</td><td>PF</td><td>Parity Flag</td><td>0x4</td><td>Gerade Anzahl gesetzter Bits (in niedrigsten 8 Bits)</td><td></td></tr><tr><td>4</td><td>AF</td><td>Auxiliary Carry Flag</td><td>0x10</td><td>Übertrag aus Bit 3 bei BCD-Arithmetik</td><td></td></tr><tr><td>6</td><td>ZF</td><td>Zero Flag</td><td>0x40</td><td>Ergebnis ist Null</td><td></td></tr><tr><td>7</td><td>SF</td><td>Sign Flag</td><td>0x80</td><td>Vorzeichen-Bit (MSB) gesetzt (negativ)</td><td></td></tr><tr><td>8</td><td>TF</td><td>Trap Flag</td><td>0x100</td><td>Einzel-Schrittmodus (Debugging)</td><td></td></tr><tr><td>9</td><td>IF</td><td>Interrupt Enable Flag</td><td>0x200</td><td>Maskiert (0) / erlaubt (1) Interrupts</td><td></td></tr><tr><td>11</td><td>OF</td><td>Overflow Flag</td><td>0x800</td><td>Überlauf bei signed Arithmetik</td><td></td></tr></tbody></table>
